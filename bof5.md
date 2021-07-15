# Bof5 ����

## bof5.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#define BUF_SIZE 128
#define KEY 0x12345678
#define G "\033[32m"
#define Y "\033[33m"
#define E "\033[0m"

// ASLR OFF
// STACK-PROTECTOR OFF
// STACK-EXECUTION OFF

void vuln() {
    int innocent;
    char buf[BUF_SIZE];

    puts(G "enter your name :)" E);
    gets(buf);
    printf("Hello " Y "%s" E "!\n", buf);

    if (innocent == KEY) {
        if (setreuid(1005, 1005)) {
            perror("setuid");
            exit(1);
        }
        if (setregid(1005, 1005)) {
            perror("setgid");
            exit(1);
        }
        system(buf);
    }
}

int main(){
    vuln();
    return 0;
}
```

���� �Ͱ� �ٸ��� `system`�� `buf`�� �� �ִ�. ���ݱ���� Buffer Overflow�� ����� `innocent`�� `key`�� ���� ������ �Ѵ�.

---

## ���� ���

���ڿ��� NULL�� �����ϴ� �������� �ϳ��� ���ڿ��� �ν��Ѵ�. `buf`�� ���ϴ� `/bin/sh`�� �Է��ϰ� `\x00`, NULL�� �Է��� �� Dummy ���� �־� `innocent` ���� ���ϴ� ������ ������ �� �ְ� �Ѵ�.

---

## `innocent`�� `buf` ������ ũ��

![bof5_1](img/bof51.png)

`buf` ������ �˱� ���� `gets`�� `innocent`�� ������ �˱� ���� `cmp`�� breakpoint�� �Ǵ�.

![bof5_2](img/bof52.png)

`gets` �Լ����� break�� �ɾ�, rdi �������Ϳ� ����� �ּҰ��� Ȯ���Ѵ�.

![bof5_3](img/bof53.png)

```c
if(innocent == KEY){
    ...
}
```

`cmp` ��ɾ�� �̰����� ����ȴ�. `innocent`�� �ּҰ��� Ȯ���Ѵ�.

![bof5_4](img/bof5_4.png)

���� �� �� �ֵ��� `innocent`�� `buf` ������ �Ÿ��� 140�� ���� �� �� �ִ�.

---

## ���

![bof5_result](img/bof5_5.png)

���α׷����� `buf`�� `/bin/sh\x00`���� �Է½�Ű�� 8����Ʈ�� �Ҹ�Ǳ� ������ Dummy ����  `128`��ŭ �ִ´�. `innocent`�� `key` ������ �ִ´�.

`id` ��ɾ �Է��غ��� ���� ���� �� �� �ֽ��ϴ�.