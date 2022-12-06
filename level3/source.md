# Source

## jsdec

```C
#include <stdint.h>

int32_t main (void) {
    v ();
}

int32_t v (void) {
    char * format;
    int32_t size;
    FILE * nitems;
    FILE * stream;
    eax = *(stdin);
    eax = &format;
    fgets (eax, *(stdin), 0x200);
    eax = &format;
    printf (eax);
    eax = m;
    if (eax == 0x40) {
        eax = *(stdout);
        edx = *(stdout);
        eax = "Wait what?!\n";
        fwrite (eax, edx, 0xc, 1);
        system ("/bin/sh");
    }
    return eax;
}
```

## Ghidra

```C
void main(void)
{
    v();
    return;
}

void v(void)
{
    char *format;

    fgets(&format, 0x200, _stdin);
    printf(&format);
    if (_m == 0x40) {
        fwrite("Wait what?!\n", 1, 0xc, _stdout);
        system("/bin/sh");
    }
    return;
}
```
