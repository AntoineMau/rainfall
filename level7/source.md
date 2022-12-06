# Source

## jsdec

```C
#include <stdint.h>

int32_t main (char ** envp) {
    const char * size;
    FILE * stream;
    void ** var_18h;
    void ** var_1ch;
    eax = malloc (8);
    *(eax) = 1;
    eax = malloc (8, eax);
    edx = eax;
    eax = var_1ch;
    *((eax + 4)) = edx;
    eax = malloc (8);
    *(eax) = 2;
    eax = malloc (8, eax);
    edx = eax;
    eax = var_18h;
    *((eax + 4)) = edx;
    eax = envp;
    eax += 4;
    eax = *(eax);
    edx = *(eax);
    eax = var_1ch;
    eax = *((eax + 4));
    strcpy (eax, edx);
    eax = envp;
    eax += 8;
    eax = *(eax);
    edx = *(eax);
    eax = var_18h;
    eax = *((eax + 4));
    strcpy (eax, edx);
    edx = 0x80486e9;
    eax = "/home/user/level8/.pass";
    eax = fopen (eax, edx);
    fgets (c, eax, 0x44);
    puts (0x8048703);
    eax = 0;
    return eax;
}

uint32_t m (void) {
    int32_t var_4h;
    time_t var_8h;
    eax = time (0);
    edx = "%s - %d\n";
    var_8h = eax;
    var_4h = c;
    printf (edx);
    return eax;
}
```

## Ghidra

```C
undefined4 main(undefined4 placeholder_0, char **envp)
{
    undefined4 *puVar1;
    undefined4 uVar2;
    undefined4 *puVar3;

    puVar1 = (undefined4 *)malloc(8);
    *puVar1 = 1;
    uVar2 = malloc(8);
    puVar1[1] = uVar2;
    puVar3 = (undefined4 *)malloc(8);
    *puVar3 = 2;
    uVar2 = malloc(8);
    puVar3[1] = uVar2;
    strcpy(puVar1[1], envp[1]);
    strcpy(puVar3[1], envp[2]);
    uVar2 = fopen("/home/user/level8/.pass", 0x80486e9);
    fgets(c, 0x44, uVar2);
    puts(0x8048703);
    return 0;
}

void m(void)
{
    undefined4 uVar1;

    uVar1 = time(0);
    printf("%s - %d\n", c, uVar1);
    return;
}
```
