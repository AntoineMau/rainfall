# Source

## jsdec

```C
#include <stdint.h>

int32_t main (void) {
    const char * s;
    eax = &s;
    pp (eax);
    eax = &s;
    puts (eax);
    eax = 0;
    return eax;
}

uint32_t pp (char * dest) {
    int32_t var_3ch;
    int32_t var_30h;
    int32_t var_1ch;
    const char * src;
    eax = &var_30h;
    p (ebx, edi, 0x80486a0);
    eax = &var_1ch;
    p (eax, 0x80486a0);
    eax = &var_30h;
    eax = dest;
    strcpy (eax, eax);
    ebx = 0x80486a4;
    eax = dest;
    var_3ch = 0xffffffff;
    edx = eax;
    eax = 0;
    ecx = var_3ch;
    edi = edx;
    __asm ("repne scasb al, byte es:[edi]");
    eax = ecx;
    eax = ~eax;
    eax--;
    eax += dest;
    edx = *(ebx);
    *(eax) = dx;
    eax = &var_1ch;
    eax = dest;
    strcat (eax, eax);
    return eax;
}

int32_t p (char * dest, const char * s) {
    const char * var_1008h;
    const char * src;
    size_t nbyte;
    eax = s;
    puts (eax);
    eax = &var_1008h;
    read (0, 0x1000, eax);
    eax = &var_1008h;
    strchr (eax, 0xa);
    *(eax) = 0;
    eax = &var_1008h;
    eax = dest;
    strncpy (eax, 0x14, eax);
    return eax;
}
```

## Ghidra

```C
undefined4 main(void)
{
char acStack58 [54];

    pp(acStack58);
    puts(acStack58);
    return 0;

}

void pp(char *dest)
{
    char cVar1;
    uint32_t uVar2;
    char *pcVar3;
    uint8_t uVar4;
    int32_t var_3ch;
    int32_t var_30h;
    int32_t var_1ch;

    uVar4 = 0;
    p((char *)&var_30h, " - ");
    p((char *)&var_1ch, " - ");
    strcpy(dest, &var_30h);
    uVar2 = 0xffffffff;
    pcVar3 = dest;
    do {
        if (uVar2 == 0) break;
        uVar2 = uVar2 - 1;
        cVar1 = *pcVar3;
        pcVar3 = pcVar3 + (uint32_t)uVar4 * -2 + 1;
    } while (cVar1 != '\0');
    *(undefined2 *)(dest + (~uVar2 - 1)) = 0x20;
    strcat(dest, &var_1ch);
    return;
}

void p(char *dest, char *s)
{
    undefined *puVar1;
    char *var_1008h;

    puts(s);
    read(0, &var_1008h, 0x1000);
    puVar1 = (undefined *)strchr(&var_1008h, 10);
    *puVar1 = 0;
    strncpy(dest, &var_1008h, 0x14);
    return;
}
```
