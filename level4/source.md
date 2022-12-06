# Source

## jsdec

```C
#include <stdint.h>

int32_t main (void) {
    n ();
}

int32_t n (void) {
    char * s;
    int32_t size;
    FILE * stream;
    eax = *(stdin);
    eax = &s;
    fgets (eax, *(stdin), 0x200);
    eax = &s;
    p (eax);
    eax = m;
    if (eax == 0x1025544) {
        system ("/bin/cat /home/user/level5/.pass");
    }
    return eax;
}

int32_t p (const char * format) {
    eax = format;
    printf (eax);
    return eax;
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
    char *s;

    fgets(&s, 0x200, _stdin);
    p((char *)&s);
    if (_m == 0x1025544) {
        system("/bin/cat /home/user/level5/.pass");
    }
    return;
}

void p(char *format)
{
    printf(format);
    return;
}
```
