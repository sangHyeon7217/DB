# 📘 SQL 요약 정리 (출퇴근 짬공부용)

## 📌 기본 조회
```sql
SELECT * FROM student; 
-- student 테이블의 모든 컬럼을 조회합니다.

SELECT DISTINCT num FROM student;
-- student 테이블의 num 컬럼에서 중복 없이 고유한 값만 조회합니다.

SELECT COUNT(DISTINCT num) FROM student;
-- 중복을 제거한 num 값의 개수를 셉니다.
```

## 📌 WHERE 조건절
```sql
SELECT * FROM student WHERE num = 1;
-- num이 1인 행만 조회합니다.

SELECT * FROM student WHERE num BETWEEN 1 AND 3;
-- num이 1 이상 3 이하인 모든 행을 조회합니다.

SELECT * FROM student WHERE city LIKE '%s';
-- city 값이 s로 끝나는 행을 조회합니다.

SELECT * FROM customer WHERE score IN (90, 100);
-- score 값이 90 또는 100인 행만 조회합니다.

SELECT * FROM customer WHERE city = 'a' OR id = 'b';
-- city가 'a'이거나 id가 'b'인 행을 조회합니다.

SELECT * FROM customer WHERE city = 'a' AND id = 'b';
-- city가 'a'이고 id가 'b'인 행을 조회합니다.

SELECT * FROM customer WHERE nation = 'korea' AND (city = 'b' OR city = 'c');
-- nation이 'korea'이고 city가 'b' 또는 'c'인 행을 조회합니다.

SELECT * FROM customer WHERE NOT country = 'j' AND NOT country = 's';
-- country가 'j'도 아니고 's'도 아닌 행을 조회합니다.
```

## 📌 정렬 ORDER BY
```sql
SELECT * FROM customer WHERE name = 'kim' ORDER BY country;
-- name이 'kim'인 행을 country 기준으로 오름차순 정렬합니다.

SELECT * FROM customer ORDER BY country, name;
-- country 기준으로 정렬 후, 동일한 country 내에서는 name으로 정렬합니다.

SELECT * FROM customer ORDER BY country ASC, name DESC;
-- country는 오름차순, name은 내림차순으로 정렬합니다.
```

## 📌 INSERT
```sql
INSERT INTO student(id, name, score) VALUES ('a', 'nick', 30);
-- 컬럼명을 명시하고 해당 순서대로 값을 삽입합니다.

INSERT INTO student VALUES ('a', 'nick', 30);
-- 모든 컬럼에 순서대로 값을 삽입합니다.
```

## 📌 NULL 조건
```sql
SELECT * FROM student WHERE address IS NOT NULL;
-- address 컬럼에 값이 있는 행만 조회합니다.

SELECT * FROM student WHERE address IS NULL;
-- address 컬럼이 NULL인 행만 조회합니다.
```

## 📌 UPDATE & DELETE
```sql
UPDATE student SET id = 'b', name = 'jack', score = 100 WHERE id = 'a';
-- id가 'a'인 행의 값을 수정합니다. WHERE 절이 없으면 전체가 변경됩니다.

UPDATE customer SET name = 'peter' WHERE country = 'mexico';
-- country가 'mexico'인 고객의 name을 모두 'peter'로 변경합니다.

DELETE FROM customer WHERE name = 'jack';
-- name이 'jack'인 고객 정보를 삭제합니다.

DELETE FROM customer;
-- customer 테이블의 모든 데이터를 삭제합니다.
```

## 📌 ROWNUM
```sql
SELECT * FROM customers WHERE ROWNUM <= 3;
-- 상위 3개의 행만 출력합니다.

SELECT * FROM customers WHERE country = 'germany' AND ROWNUM <= 3;
-- country가 'germany'인 행 중 상위 3개만 출력합니다.
```

