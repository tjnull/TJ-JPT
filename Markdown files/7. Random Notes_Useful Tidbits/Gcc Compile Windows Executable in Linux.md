# Gcc Compile Windows Executable in Linux

```
$ sudo apt install mingw-w64
$ x86_64-w64-mingw32-gcc cfile.c -o exefile.exe
```

Reduce filesize, use -s:

```
$ x86_64-w64-mingw32-gcc cfile.c -s -o exefile.exe
```

#gcc