# SQL Injection (HIGH)

## ����

![sql_high_1](../img/sql_high_1.png)

�� ������ ������� �Է��� ID�� ���� ��°��� �ٸ���.

ID�� �Է��� �� Query ���� �ۼ��ϸ� �� �� ����.

<Br>

## ���

![sql_high_2](../img/sql_high_2.png)

ID�� `OR 1=1 #`�� �־�� ���� �״�� �ԷµǴ� ���� �� �� �ִ�.

`WHERE user_id = '$id';` �� ���� ���ִ� ���� ������ �� �ִ�.

<br>

![sql_high_3](../img/sql_high_3.png)

ID�� `'OR 1=1 #`�� �־�� ������ ��� ��µ� ���� Ȯ���� �� �ִ�.