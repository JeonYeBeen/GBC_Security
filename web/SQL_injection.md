# SQL_Injection

악의적인 sql문을 실행되게 함으로 데이터베이스를 비정상적으로 동작하게 하는 code injection 공격 방법

## 성공적인 sql injection

민감한 데이터를 읽을 수 있고 데이터의 수정 가능

## 방법

id : admin
password : password' OR 1=1 --

or

id : admin' OR 1=1 --

## 보안 방법

1. prepared statement 사용
2. 특별한 의미를 갖는 문자들을 인코딩 시켜서 sql 삽입을 방어한다.
3. passwd 를 md5 방식(비대칭 키)으로 암호화시켜 저장한다.