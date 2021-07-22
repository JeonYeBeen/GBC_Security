# MySQL

## SELECT Statement

`SELECT` 문은 database에서 data를 찾아올 때 사용한다.
반환된 data는 result-set 이라 불리는 result table에 저장된다.

``` mysql
SELECT column1, column2 FROM table_name ;
```

table_name에 해당하는 table에서 column 에 해당하는 data 들을 찾아온다.

```mysql
SELECT COUNT(DISTINCT Country) FROM Customers;
```

Country column 중에 중복되는 값은 제외하고 개수를 알려준다.

---

## WHERE Clause(조항)

WHERE Clause 는 특정 조건을 충족하는 값들을 추출한다.

```mysql
SELECT * FROM Customers
WHERE Country = 'MEXICO';
```

위 명령어는 Customes table 에서 Country 라는 column이 'MEXICO' 라는 값을 반환한다.

> SQL 에서는 text 변수만 ''나 ""로 감싼다. 숫자는 그렇지 않다.

| Operator | 설명 |
| :--: | :--: |
| = | equal |
| <> | Not equal |
| BETWEEN | 지정 값 사이 |
| example | `BETWEEN 50 AND 60` |
| LIKE | 지정 패턴 |
| example | `LIKE 's%'` |
| IN | 가능한 값의 집합 |
| example | `IN ('Paris', 'LONDON')` |

---

## AND, OR and NOT Operators

`AND`는 조건이 모두 참일 경우만 실행한다.

`OR` 는 조건 중 하나만 참이어도 실행한다.

`NOT` 은 조건이 거짓일 때 실행한다.

```mysql
SELECT column1, column2, ...
FROM table_name
WHERE condition1 AND(OR) condition2 ; 
WHERE NOT condition;
```

---

## ORDER BY Keyword

`ORDER BY` 는 result-set을 오름차순이나 내림차순으로 정렬할 때 사용한다. 기본값은 오름차순이다.

```mysql
SELECT column1, column2 FROM table_name
ORDER BY column2 ASC, column1 DESC;
```

위와 같이 열을 지정해서 정렬을 원하는데로 할 수 있다.

---

## INSERT INTO statement

`INSERT INTO` statement 는 table에 새로 기록을 할 때 사용한다.

1. 값을 넣은 열을 정해준다.

```mysql
INSERT INTO table_name (column1, column2, column3, ... )
VALUES (value1, value2, value3, ... );
```

2. 열을 정해주지 않고 모든 열에 순서대로 값을 넣는다.

```mysql
INSERT INTO table_name
VALUES (value1, value2, value3, ... );
```

---

## NULL Values

`NULL Value` 는 빈 값이다. table에 아무런 값을 저장하지 않았을 때 이 값이 저장된다.

```mysql
SELECT column_names
FROM table_name
WHERE column_name IS NULL / IS NOT NULL;
```

---

## UPDATE Statement

`UPDATE` statement 는 table에 작성된 기록(records)을 수정할 때 사용한다.

```mysql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition
```

> `WHERE condition`이 없으면 모든 값들이 수정된다.

---

## DELETE Statement

`DELETE` Statement는 해당 테이블에 존재하는 기록을 삭제할 때 사용한다.

```mysql
DELETE FROM table_name WHERE condition;
```
 
> `WHERE condition`이 없으면 모든 record 들이 삭제된다. 

---

## LIMIT Clause

`LIMIT` Clause는 반환되는 record 의 수를 정한다.

```mysql
SELECT column_name(s)
FROM table_name
WHERE condition
LIMIT number
```

---

## MIN() and MAX() Function

`MIN(), MAX()` 는 각각 지정한 column 에서 최소와 최대를 반환한다.

```mysql
SELECT MIN/MAX(column_name)
FROM table_name
WHERE condition;
```

---

## Wildcard Characters

`Wildcard Character` 는 문자열에서 하나 혹은 여러 개의 문자를 대체할 때 사용한다. `LIKE` 와 함께 주로 사용된다.

- % 는 빈 칸 혹은 여러 개의 문자열을 나타낼 때 사용한다.
  - Ex> bl% 는 bl, black, blue, blob.
- _ 는 한 개의 문자를 나타낼 때 사용한다.
  - Ex> h_t 는 hot, hat, hit.

---

## LIKE Operator

`LIKE` Operator 는 `WHERE` 안에서 특정한 규칙을 가진 열을 찾을 때 사용한다. 규칙을 나타낼 때 `Wildcard Characters`를 사용한다.

```mysql
SELECT columns FROM table_name
WHERE columns LIKE pattern
```

---

## IN Operator

`IN` Operator 는 `WHERE` 안에서 가능한 여러 변수의 집합을 나타낸다. 다수의 `OR`를 축약한 것과 같다.

```mysql
SELECT column_name FROM table_name
WHERE column_name IN ( value1, value2, ...);
WHERE column_name IN ( SELECT STATEMENT );
```

---

## UINON Operator

`UNION` Operator 는 두 개 이상의 `SELECT` STATEMENTS의 result-set 을 결합할 때 사용한다.
- 모든 `SELECT` 문 안에서는 column의 수가 같아야 한다.
- 모든 column은 비슷한 data type 이어야 한다.
- 모든 `SELECT` 문 안에 column은 같은 순서대로 정령되어야 한다.

```mysql
SELECT column_names FROM table1
UNION     // default 값으로 중복된 값 제외
UNION ALL // 중복된 값도 포함
SELECT column_names FROM table2;
```

---

## PRIMARY KEY(PK)

- null 값을 가질 수 없다. unique한 것. 자주 바뀌지 않는 것 
- table 하나당 하나의 key 값을 갖는다.
- key는 특정 하나 혹은 두 개이상의 column으로 갖는다.

```mysql
CREATE TABLE Persons (
  ID int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Age int,
  PRIMARY KEY(ID)
  CONSTRAINT PK_Person PRIMARY KEY (ID,LastName) // PK의 이름을 정해주고 key 값을 두 개를 갖는다.
);

ALTER TABLE Persons  // 테이블이 먼저 만들어졌을 때 사용
ADD PRIMARY KEY (ID)   // PK 추가
ADD CONSTRAINT PK_Person PRIMARY KEY (ID,LastName); // PK 이름과 추가
DROP PRIMARY KEY;    // PK drop
```

---

## FOREIGN KEY

`FOREIGN KEY`는 테이블 간의 연결이 끊어지는 것을 방지하기 위해서 사용된다.

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

위 같은 경우 Order table의 PersonID column 이 Persons Table의 Person ID를 가리키고 있다. 

Persons Table에서 Person ID는 PK 이고, Orders Table에서 PersonID는 FK이다.

Persons Table은 Parent Table이 되고, Orders Table은 Child Talbe이 된다.

```mysql
CREATE TABLE Orders (
  OrderID int NOT NULL,
  OrderNumber int NOT NULL,
  PersonID int,
  PRIMARY KEY (OrderID),
  CONSTRAINT FK_PersonOrder FOREIGN KEY (PersonID) // child table을 만들 때 사용
  REFERENCES Persons (PersonID)
);

ALTER TABLE Orders    // Table이 만들어지고 난 뒤 FK를 지정할 때
ADD CONSTRAINT FK_PersonOrder
FOREIGN KEY (PersonID) REFERENCES Persons (PersonID);

DROP FOREIGN KEY FK_PersonOrder;   // FK 지정을 삭제할 때
```

---

