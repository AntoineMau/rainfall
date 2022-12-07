# Source

## jsdec

```C
#include <stdint.h>

int32_t main (char ** argv, char ** envp) {
    int32_t var_bp_4h;
    int32_t var_4h;
    int32_t var_10h;
    int32_t var_14h;
    int32_t var_18h;
    int32_t var_1ch;
    if (argv <= 1) {
        exit (1);
    }
    eax = operatornew(unsignedint) (0x6c);
    ebx = eax;
    N::N(int) (ebx, 5);
    eax = operatornew(unsignedint) (0x6c, ebx);
    ebx = eax;
    N::N(int) (ebx, 6);
    var_18h = ebx;
    eax = var_1ch;
    var_14h = var_1ch;
    eax = var_18h;
    var_10h = var_18h;
    eax = envp;
    eax += 4;
    eax = *(eax);
    eax = var_14h;
    N::setAnnotation(char*) (eax, *(eax));
    eax = var_10h;
    eax = *(eax);
    edx = *(eax);
    eax = var_14h;
    eax = var_10h;
    void (*edx)(uint32_t, uint32_t) (eax, var_14h);
    ebx = var_bp_4h;
    return eax;
}

int32_t method_N_N_int (int32_t arg_8h, int32_t arg_ch) {
    /* N::N(int) */
    eax = arg_8h;
    *(eax) = vtable.N.0;
    eax = arg_8h;
    edx = arg_ch;
    *((eax + 0x68)) = edx;
    return eax;
}

int32_t method_N_operator_N (int32_t arg_8h, int32_t arg_ch) {
    /* N::operator-(N&) */
    eax = arg_8h;
    edx = *((eax + 0x68));
    eax = arg_ch;
    eax = *((eax + 0x68));
    ecx = edx;
    ecx -= eax;
    eax = ecx;
    return eax;
}

int32_t method_N_setAnnotation_char (void * s1, const char * s) {
    const void * s2;
    size_t n;
    /* N::setAnnotation(char*) */
    eax = s;
    eax = strlen (eax);
    edx = s1;
    edx += 4;
    eax = s;
    memcpy (edx, eax, s);
    return eax;
}
```

## Ghidra

```C
void main(char **argv, char **envp)
{
void *s1;
undefined4 *arg_8h;
int32_t var_bp_4h;

    if ((int32_t)argv < 2) {
        _exit(1);
    }
    s1 = (void *)operator new(unsigned int)(0x6c);
    N::N(int)((int32_t)s1, 5);
    arg_8h = (undefined4 *)operator new(unsigned int)(0x6c);
    N::N(int)((int32_t)arg_8h, 6);
    N::setAnnotation(char*)(s1, envp[1]);
    (**(code **)*arg_8h)(arg_8h, s1);
    return;

}

void N::N(int)(int32_t arg_8h, int32_t arg_ch)
{
    // N::N(int)
    *(code **)arg_8h = vtable.N.0;
    *(int32_t *)(arg_8h + 0x68) = arg_ch;
    return;
}

int32_t N::operator-(N&)(int32_t arg_8h, int32_t arg_ch)
{
    // N::operator-(N&)
    return *(int32_t *)(arg_8h + 0x68) - *(int32_t *)(arg_ch + 0x68);
}

void N::setAnnotation(char*)(void *s1, char *s)
{
    undefined4 uVar1;

    // N::setAnnotation(char*)
    uVar1 = strlen(s);
    memcpy((int32_t)s1 + 4, s, uVar1);
    return;
}
```
