# Source

## jsdec

```C
#include <stdint.h>

int32_t main (char ** envp) {
    const char * src;
    void * var_18h;
    char * dest;
    eax = malloc (0x40);
    eax = malloc (4, eax);
    var_18h = eax;
    edx = m;
    eax = var_18h;
    *(eax) = edx;
    eax = envp;
    eax += 4;
    eax = *(eax);
    edx = *(eax);
    eax = dest;
    strcpy (eax, edx);
    eax = var_18h;
    eax = *(eax);
    void (*eax)() ();
    return eax;
}

void m (void) {
    puts ("Nope");
}

void n (void) {
    system ("/bin/cat /home/user/level7/.pass");
}

```

## Ghidra

```C
void main(undefined4 placeholder_0, char **envp)
{
    undefined4 uVar1;
    code **ppcVar2;

    uVar1 = malloc(0x40);
    ppcVar2 = (code **)malloc(4);
    *ppcVar2 = m;
    strcpy(uVar1, envp[1]);
    (**ppcVar2)();
    return;
}

void m(void)
{
    puts("Nope");
    return;
}

void n(void)
{
    system("/bin/cat /home/user/level7/.pass");
    return;
}

```
