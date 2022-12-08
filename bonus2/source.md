# Source

## jsdec

```C
#include <stdint.h>

int32_t main (char ** argv, char ** envp) {
    int32_t var_ch;
    const char * src;
    size_t n;
    char * dest;
    char * s1;
    if (argv != 3) {
        eax = 1;
        goto label_0;
    }
    ebx = &dest;
    eax = 0;
    edx = 0x13;
    edi = ebx;
    ecx = edx;
    memset (edi, eax, ecx);
    eax = envp;
    eax += 4;
    eax = *(eax);
    eax = &dest;
    strncpy (eax, 0x28, eax);
    eax = envp;
    eax += 8;
    eax = *(eax);
    eax = &dest;
    eax += 0x28;
    strncpy (eax, 0x20, eax);
    eax = getenv ("LANG");
    s1 = eax;
    if (s1 != 0) {
        eax = s1;
        eax = memcmp (eax, 2, 0x804873d);
        if (eax == 0) {
            *(language) = 1;
        } else {
            eax = s1;
            eax = memcmp (eax, 2, 0x8048740);
            if (eax != 0) {
                goto label_1;
            }
            *(language) = 2;
        }
    }
label_1:
    edx = esp;
    ebx = &dest;
    eax = 0x13;
    edi = edx;
    esi = ebx;
    ecx = eax;
    do {
        *(es:edi) = *(esi);
        ecx--;
        esi += 4;
        es:edi += 4;
    } while (ecx != 0);
    greetuser ();
label_0:
    esp = &var_ch;
    return eax;
}

int32_t greetuser (int32_t arg_8h) {
    char * s1;
    const char * s2;
    eax = language;
    if (eax != 1) {
        if (eax != 2) {
            if (eax != 0) {
                goto label_0;
            }
            edx = "Hello ";
            eax = &s1;
            ecx = *(edx);
            *(eax) = ecx;
            ecx = *((edx + 4));
            *((eax + 4)) = cx;
            edx = *((edx + 6));
            *((eax + 6)) = dl;
            edx = "Hyv\u00e4\u00e4 p\u00e4iv\u00e4\u00e4 ";
            eax = &s1;
            ecx = *(edx);
            *(eax) = ecx;
            ecx = *((edx + 4));
            *((eax + 4)) = ecx;
            ecx = *((edx + 8));
            *((eax + 8)) = ecx;
            ecx = *((edx + 0xc));
            *((eax + 0xc)) = ecx;
            ecx = *((edx + 0x10));
            *((eax + 0x10)) = cx;
            edx = *((edx + 0x12));
            *((eax + 0x12)) = dl;
        } else {
        } else {
            edx = "Goedemiddag! ";
            eax = &s1;
            ecx = *(edx);
            *(eax) = ecx;
            ecx = *((edx + 4));
            *((eax + 4)) = ecx;
            ecx = *((edx + 8));
            *((eax + 8)) = ecx;
            edx = *((edx + 0xc));
            *((eax + 0xc)) = dx;
        }
    }
label_0:
    eax = &arg_8h;
    eax = &s1;
    strcat (eax, eax);
    eax = &s1;
    puts (eax);
    return eax;
}
```

## Ghidra

```C
undefined4 main(char **argv, char **envp)
{
    undefined4 uVar1;
    char *pcVar2;
    int32_t iVar3;
    undefined4 *puVar4;
    undefined4 *puVar5;
    uint8_t uVar6;
    char *arg_8h;
    undefined4 auStack96 [19];
    char *pcStack20;
    int32_t var_ch;

    uVar6 = 0;
    if (argv == (char **)0x3) {
        puVar4 = auStack96;
        for (iVar3 = 0x13; iVar3 != 0; iVar3 = iVar3 + -1) {
            *puVar4 = 0;
            puVar4 = puVar4 + 1;
        }
        strncpy();
        strncpy();
        arg_8h = "LANG";
        pcVar2 = (char *)getenv();
        pcStack20 = pcVar2;
        if (pcVar2 != (char *)0x0) {
            iVar3 = memcmp();
            if (iVar3 == 0) {
                _language = 1;
                arg_8h = pcVar2;
            } else {
                arg_8h = pcStack20;
                iVar3 = memcmp();
                if (iVar3 == 0) {
                    _language = 2;
                }
            }
        }
        puVar4 = auStack96;
        puVar5 = (undefined4 *)&stack0xffffff50;
        for (iVar3 = 0x13; iVar3 != 0; iVar3 = iVar3 + -1) {
            *puVar5 = *puVar4;
            puVar4 = puVar4 + (uint32_t)uVar6 * -2 + 1;
            puVar5 = puVar5 + (uint32_t)uVar6 * -2 + 1;
        }
        uVar1 = greetuser((int32_t)arg_8h);
    } else {
        uVar1 = 1;
    }
    return uVar1;
}

void greetuser(int32_t arg_8h)
{
    char *s1;
    undefined4 uStack72;
    undefined4 uStack68;
    undefined4 uStack64;
    undefined2 uStack60;
    char cStack58;

    if (_language == 1) {
        s1 = (char *)0xc3767948;
        uStack72 = 0x20a4c3a4;
        uStack68 = 0x69a4c370;
        uStack64 = 0xc3a4c376;
        uStack60 = 0x20a4;
        cStack58 = '\0';
    } else if (_language == 2) {
        s1 = (char *)0x64656f47;
        uStack72 = 0x64696d65;
        uStack68 = 0x21676164;
        uStack64 = CONCAT22(uStack64._2_2_, 0x20);
    } else if (_language == 0) {
        s1 = (char *)0x6c6c6548;
        uStack72 = CONCAT13(uStack72._3_1_, 0x206f);
    }
    strcat(&s1, &arg_8h);
    puts(&s1);
    return;
}
```
