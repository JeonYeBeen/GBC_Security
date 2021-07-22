# 메모리 보호 기법 정리

## ASLR

ASLR이란? Address Space Layout Randomization의 줄임말이다. 프로그램의 Heap, Stack, 공유 라이브러리 등을 가상 메모리 공간에 적용하는 위치를 매번 실행할 때마다 무작위화시켜 공격을 방해한다. ASLR은 데이터 영역의 주소만 무작위화시킨다. 

---

## RELRO

RELRO란 ? Relocation Read-Only의 줄임말로, got-overwrite과 같은 공격에 대비하여 ELF 바이너리 또는 프로세스의 데이터 섹션을 보호하는 기술이다.

즉, 메모리가 변경되는 것을 보호하는 기술이다.

### 2가지 모드
- Partial RELRO
  - GOT 상태 : Writable
  - 특징 : 함수 호출 시 해당 함수의 주소를 알아온다.
- Full RELRO
  - GOT 상태 : Read-Only
  - 특징 : ELF 실행 시 GOT에 모든 라이브러리 주소를 알아온다.

---

## Canary

Buffer와 SFP(Stack Frame Pointer) 사이에 Buffer Overflow를 탐지하기 위해 임의의 데이터를 삽입하는 기법이다. 만약 Buffer Overflow를 통해 공격을 하면 Canary가 변경되어 경고가 발생할 것이다.

---

## NX-Bit

NX-Bit란 ? Never Execute Bit의 줄임말로, 실행 방지 비트이다. 이 기술은 프로세스 명령어나 코드 또는 데이터 저장을 위한 메모리 영역을 따로 분리하는 CPU의 기술이다.

NX 특성으로 지정된 메모리 구역은 데이터 저장을 위해서만 사용되고, 프로세스 명령어가 그곳에 저장되지 않게 함으로써 실행되지 않도록 만들어 준다.

