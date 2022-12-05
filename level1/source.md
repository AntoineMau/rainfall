# Source

## jsdec

```C
#include <stdint.h>

int32_t main (void) {
    char * s;
    eax = &s;
    gets (eax);
    return eax;
}
```

## Ghidra

```C
void main(void)
{
    undefined auStack80 [76];

    gets(auStack80);
    return;
}
```
