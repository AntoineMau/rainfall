# Source

## Code reconstitu√© :

```C
void o(void)
{
    system("/bin/sh");
    exit(1);
    return;
}

void n(void)
{
    char *str;

    fgets(&str, 512, stdin);
    printf(&str);
    exit(1);
    return;
}

void main(void)
{
    n();
    return;
}
```

## Origine depuis cutter :

```C
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

void n(void)
{
    char *format;

    fgets(&format, 0x200, _stdin);
    printf(&format);
    exit(1);
    n(&stack0xfffffffc);
    return;
}

void main(void)
{
    n();
    return;
}
```
