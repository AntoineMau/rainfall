# Source

## Code reconstitué :

```C
void m(void)
{
    puts("Nope");
    return;
}

void n(void)
{
    system("/bin/cat /home/user/level7/.pass");
    return;
}

void main(int argz, char **argv)
{
    var_1 = malloc(60);
    var_2 = (code **)malloc(4);
    *var_2 = m;
    strcpy(var_1, argv[1]);
    (**var_2)();
    return;
}
```

## Origine depuis cutter :

```C
void m(void)
{
    puts("Nope");
    return;
}

void n(void)
{
    system("/bin/cat /home/user/level7/.pass");
    return;
}

void main(undefined4 placeholder_0, char **envp)
{
    undefined4 uVar1;
    code **ppcVar2;

    uVar1 = malloc(0x40);
    ppcVar2 = (code **)malloc(4);
    *ppcVar2 = m;
    strcpy(uVar1, envp[1]);
    (**ppcVar2)();
    return;
}
```
