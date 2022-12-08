# Source

## Code reconstitué :

```C
void p(char *str)
{
    printf(str);
    return;
}

void n(void)
{
    char *str;

    fgets(&str, 512, _stdin);
    p(&str);
    if (_m == 0x1025544) {
        system("/bin/cat /home/user/level5/.pass");
    }
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
void p(char *format)
{
    printf(format);
    return;
}

void n(void)
{
    char *s;

    fgets(&s, 0x200, stdin);
    p((char *)&s);
    if (_m == 0x1025544) {
        system("/bin/cat /home/user/level5/.pass");
    }
    return;
}

void main(void)
{
    n();
    return;
}
```
