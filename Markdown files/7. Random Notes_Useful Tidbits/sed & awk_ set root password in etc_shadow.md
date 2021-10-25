# sed & awk: set root password in etc/shadow

You could do it with GNU sed like

`sed -i -e "s/^root:[^:]\+:/root:$pass:/" etc/shadow`

if $pass might have / characters in it you can use a different separator, like , if that won't be there. You can do that like:

`sed -i -e  "s,^root:[^:]\+:,root:$pass:," etc/shadow`

if you do not have GNU sed you may not have -i or it may require an argument.
You could also use awk to do this:

`awk -F: "BEGIN {OFS=FS;} \$1 == \"root\" {\$2=\"$pass\"} 1" etc/shadow`

if you have a recent version of GNU awk you can do it with the -i like so:

`awk -i inplace -F: "BEGIN {OFS=FS;} \$1 == \"root\" {\$2=\"$pass\"} 1" etc/shadow`

From <https://stackoverflow.com/questions/30534693/linux-set-root-password-on-etc-shadow-file> 