# Source

## jsdec

```C
#include <stdint.h>

int32_t main (void) {
    p ();
}

int32_t p (int32_t arg_4h) {
    char * src;
    int32_t var_ch;
    int32_t var_4h;
    eax = *(stdout);
    fflush (eax);
    eax = &src;
    gets (eax);
    eax = arg_4h;
    var_ch = arg_4h;
    eax = arg_4h;
    eax &= 0xb0000000;
    if (eax == 0xb0000000) {
        eax = "(%p)\n";
        edx = var_ch;
        var_4h = var_ch;
        printf (eax);
        exit (1);
    }
    eax = &src;
    puts (eax);
    eax = &src;
    strdup (eax);
    return eax;
}
```

## Ghidra

```C
void main(void)
{
    p();
    return;
}

void p(void)
{
    uint32_t unaff_retaddr;
    char *src;
    int32_t var_ch;

    fflush(_stdout);
    gets(&src);
    if ((unaff_retaddr & 0xb0000000) == 0xb0000000) {
        printf("(%p)\n", unaff_retaddr);
        _exit(1);
    }
    puts(&src);
    strdup(&src);
    return;
}
```
