# Source

## Code reconstitué

```C
void greetuser(char *Var4)
{
    char *s1;

    if (_language == 1) {
        &s1 = 0xc3767948;
    } else if (_language == 2) {
        &s1 = 0x64656f47;
    } else if (_language == 0) {
        &s1 = 0x6c6c6548;
    }
    strcat(&s1, &Var4);
    puts(&s1);
    return;
}

int main(int argc, char **argv)
{
    char *Var2;
    char *Var4;
    char *Dest1;
    char *Dest2;

    if (argc == 3) {
        strncpy(Dest1, argv[1], 40);
        strncpy(Dest2, argv[2], 32);
        Var2 = getenv("LANG");
        if (Var2 != 0) {
            if (memcmp(Var2, 0x804873d, 2) == 0) {
                _language = 1;
                Var4 = Var2;
            } else {
                Var4 = Var2;
                if (memcmp(Var4, 0x8048740, 2) == 0) {
                    _language = 2;
                } else {
                    _language = 0;
                }
            }
        }
        return greetuser(Var4);
    } else {
        return 1;
    }
}
```

## Origine depuis cutter

```C
undefined4 main(char **argv, char **envp)
{
    undefined4 uVar1;
    char *pcVar2;
    int32_t iVar3;
    undefined4 *puVar4;
    undefined4 *puVar5;
    uint8_t uVar6;
    char *arg_8h;
    undefined4 auStack96 [19];
    char *pcStack20;
    int32_t var_ch;

    uVar6 = 0;
    if (argv == (char **)0x3) {
        puVar4 = auStack96;
        for (iVar3 = 0x13; iVar3 != 0; iVar3 = iVar3 + -1) {
            *puVar4 = 0;
            puVar4 = puVar4 + 1;
        }
        strncpy();
        strncpy();
        arg_8h = "LANG";
        pcVar2 = (char *)getenv();
        pcStack20 = pcVar2;
        if (pcVar2 != (char *)0x0) {
            iVar3 = memcmp();
            if (iVar3 == 0) {
                _language = 1;
                arg_8h = pcVar2;
            } else {
                arg_8h = pcStack20;
                iVar3 = memcmp();
                if (iVar3 == 0) {
                    _language = 2;
                }
            }
        }
        puVar4 = auStack96;
        puVar5 = (undefined4 *)&stack0xffffff50;
        for (iVar3 = 0x13; iVar3 != 0; iVar3 = iVar3 + -1) {
            *puVar5 = *puVar4;
            puVar4 = puVar4 + (uint32_t)uVar6 * -2 + 1;
            puVar5 = puVar5 + (uint32_t)uVar6 * -2 + 1;
        }
        uVar1 = greetuser((int32_t)arg_8h);
    } else {
        uVar1 = 1;
    }
    return uVar1;
}

void greetuser(int32_t arg_8h)
{
    char *s1;
    undefined4 uStack72;
    undefined4 uStack68;
    undefined4 uStack64;
    undefined2 uStack60;
    char cStack58;

    if (_language == 1) {
        s1 = (char *)0xc3767948;
        uStack72 = 0x20a4c3a4;
        uStack68 = 0x69a4c370;
        uStack64 = 0xc3a4c376;
        uStack60 = 0x20a4;
        cStack58 = '\0';
    } else if (_language == 2) {
        s1 = (char *)0x64656f47;
        uStack72 = 0x64696d65;
        uStack68 = 0x21676164;
        uStack64 = CONCAT22(uStack64._2_2_, 0x20);
    } else if (_language == 0) {
        s1 = (char *)0x6c6c6548;
        uStack72 = CONCAT13(uStack72._3_1_, 0x206f);
    }
    strcat(&s1, &arg_8h);
    puts(&s1);
    return;
}
```
