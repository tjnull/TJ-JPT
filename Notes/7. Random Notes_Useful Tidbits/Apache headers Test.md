# Apache headers Test

```
for i in {1..100}; do curl -sI -X OPTIONS https://site.com|grep -i "allow:"; done
```