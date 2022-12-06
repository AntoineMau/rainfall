# Level0

On observe un exécutable à la racine : `level0`

```shell
$ ./level0
Segmentation fault (core dumped)

$ ./level0 8946153
No !
```

On cherche donc à comprendre ce que fait l'exécutable

```shell
$ gdb level0 -q
Reading symbols from level0...
(No debugging symbols found in level0)
(gdb) set disassembly-flavor intel
(gdb) disas main
Dump of assembler code for function main:
   0x08048ec0 <+0>:   push   ebp
   0x08048ec1 <+1>:   mov    ebp,esp
   0x08048ec3 <+3>:   and    esp,0xfffffff0
   0x08048ec6 <+6>:   sub    esp,0x20
   0x08048ec9 <+9>:   mov    eax,DWORD PTR [ebp+0xc]
   0x08048ecc <+12>:  add    eax,0x4
   0x08048ecf <+15>:  mov    eax,DWORD PTR [eax]
   0x08048ed1 <+17>:  mov    DWORD PTR [esp],eax
   0x08048ed4 <+20>:  call   0x8049710 <atoi>
   0x08048ed9 <+25>:  cmp    eax,0x1a7
   0x08048ede <+30>:  jne    0x8048f58 <main+152>
   0x08048ee0 <+32>:  mov    DWORD PTR [esp],0x80c5348
   0x08048ee7 <+39>:  call   0x8050bf0 <strdup>
   0x08048eec <+44>:  mov    DWORD PTR [esp+0x10],eax
   0x08048ef0 <+48>:  mov    DWORD PTR [esp+0x14],0x0
   0x08048ef8 <+56>:  call   0x8054680 <getegid>
   0x08048efd <+61>:  mov    DWORD PTR [esp+0x1c],eax
   0x08048f01 <+65>:  call   0x8054670 <geteuid>
   0x08048f06 <+70>:  mov    DWORD PTR [esp+0x18],eax
   0x08048f0a <+74>:  mov    eax,DWORD PTR [esp+0x1c]
   0x08048f0e <+78>:  mov    DWORD PTR [esp+0x8],eax
   0x08048f12 <+82>:  mov    eax,DWORD PTR [esp+0x1c]
```

On va utiliser `Cutter` pour avoir un idee plus clair de `level0`

```shell
$ scp -P 4242 -r level0@192.168.56.102:/home/user/level0/level0 .

$ ./Cutter-v2.1.2-Linux-x86_64.AppImage level0
```

_resultat sauvegardé dans [source.md](source.md)_

On remarque une comparaison `iVar1 == 0x1a7` (`<main+25>` dans l'ASM), ce qui donne en base 10 :

- <code>0x1a7<sub>16</sub> = 423<sub>10</sub></code>

On essaye donc 423 en paramètre de l'exécutable

```shell
level0:$ ./level0 423
$ cat /home/user/level1/.pass
1fe8a524fa4bec01ca4ea2a869af2a02260d4a7d5fe7e7c24d8617e6dca12d3a
$ exit
```
