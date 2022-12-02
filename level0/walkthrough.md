# level0

On observe un executable a la racine `level0`

```bash
$ ./level0
Segmentation fault (core dumped)

$ ./level0 8946153
No !
```

On cherche donc a comprendre ce que fait l'executable

```bash
$ gdb level0 -q
Reading symbols from /home/user/level0/level0...(no debugging symbols found)...done.
(gdb) disas main
Dump of assembler code for function main:
   0x08048ec0 <+0>:    push   %ebp
   0x08048ec1 <+1>:    mov    %esp,%ebp
   0x08048ec3 <+3>:    and    $0xfffffff0,%esp
   0x08048ec6 <+6>:    sub    $0x20,%esp
   0x08048ec9 <+9>:    mov    0xc(%ebp),%eax
   0x08048ecc <+12>:   add    $0x4,%eax
   0x08048ecf <+15>:   mov    (%eax),%eax
   0x08048ed1 <+17>:   mov    %eax,(%esp)
   0x08048ed4 <+20>:   call   0x8049710 <atoi>
   0x08048ed9 <+25>:   cmp    $0x1a7,%eax
   0x08048ede <+30>:   jne    0x8048f58 <main+152>
   0x08048ee0 <+32>:   movl   $0x80c5348,(%esp)
   0x08048ee7 <+39>:   call   0x8050bf0 <strdup>
   0x08048eec <+44>:   mov    %eax,0x10(%esp)
   0x08048ef0 <+48>:   movl   $0x0,0x14(%esp)
   0x08048ef8 <+56>:   call   0x8054680 <getegid>
   0x08048efd <+61>:   mov    %eax,0x1c(%esp)
   0x08048f01 <+65>:   call   0x8054670 <geteuid>
   0x08048f06 <+70>:   mov    %eax,0x18(%esp)
   0x08048f0a <+74>:   mov    0x1c(%esp),%eax
   0x08048f0e <+78>:   mov    %eax,0x8(%esp)
   0x08048f12 <+82>:   mov    0x1c(%esp),%eax
```

On va utiliser `Cutter` pour avoir un idee plus clair de `level0`

```bash
$ scp -P 4242 -r level0@192.168.56.102:/home/user/level0/level0 .

$ ./Cutter-v2.1.2-Linux-x86_64.AppImage level0
```

```c
#include <stdint.h>

int32_t main (char ** envp) {
    int32_t var_4h;
    int32_t var_8h;
    int32_t var_ch;
    int32_t var_10h;
    int32_t var_14h;
    uid_t var_18h;
    int32_t var_1ch;
    eax = envp;
    eax += 4;
    eax = *(eax);
    eax = atoi (eax);
    if (eax == 0x1a7) {
        eax = _strdup ("/bin/sh");
        eax = getegid (eax, 0);
        eax = geteuid (eax);
        eax = var_1ch;
        eax = var_1ch;
        eax = var_1ch;
        _setresgid (eax, eax, var_1ch, var_1ch);
        eax = var_18h;
        eax = var_18h;
        eax = var_18h;
        setresuid (eax, var_18h, var_18h);
        eax = &var_10h;
        execv ("/bin/sh", eax);
    } else {
        eax = *(stderr);
        edx = *(stderr);
        eax = "No !\n";
        _IO_fwrite (eax, edx, 5, 1);
    }
    eax = 0;
    return eax;
}
```

on peut remarquer une comparaison `eax == 0x1a7`, on converti en base 10 :

- <code>0x1a7<sub>16</sub> = 423<sub>10</sub></code>

On essaye donc 423 en parametre de l'executable

```bash
level0:$ ./level0 423

$ cat /home/user/level1/.pass
1fe8a524fa4bec01ca4ea2a869af2a02260d4a7d5fe7e7c24d8617e6dca12d3a

$ exit

level0:$ su level1
Password:

level1:$
```
