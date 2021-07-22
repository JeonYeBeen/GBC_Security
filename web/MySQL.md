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
| = | equal |
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

## Wildcard Characters

`Wildcard Character` �� ���ڿ����� �ϳ� Ȥ�� ���� ���� ���ڸ� ��ü�� �� ����Ѵ�. `LIKE` �� �Բ� �ַ� ���ȴ�.

- % �� �� ĭ Ȥ�� ���� ���� ���ڿ��� ��Ÿ�� �� ����Ѵ�.
  - Ex> bl% �� bl, black, blue, blob.
- _ �� �� ���� ���ڸ� ��Ÿ�� �� ����Ѵ�.
  - Ex> h_t �� hot, hat, hit.

---

## LIKE Operator

`LIKE` Operator �� `WHERE` �ȿ��� Ư���� ��Ģ�� ���� ���� ã�� �� ����Ѵ�. ��Ģ�� ��Ÿ�� �� `Wildcard Characters`�� ����Ѵ�.

```mysql
SELECT columns FROM table_name
WHERE columns LIKE pattern
```

---

## IN Operator

`IN` Operator �� `WHERE` �ȿ��� ������ ���� ������ ������ ��Ÿ����. �ټ��� `OR`�� ����� �Ͱ� ����.

```mysql
SELECT column_name FROM table_name
WHERE column_name IN ( value1, value2, ...);
WHERE column_name IN ( SELECT STATEMENT );
```

---

## UINON Operator

`UNION` Operator �� �� �� �̻��� `SELECT` STATEMENTS�� result-set �� ������ �� ����Ѵ�.
- ��� `SELECT` �� �ȿ����� column�� ���� ���ƾ� �Ѵ�.
- ��� column�� ����� data type �̾�� �Ѵ�.
- ��� `SELECT` �� �ȿ� column�� ���� ������� ���ɵǾ�� �Ѵ�.

```mysql
SELECT column_names FROM table1
UNION     // default ������ �ߺ��� �� ����
UNION ALL // �ߺ��� ���� ����
SELECT column_names FROM table2;
```

---

## PRIMARY KEY(PK)

- null ���� ���� �� ����. unique�� ��. ���� �ٲ��� �ʴ� �� 
- table �ϳ��� �ϳ��� key ���� ���´�.
- key�� Ư�� �ϳ� Ȥ�� �� ���̻��� column���� ���´�.

```mysql
CREATE TABLE Persons (
  ID int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Age int,
  PRIMARY KEY(ID)
  CONSTRAINT PK_Person PRIMARY KEY (ID,LastName) // PK�� �̸��� �����ְ� key ���� �� ���� ���´�.
);

ALTER TABLE Persons  // ���̺��� ���� ��������� �� ���
ADD PRIMARY KEY (ID)   // PK �߰�
ADD CONSTRAINT PK_Person PRIMARY KEY (ID,LastName); // PK �̸��� �߰�
DROP PRIMARY KEY;    // PK drop
```

---

## FOREIGN KEY

`FOREIGN KEY`�� ���̺� ���� ������ �������� ���� �����ϱ� ���ؼ� ���ȴ�.

### Persons Table

| Person ID | Name | Age |
| :-- | :-- | :-- |
| 1 | Ola Hansen | 30 |
| 2 | Tove Svendson | 23 |
| 3 | Kari Pettersen | 20 |

### Orders Table

| Order ID | OrderNumber | PersonID |
| 1 | 77897 | 3 |
| 2 | 45618 | 3 |
| 3 | 45231 | 2 |

�� ���� ��� Order table�� PersonID column �� Persons Table�� Person ID�� ����Ű�� �ִ�. 

Persons Table���� Person ID�� PK �̰�, Orders Table���� PersonID�� FK�̴�.

Persons Table�� Parent Table�� �ǰ�, Orders Table�� Child Talbe�� �ȴ�.

```mysql
CREATE TABLE Orders (
  OrderID int NOT NULL,
  OrderNumber int NOT NULL,
  PersonID int,
  PRIMARY KEY (OrderID),
  CONSTRAINT FK_PersonOrder FOREIGN KEY (PersonID) // child table�� ���� �� ���
  REFERENCES Persons (PersonID)
);

ALTER TABLE Orders    // Table�� ��������� �� �� FK�� ������ ��
ADD CONSTRAINT FK_PersonOrder
FOREIGN KEY (PersonID) REFERENCES Persons (PersonID);

DROP FOREIGN KEY FK_PersonOrder;   // FK ������ ������ ��
```

---

