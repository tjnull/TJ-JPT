## Debian:

- ls -alh /usr/bin/
- ls -alh /sbin/
- dpkg -l
- ls -alh /var/cache/apt/archivesO
- ls /usr/share/applications | awk -F '.desktop' ' { print $1}' -

## RedHat:

- rpm -qa
- ls -alh /var/cache/yum/


## BSD:

- pkg_info

## Gentoo:

- equery list 
- eix -I

## Arch Linux:

- pacman -Q


## Bash Script:

```
#!/bin/bash
IFS=: read -ra dirs_in_path <<< "$PATH"

for dir in "${dirs_in_path[@]}"; do
    for file in "$dir"/*; do
        [[ -x $file && -f $file ]] && printf '%s\n' "${file##*/}"
    done
done
```