# ENV�� �̿��� Ret-Sled 

ȯ�溯���� �̿��� Ret-Sled ���ݿ� ���� �˾ƺ��ô�.

> ���� : https://d4m0n.tistory.com/86

---

## Ret-Sled ��?

Return Address�� `ret`(�������) ��ɾ��� �ּҸ� ����� `ret` ����� ���������� ȣ���Ͽ� �� ������ �����ϴ� ���� ���Ѵ�.

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

Ret-Sled�� �̷� ���� �̿��Ͽ� ret ��ɾ �������� �����Ѵ�.

---

## Ret-Sled ����

Return Address ���� �ּҿ� Shell Code�� �ּҸ� �Է½�Ű��,

Return Address�� ret ��ɾ��� �ּ�(ret�� �����Ŵ)�� �Է½�Ű��,

ret ����� �� �� �� �����ϸ鼭 pop rip�� ���� rip���� shell code�� �ּҰ� ���� �ǰ�,

jmp rip�� ���� shell code�� �ּҷ� jump �ϰ� �Ǹ鼭 shell code�� ����� �����ϰ� �ȴ�.
