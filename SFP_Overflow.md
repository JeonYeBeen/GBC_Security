# SFP Overflow

*** 한 바이트 더 받는 것이 포인트

마지막 한 바이트를 buf 시작 전전인 부분으로 수정해서 buf에 shell code를 집어 넣는다.

## UAF(User After Free)

heap overflow라 일어나면 free 를 해도 그 전에 쓰였던 데이터들이 남아 있다.

만약 같은 크기만큼 할당을 하면 그 전 데이터에 접근이 가능하다.


## Fake EBP

## ROP