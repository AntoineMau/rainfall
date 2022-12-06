# Source

## jsdec

```C
#include <stdint.h>

int32_t main (void) {
    n ();
}

int32_t n (void) {
    char * format;
    int32_t size;
    FILE * stream;
    eax = *(stdin);
    eax = &format;
    fgets (eax, *(stdin), 0x200);
    eax = &format;
    printf (eax);
    return exit (1);
}

void o (void) {
    system ("/bin/sh");
    return exit (1);
}
```

## Ghidra

```C
void main(void)
{
    n();
    return;
}

void n(void)
{
    char *format;

    fgets(&format, 0x200, _stdin);
    printf(&format);
    exit(1);
    n(&stack0xfffffffc);
    return;
}

void o(void)
{
    undefined auStack552 [520];
    code *pcStack32;
    char *pcStack28;

    pcStack28 = "/bin/sh";
    pcStack32 = (code *)0x80484b6;
    system();
    pcStack28 = (char *)0x1;
    pcStack32 = n;
    _exit();
    pcStack32 = (code *)&stack0xfffffffc;
    fgets(auStack552, 0x200, _stdin);
    printf(auStack552);
    exit(1);
    n();
    return;
}
```
