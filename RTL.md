# RTL

## note

system �ּҸ� ã�� ������(p system)
memcmp((void*)addr, "/bin/sh\x00",8) -> system �Լ� �ȿ� /bin/sh �ּҸ� ã�´�
return address �� system �Լ� �ּҰ��� �ִ´�. argument �� /bin/sh�� �־�� �ϱ� ����.

�׷��� system�� ����ȴ�.

system�� �Լ����� return ���� ���� �ϴµ� system ȣ���ϸ� return ��, ���ڰ��� �����Ǵµ� ���ڰ����ٰ� /bin/sh �ּҸ� �ִ´�.

���ڰ��� return address ���� �ִ´�.

�Լ� ȣ���ϱ����� ���ڸ� push �Ѵ�.

/sfp/ret(system �ּ�)/������(�� �� rip �ּҰ� �ִµ� ���� �ȵǰ� ����)/"/bin/sh"�ּ�

## note2

�� �ڵ带 ������ �� ���� �� nx-bit�� ��ȸ�ϴ� ���

int add��=system �ּ�;

while(memcmp((void*)addr, "/bin/sh\x00",8)){
    addr++;
}
printf(");

0x7ffff7a52390