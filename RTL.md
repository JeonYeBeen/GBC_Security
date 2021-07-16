# RTL

## note

system 주소를 찾고 나가서(p system)
memcmp((void*)addr, "/bin/sh\x00",8) -> system 함수 안에 /bin/sh 주소를 찾는다
return address 에 system 함수 주소값을 넣는다. argument 로 /bin/sh을 넣어야 하기 때문.

그러면 system이 실행된다.

system도 함수여서 return 값이 존재 하는데 system 호출하면 return 값, 인자값이 생성되는데 인자값에다가 /bin/sh 주소를 넣는다.

인자값은 return address 전에 넣는다.

함수 호출하기전에 인자를 push 한다.

/sfp/ret(system 주소)/쓰레기(그 전 rip 주소가 있는데 실행 안되게 만듬)/"/bin/sh"주소

## note2

쉘 코드를 실행할 수 없을 때 nx-bit를 우회하는 기법

int add듬=system 주소;

while(memcmp((void*)addr, "/bin/sh\x00",8)){
    addr++;
}
printf(");

0x7ffff7a52390