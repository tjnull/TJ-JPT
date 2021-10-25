# Filter out permission denied errors from find command

```
find / -name fileName 2>&1 | grep -v "Permission denied"
```
From <https://unix.stackexchange.com/questions/42841/how-to-skip-permission-denied-errors-when-running-find-in-linux> 
