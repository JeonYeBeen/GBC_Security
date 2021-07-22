# Resister 요약

EAX ? 피연산자와 연산 결과의 저장소 

EBX ? DS segment안의 데이터를 가리키는 포인터 

ECX ? 문자열 처리나 루프를 위한 카운터 

EDX ? I/O 포인터 

ESI ? DS 레지스터가 가리키는 data segment 내의 어느 데이터를 가리키고 있는 포인터. 문자열 처리에서 source를 가리킴.

EDI ? ES 레지스터가 가리키고 있는 data segment 내의 어느 데이터를 가리키고 있는 포 인터. 문자열 처리에서 destination을 가리킴.

ESP ? SS 레지스터가 가리키는 stack segment의 맨 꼭대기를 가리키는 포인터 

EBP ? SS 레지스터가 가리키는 스텍상의 한 데이터를 가리키는 포인터