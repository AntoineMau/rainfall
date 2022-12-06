# Source

Code reconstitué :

```C
int run()
{
    fwrite("Good... Wait what?\n", 1, 19, _stdout);
    system("/bin/sh");
    return(0);
}

void main(void)
{
    char buffer[76];

    gets(buffer);
    return(0);
}
```

Origine depuis cutter :

```C
void run(void)
{
    fwrite("Good... Wait what?\n", 1, 0x13, _stdout);
    system("/bin/sh");
    return;
}

void main(void)
{
    undefined auStack80 [76];

    gets(auStack80);
    return;
}
```
