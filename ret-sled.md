# ENV를 이용한 Ret-Sled 

환경변수를 이요한 Ret-Sled 공격에 대해 알아봅시다.

> 참고 : https://d4m0n.tistory.com/86

---

## Ret-Sled 란?

Return Address에 `ret`(어셈블리어) 명령어의 주소를 덮어씌어 `ret` 명령을 연속적으로 호출하여 콜 스택을 조작하는 것을 말한다.

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

Ret-Sled는 이런 점을 이용하여 ret 명령어를 연속으로 실행한다.

---

## Ret-Sled 원리

Return Address 이전 주소에 Shell Code의 주소를 입력시키고,

Return Address에 ret 명령어의 주소(ret을 실행시킴)를 입력시키면,

ret 명령이 한 번 더 실행하면서 pop rip로 인해 rip에는 shell code의 주소가 들어가게 되고,

jmp rip로 인해 shell code의 주소로 jump 하게 되면서 shell code의 명령을 실행하게 된다.
