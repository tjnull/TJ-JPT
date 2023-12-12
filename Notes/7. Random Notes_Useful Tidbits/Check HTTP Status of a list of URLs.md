# Check HTTP Status of a list of URLs 

`xargs -a urls.txt -L 1 -i sh -c 'url={}; curl -k -s -I $url > /dev/null && echo "[UP] $url"'`

Parallelize it by adding `-P 10`

`xargs -a urls.txt -L 1 -i -P 10 sh -c 'url={}; curl -k -s -I $url > /dev/null && echo "[UP] $url"'`