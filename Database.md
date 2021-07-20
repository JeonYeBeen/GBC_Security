# MySQL

## SELECT Statement

`SELECT` ���� database���� data�� ã�ƿ� �� ����Ѵ�.
��ȯ�� data�� result-set �̶� �Ҹ��� result table�� ����ȴ�.

``` mysql
SELECT column1, column2 FROM table_name ;
```

table_name�� �ش��ϴ� table���� column �� �ش��ϴ� data ���� ã�ƿ´�.

```mysql
SELECT COUNT(DISTINCT Country) FROM Customers;
```

Country column �߿� �ߺ��Ǵ� ���� �����ϰ� ������ �˷��ش�.

---

## WHERE Clause(����)

WHERE Clause �� Ư�� ������ �����ϴ� ������ �����Ѵ�.

```mysql
SELECT * FROM Customers
WHERE Country = 'MEXICO';
```

�� ��ɾ�� Customes table ���� Country ��� column�� 'MEXICO' ��� ���� ��ȯ�Ѵ�.

> SQL ������ text ������ ''�� ""�� ���Ѵ�. ���ڴ� �׷��� �ʴ�.

| Operator | ���� |
| :--: | :--: |
| <> | Not equal |
| BETWEEN | ���� �� ���� |
| example | `BETWEEN 50 AND 60` |
| LIKE | ���� ���� |
| example | `LIKE 's%'` |
| IN | ������ ���� ���� |
| example | `IN ('Paris', 'LONDON')` |

---

## AND, OR and NOT Operators

`AND`�� ������ ��� ���� ��츸 �����Ѵ�.

`OR` �� ���� �� �ϳ��� ���̾ �����Ѵ�.

`NOT` �� ������ ������ �� �����Ѵ�.

```mysql
SELECT column1, column2, ...
FROM table_name
WHERE condition1 AND(OR) condition2 ; 
WHERE NOT condition;
```

---

## ORDER BY Keyword

`ORDER BY` �� result-set�� ���������̳� ������������ ������ �� ����Ѵ�. �⺻���� ���������̴�.

```mysql
SELECT column1, column2 FROM table_name
ORDER BY column2 ASC, column1 DESC;
```

���� ���� ���� �����ؼ� ������ ���ϴµ��� �� �� �ִ�.

---

## INSERT INTO statement

`INSERT INTO` statement �� table�� ���� ����� �� �� ����Ѵ�.

1. ���� ���� ���� �����ش�.

```mysql
INSERT INTO table_name (column1, column2, column3, ... )
VALUES (value1, value2, value3, ... );
```

2. ���� �������� �ʰ� ��� ���� ������� ���� �ִ´�.

```mysql
INSERT INTO table_name
VALUES (value1, value2, value3, ... );
```

---

## NULL Values

`NULL Value` �� �� ���̴�. table�� �ƹ��� ���� �������� �ʾ��� �� �� ���� ����ȴ�.

```mysql
SELECT column_names
FROM table_name
WHERE column_name IS NULL / IS NOT NULL;
```

---

## UPDATE Statement

`UPDATE` statement �� table�� �ۼ��� ���(records)�� ������ �� ����Ѵ�.

```mysql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition
```

> `WHERE condition`�� ������ ��� ������ �����ȴ�.

---

## DELETE Statement

`DELETE` Statement�� �ش� ���̺� �����ϴ� ����� ������ �� ����Ѵ�.

```mysql
DELETE FROM table_name WHERE condition;
```
 
> `WHERE condition`�� ������ ��� record ���� �����ȴ�. 

---

## LIMIT Clause

`LIMIT` Clause�� ��ȯ�Ǵ� record �� ���� ���Ѵ�.

```mysql
SELECT column_name(s)
FROM table_name
WHERE condition
LIMIT number
```

---

## MIN() and MAX() Function

`MIN(), MAX()` �� ���� ������ column ���� �ּҿ� �ִ븦 ��ȯ�Ѵ�.

```mysql
SELECT MIN/MAX(column_name)
FROM table_name
WHERE condition;
```

---

primary key �� null ���� ���� �� ����

```
CREATE TABLE info(
userid VARCHAR(20) PRIMARY KEY,
userpw VARCHAR(20)
);

```

select * from info;
select userid from info;
select * from info where(����) userid="USER1";
select * from info where userid="admin" and userpw="admin"

insert into info values("USER1","PASSWD");
