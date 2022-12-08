# Source

## jsdec

```C
#include <stdint.h>

int32_t main (char ** envp) {
    const void * s2;
    size_t n;
    void * s1;
    unsigned long var_3ch;
    eax = envp;
    eax += 4;
    eax = *(eax);
    eax = atoi (eax);
    var_3ch = eax;
    if (var_3ch > 9) {
        eax = 1;
    } else {
        eax = var_3ch;
        ecx = eax*4;
        eax = envp;
        eax += 8;
        eax = *(eax);
        edx = *(eax);
        eax = &s1;
        memcpy (eax, ecx, edx);
        if (var_3ch == 0x574f4c46) {
            execl ("/bin/sh", 0, 0x8048580);
        }
        eax = 0;
    }
    return eax;
}
```

## Ghidra

```C
undefined4 main(undefined4 placeholder_0, char **envp)
{
    undefined4 uVar1;
    undefined auStack60 [40];
    int32_t iStack20;

    iStack20 = atoi(envp[1]);
    if (iStack20 < 10) {
        memcpy(auStack60, envp[2], iStack20 * 4);
        if (iStack20 == 0x574f4c46) {
            execl("/bin/sh", 0x8048580, 0);
        }
        uVar1 = 0;
    } else {
        uVar1 = 1;
    }
    return uVar1;
}
```