## 📌 집계 함수
```sql
SELECT MIN(num) AS smallNum FROM student;
-- num 컬럼의 최소값을 smallNum이라는 이름으로 조회합니다.

SELECT MAX(num) AS bigNum FROM student;
-- num 컬럼의 최대값을 조회합니다.

SELECT COUNT(id) FROM student;
-- id 컬럼에 값이 있는 행의 개수를 조회합니다.

SELECT AVG(score) FROM student;
-- score의 평균값을 계산합니다.

SELECT SUM(score) FROM student;
-- score의 총합을 계산합니다.
```

## 📌 LIKE & 와일드카드
```sql
SELECT * FROM student WHERE name LIKE 'a%';
-- 이름이 a로 시작하는 행을 조회합니다.

SELECT * FROM student WHERE name LIKE '%a';
-- 이름이 a로 끝나는 행을 조회합니다.

SELECT * FROM student WHERE name LIKE '%a%';
-- 이름에 a가 포함된 행을 조회합니다.

SELECT * FROM student WHERE name LIKE '_k';
-- 두 번째 문자가 k인 2글자 이름을 조회합니다.

SELECT * FROM student WHERE name LIKE 'A__';
-- 이름이 A로 시작하고 총 3글자인 행을 조회합니다.

SELECT * FROM student WHERE name LIKE 'a%o';
-- 이름이 a로 시작하고 o로 끝나는 행을 조회합니다.

SELECT * FROM student WHERE name LIKE '_ack';
-- 첫 글자는 아무거나, 뒤에 ack가 오는 이름을 조회합니다.

SELECT * FROM customer WHERE city LIKE '[bsp]%';
-- city가 b, s, p 중 하나로 시작하는 행을 조회합니다.

SELECT * FROM customer WHERE city LIKE '[a-c]%';
-- city가 a~c 사이 문자로 시작하는 행을 조회합니다.

SELECT * FROM customer WHERE city LIKE '[!bsp]%';
-- city가 b, s, p가 아닌 문자로 시작하는 행을 조회합니다.
```

## 📌 IN & BETWEEN & AS
```sql
SELECT * FROM customer WHERE score IN (80, 90);
-- score가 80 또는 90인 행을 조회합니다.

SELECT * FROM student WHERE score BETWEEN 70 AND 100;
-- score가 70 이상 100 이하인 행을 조회합니다.

SELECT name AS n, id AS [student id] FROM student;
-- name과 id 컬럼에 별칭을 지정하여 조회합니다.
```

## 📌 JOIN
```sql
-- INNER JOIN
SELECT c.score, k.num FROM testc c, testk k WHERE c.pw = k.pw;
-- 두 테이블의 pw가 같은 행을 조인해서 score와 num을 조회합니다.

-- OUTER JOIN (Oracle 방식)
SELECT c.num, k.id FROM testk c, member k WHERE c.id = k.id(+);
-- testk 기준으로 조인하며 member에 없는 값도 출력합니다 (NULL로 표시).
```

## 📌 GROUP BY & HAVING
```sql
SELECT COUNT(customerId), country FROM customer GROUP BY country ORDER BY COUNT(customerId) DESC;
-- 국가별 고객 수를 세고, 많은 순서대로 정렬합니다.

SELECT COUNT(customerId), country FROM customer GROUP BY country HAVING COUNT(customerId) > 5;
-- 국가별 고객 수가 5명 이상인 행만 필터링합니다.
```

## 📌 ANY / ALL
```sql
SELECT name FROM product WHERE productId = ANY(SELECT productId FROM orderDetail WHERE quantity = 10);
-- 수량이 10인 제품들 중 하나라도 일치하면 해당 이름 조회

SELECT name FROM product WHERE productId = ALL(SELECT productId FROM orderDetail WHERE quantity = 10);
-- 모든 수량이 10인 경우에만 해당 제품 이름을 조회
```

## 📌 테이블 구조 변경 (ALTER)
```sql
ALTER TABLE student ADD email VARCHAR2(50);
-- email 컬럼을 새로 추가합니다.

ALTER TABLE student DROP COLUMN email;
-- email 컬럼을 삭제합니다.

ALTER TABLE person MODIFY num VARCHAR2(3);
-- num 컬럼의 데이터 타입을 VARCHAR2(3)으로 변경합니다.

ALTER TABLE t1 RENAME COLUMN num TO pw;
-- num 컬럼의 이름을 pw로 변경합니다.
```

