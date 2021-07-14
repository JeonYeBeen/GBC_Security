# bof4

## bof4.c

``` c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#define BUF_SIZE 128
#define KEY 0x12345678
#define R "\033[31m"
#define E "\033[0m"

// ASLR OFF
// STACK-PROTECTOR OFF
// STACK-EXECUTION OFF

void vuln(char * arg) {
    int innocent;
    char buf[BUF_SIZE];

    strcpy(buf, arg);
    printf("Hello %s!\n", buf);

    if (innocent == KEY) {
        if (setreuid(1004, 1004)) {
            perror("setuid");
            exit(1);
        }
        if (setregid(1004, 1004)) {
            perror("setgid");
            exit(1);
        }
        system("/bin/sh");
    }
}

int main(int argc, char *argv[]){
    if (argc < 2) {
        fputs(R "error :( this program needs some arguments\n" E, stderr);
        return 1;
    }
    vuln(argv[1]);
    return 0;
}
```

�� �ڵ�� strcpy �� ������̴�. main �Լ����� �Է¹��� ���ڸ� ���̿� ������� �޴´�.

## ���� ���

���� ����� [bof3](bof3.md)�� ���� Buffer Overflow ����� ����� ���̴�. `buf`�� �ִ��� ���� �Է� ���� `innocent` ���� ���ϴ� ������ ������ �� �ְ� �Ѵ�. 

---

## `innocent`�� `buf` ������ ũ��

![bof4 rdi](img/bof4%20rdi.png)

`strcpy` �Լ��� breakpoint �� �ɾ� ���� ��Ų ��, rdi �������Ϳ� ����� �ּҰ��� Ȯ���Ѵ�.

![bof4 ino](img/bof4%20ino.png)

``` c
if (innocent == KEY) {
    ...
}
```
�̰����� �̵��Ͽ� `innocent`�� �ּҰ��� Ȯ���Ѵ�.

![bof4 sub](img/bof4%20sub.png)

���� �� �� �ֵ��� `innocent`�� `buf` ������ ũ��� 140 �� ���� �� �� �ִ�. 
---
## ���

![bof4 result](img/bof4%20result.png)

`bof4` �� ������ �� `main` �Լ��� ���ڸ� ��������� �ϱ� ������ ������ ���� ����� �ۼ��Ѵ�. `a`�� 140�� �Է��ؼ� `buf`�� �� ä�� �� `innocent`�� `KEY(0x12345678)`�� ���� ���� �Է��Ѵ�. �Է� ����� `little endian`�̹Ƿ� �� ����Ʈ�� �������� �Է��Ѵ�.

��ɾ �Է��ϴ� shell �� ��Ÿ���� id�� �Է����� �� bof5�� id�� ���� ���� Ȯ���� �� �ִ�.