# Source

## Code reconstitué

```CPP
class N {
  public:

  void N(int) {
    this->n = n
  }

  int operator+(N *param) {
    return (this->n + param->n)
  }

  int operator-(N *param) {
    return (this->n + param->n)
  }

  void setAnnotation(char *s) {
    memcpy(this.s, s, strlen(s));
    return
  }
}

void main(int argc, char **argv)
{
    if (argc < 2) {
        exit(1);
    }
    N *class1 = new N(5)
    N *class2 = new N(6)
    class1->setAnnotation(argv[1])
    class2->operator+(class1)
    return;
}
```

## Origine depuis cutter

```CPP
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