## 📌 제약조건 (Constraints)
```sql
-- NOT NULL
ALTER TABLE person MODIFY age NUMBER(2) NOT NULL;
-- age 컬럼에 NULL이 들어가지 않도록 설정합니다.

-- UNIQUE
ALTER TABLE t1 ADD UNIQUE(pw);
-- pw 컬럼에 중복을 허용하지 않도록 합니다.

ALTER TABLE person ADD CONSTRAINT unique_c UNIQUE(id, pw);
-- id, pw 복합 유니크 제약을 추가하고 이름을 unique_c로 설정합니다.

ALTER TABLE person DROP CONSTRAINT unique_c;
-- 제약조건 unique_c를 제거합니다.

-- PRIMARY KEY
ALTER TABLE Persons DROP CONSTRAINT PK_Person;
-- PK_Person이라는 기본키 제약을 제거합니다.

-- FOREIGN KEY
ALTER TABLE Orders ADD FOREIGN KEY (PersonID) REFERENCES Persons(PersonID);
-- Orders 테이블의 PersonID가 Persons 테이블의 PersonID를 참조하도록 합니다.

ALTER TABLE Orders DROP CONSTRAINT fk_orderskey;
-- 참조 제약 조건을 삭제합니다.

-- CHECK
CREATE TABLE person (
  id INT NOT NULL,
  pw NUMBER(10),
  age INT CHECK (age >= 18)
);
-- age가 18 이상만 입력 가능하도록 제한합니다.

ALTER TABLE Persons ADD CONSTRAINT CHK_PersonAge CHECK (Age >= 18 AND City = 'Sandnes');
-- age가 18 이상이고 city가 'Sandnes'일 경우만 허용하는 제약조건을 추가합니다.

ALTER TABLE Persons DROP CONSTRAINT CHK_PersonAge;
-- 해당 체크 제약조건을 삭제합니다.

-- DEFAULT
CREATE TABLE Ord (
  ID VARCHAR2(3) NOT NULL,
  OrderNumber INT NOT NULL,
  OrderDate DATE DEFAULT SYSDATE
);
-- OrderDate는 기본값으로 현재 날짜가 들어가게 설정합니다.

INSERT INTO Ord(id, OrderNumber) VALUES ('a', 3);
-- 날짜 생략 시 자동으로 SYSDATE가 들어갑니다.

ALTER TABLE Ord MODIFY ID DEFAULT 'EX';
-- ID의 기본값을 'EX'로 지정합니다.
```

## 📌 SEQUENCE 사용법
```sql
CREATE SEQUENCE seq_person
  MINVALUE 1
  START WITH 1
  INCREMENT BY 1
  CACHE 10;
-- 1부터 시작해서 1씩 증가하고, 10개 값을 캐시하는 시퀀스를 생성합니다.

INSERT INTO Persons (PersonID, FirstName, LastName)
VALUES (seq_person.NEXTVAL, 'Lars', 'Monsen');
-- 시퀀스를 이용하여 자동으로 증가하는 ID 값을 넣습니다.
```

## 📌 VIEW 생성
```sql
CREATE OR REPLACE VIEW result_inner_join AS
SELECT a.empno, a.ename, a.job, a.hiredate, a.deptno, b.dname, b.loc
FROM emp a, dept b
WHERE a.deptno = b.deptno;
-- emp와 dept 테이블을 조인하여 보기 좋게 만든 뷰입니다.
```

> 💡 **뷰(View)**는 물리적으로 데이터를 저장하지 않고 쿼리로 정의된 가상 테이블입니다. 읽기 전용처럼 사용할 수 있으며 보안 및 편리성을 위해 사용됩니다.

---



## ✅ 마무리
- 제약조건과 조인, 그룹바이 등은 자주 실습하면서 반복 숙달이 중요합니다.


