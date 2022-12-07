# Source

Code reconstitué :

```C
void m(void)
{
    int time;

    time = time(0);
    printf("%s - %d\n", c, time);
    return;
}

int main(int argc, char **argv)
{
    var_1 = malloc(8);
    *var_1 = 1;

    var_2 = malloc(8);
    var_1[1] = var_2;

    var_3 = malloc(8);
    *var_3 = 2;

    var_2 = malloc(8);
    var_3[1] = var_2;

    strcpy(var_1[1], argv[1]);
    strcpy(var_3[1], argv[2]);

    var_2 = fopen("/home/user/level8/.pass", "r");
    fgets(c, 68, var_2);
    puts("~~");

    return 0;
}
```

Origine depuis cutter :

```C
void m(void)
{
    undefined4 uVar1;

    uVar1 = time(0);
    printf("%s - %d\n", c, uVar1);
    return;
}

undefined4 main(undefined4 placeholder_0, char **envp)
{
    undefined4 *puVar1;
    undefined4 uVar2;
    undefined4 *puVar3;

    puVar1 = (undefined4 *)malloc(8);
    *puVar1 = 1;
    uVar2 = malloc(8);
    puVar1[1] = uVar2;
    puVar3 = (undefined4 *)malloc(8);
    *puVar3 = 2;
    uVar2 = malloc(8);
    puVar3[1] = uVar2;
    strcpy(puVar1[1], envp[1]);
    strcpy(puVar3[1], envp[2]);
    uVar2 = fopen("/home/user/level8/.pass", 0x80486e9);
    fgets(c, 0x44, uVar2);
    puts(0x8048703);
    return 0;
}
```
