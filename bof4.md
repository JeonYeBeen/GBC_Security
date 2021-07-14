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

이 코드는 strcpy 가 취약점이다. main 함수에서 입력받은 인자를 길이에 상관없이 받는다.

## 공격 방법

공격 방법은 [bof3](bof3.md)와 같은 Buffer Overflow 방법을 사용할 것이다. `buf`에 최대한 값을 입력 시켜 `innocent` 값을 원하는 값으로 저장할 수 있게 한다. 

---

## `innocent`와 `buf` 사이의 크기

![bof4 rdi](img/bof4%20rdi.png)

`strcpy` 함수에 breakpoint 를 걸어 실행 시킨 후, rdi 레지스터에 저장된 주소값을 확인한다.

![bof4 ino](img/bof4%20ino.png)

``` c
if (innocent == KEY) {
    ...
}
```
이곳으로 이동하여 `innocent`의 주소값을 확인한다.

![bof4 sub](img/bof4%20sub.png)

위에 볼 수 있듯이 `innocent`와 `buf` 사이의 크기는 140 인 것을 알 수 있다. 
---
## 결과

![bof4 result](img/bof4%20result.png)

`bof4` 는 실행할 때 `main` 함수로 인자를 전달해줘야 하기 때문에 다음과 같이 명령을 작성한다. `a`를 140개 입력해서 `buf`를 다 채운 후 `innocent`에 `KEY(0x12345678)`과 같은 수를 입력한다. 입력 방식은 `little endian`이므로 한 바이트씩 역순으로 입력한다.

명령어를 입력하니 shell 이 나타나며 id를 입력했을 때 bof5의 id로 변한 것을 확인할 수 있다.