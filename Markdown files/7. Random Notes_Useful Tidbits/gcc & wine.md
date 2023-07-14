# gcc & wine

Compiling Linux executible: 

```
gcc -o exploit exploit.c
```

Compiling Windows executible:

```
i686-w64-mingw32-gcc exploit.c -lws2_32 -o exploit.exe
```

Run .exe on Linux:

```
wine exploit.exe x.x.x.x
```


#gcc