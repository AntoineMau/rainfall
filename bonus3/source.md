# Source

## Code reconstitué

```C
int main(int argc, char **argv)
{
    int Var1;
    char Var2[16];
    char Var3[66];
    FILE *file;

    file = fopen("/home/user/end/.pass", 0x80486f0);
    if ((file == 0) || (argc != 2)) {
        Var1 = 1;
    } else {
        fread(Var2, 1, 66, file);
        Var2[atoi(argv[1])] = "\0";
        fread(Var3, 1, 65, file);
        fclose(file);
        if (strcmp(Var2, argv[1]) == 0) {
            execl("/bin/sh", 0x8048707, 0);
        } else {
            puts(Var3);
        }
        Var1 = 0;
    }
    return Var1;
}
```

## Origine depuis cutter

```C
undefined4 main(char **argv, char **envp)
{
    undefined4 uVar1;
    int32_t iVar2;
    undefined4 *puVar3;
    uint8_t uVar4;
    undefined4 auStack152 [16];
    undefined uStack87;
    undefined auStack86 [66];
    int32_t iStack20;
    int32_t var_8h;

    uVar4 = 0;
    iStack20 = fopen("/home/user/end/.pass", 0x80486f0);
    puVar3 = auStack152;
    for (iVar2 = 0x21; iVar2 != 0; iVar2 = iVar2 + -1) {
        *puVar3 = 0;
        puVar3 = puVar3 + (uint32_t)uVar4 * -2 + 1;
    }
    if ((iStack20 == 0) || (argv != (char **)0x2)) {
        uVar1 = 0xffffffff;
    } else {
        fread(auStack152, 1, 0x42, iStack20);
        uStack87 = 0;
        iVar2 = atoi(envp[1]);
        *(undefined *)((int32_t)auStack152 + iVar2) = 0;
        fread(auStack86, 1, 0x41, iStack20);
        fclose(iStack20);
        iVar2 = strcmp(auStack152, envp[1]);
        if (iVar2 == 0) {
            execl("/bin/sh", 0x8048707, 0);
        } else {
            puts(auStack86);
        }
        uVar1 = 0;
    }
    return uVar1;
}
```
