# Source

## Code reconstitué :

```C
void v(void)
{
    char *str;

    fgets(&str, 512, stdin);
    printf(&str);
    if (_m == 64) {
        fwrite("Wait what?!\n", 1, 12, stdout);
        system("/bin/sh");
    }
    return;
}

void main(void)
{
    v();
    return;
}
```

## Origine depuis cutter :

```C
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

void main(void)
{
    v();
    return;
}
```
