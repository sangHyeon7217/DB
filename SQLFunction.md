## 함수(Function) 정리

### 묵시적(형변환) 함수 / 명시적 함수
- **묵시적 형변환 함수**: SQL이 자동으로 데이터 타입을 변환하는 경우
- **명시적 형변환 함수**: 개발자가 직접 변환을 지정하는 경우

```sql
SELECT *
FROM EMPLOYEES2 
WHERE employee_id = '100'; -- 묵시적 형변환 함수 예제
```

---

### 내장 함수 / 사용자 정의 함수
- **내장 함수**: SQL에서 기본적으로 제공하는 함수
- **사용자 정의 함수**: 사용자가 직접 정의하여 사용하는 함수

---

### 다중행 함수 / 단일행 함수
- **다중행 함수**: 여러 행을 입력받아 하나의 결과를 반환 (예: SUM, AVG)
- **단일행 함수**: 한 행에 대해 하나의 값을 반환 (예: LENGTH, UPPER)

---

### 문자 함수 / 숫자 함수 / 날짜 함수 / 변환 함수
- **[숫자형 함수 (Numeric Functions)](http://www.gurubee.net/lecture/1024)**
- **[문자형 함수 (Character Functions)](http://www.gurubee.net/lecture/1025)**
- **[날짜 함수 (Datetime Functions)](http://www.gurubee.net/lecture/1026)**

---

### 날짜 함수 (Datetime Functions)
- 날짜 함수는 `NUMBER`형 값 또는 `DATE`형 값을 반환함
- `SYSDATE`: 현재 일자와 시간 (시스템 기준, 최소 단위 = 1초)

```sql
SELECT SYSDATE-1, SYSDATE, SYSDATE+1 FROM DUAL;
SELECT SYSDATE-(24/24), SYSDATE, SYSDATE+(24/24) FROM DUAL;
SELECT SYSDATE-(10/24), SYSDATE, SYSDATE+(10/24) FROM DUAL; -- 10시간 전/후
SELECT SYSDATE-(10/(24*60)), SYSDATE, SYSDATE+(10/(24*60)) FROM DUAL; -- 10분 전/후
```

#### 날짜 포맷 변환
```sql
SELECT TO_CHAR(SYSDATE, 'RRRR-MM-DD HH24:MI:SS') "지금시간" FROM DUAL;
SELECT TO_CHAR(SYSDATE-1, 'RRRR-MM-DD HH24:MI:SS') "하루전지금시간" FROM DUAL;
SELECT TO_CHAR(SYSDATE-1/24, 'RRRR-MM-DD HH24:MI:SS') "1시간전시간" FROM DUAL;
SELECT TO_CHAR(SYSDATE-1/24/60, 'RRRR-MM-DD HH24:MI:SS') "1분전시간" FROM DUAL;
SELECT TO_CHAR(SYSDATE-1/24/60/10, 'RRRR-MM-DD HH24:MI:SS') "6초전시간" FROM DUAL;
SELECT TO_CHAR(SYSDATE-(5/24 + 30/24/60 + 10/24/60/60), 'RRRR-MM-DD HH24:MI:SS') "5시간 30분 10초전" FROM DUAL;
```

#### 특정 날짜 연산
```sql
SELECT TO_CHAR(ADD_MONTHS(SYSDATE,3),'RRRR-MM-DD'), ADD_MONTHS(SYSDATE,-3) FROM DUAL; -- 3개월 후, 3개월 전
```

#### 두 날짜 간의 개월 수 계산
```sql
SELECT MONTHS_BETWEEN(TO_DATE('2010-06-05','RRRR-MM-DD'), TO_DATE('2010-05-01','RRRR-MM-DD')) "month" FROM DUAL;
```

#### 사원의 근무 개월 수 조회
```sql
SELECT employee_id, FIRST_name, HIRE_DATE, SYSDATE, MONTHS_BETWEEN(SYSDATE, EMPLOYEES.HIRE_DATE) FROM employees ORDER BY hire_date DESC;
```

#### 두 날짜 간의 일수 계산
```sql
SELECT TO_DATE('2010-06-05','RRRR-MM-DD') - TO_DATE('2010-05-01','RRRR-MM-DD') "Day" FROM DUAL;
```

#### 달의 마지막 날 조회
```sql
SELECT SYSDATE, LAST_DAY(SYSDATE) FROM DUAL;
SELECT employee_id, first_name, hire_date, LAST_DAY(hire_date), LAST_DAY(hire_date) - hire_date FROM EMPLOYEES2 ORDER BY hire_date DESC;
```

---

### 문자열 검색 함수
#### INSTR 함수
- `INSTR(char1, str1, [m], [n])`
- 문자열이 포함되어 있는지를 조사하여 해당 위치를 반환
- 발견되지 않으면 `0` 반환 (대소문자 구분)
- 문자열의 첫 번째 위치는 `1`부터 시작 (`0` 아님)
- `m`: 검색 시작 위치 (`음수`일 경우 오른쪽부터 검색)
- `n`: 검색 순위 (찾고자 하는 몇 번째 위치인지 지정)

```sql
SELECT INSTR('CORPORATE FLOOR','OK') idx FROM DUAL; -- 0 (발견되지 않음)
SELECT INSTR('CORPORATE FLOOR','C'), INSTR('CORPORATE FLOOR','c') FROM DUAL; -- 대소문자 구분
```

```sql
-- 역방향 검색 예제
SELECT INSTR('CORPORATE FLOOR','OR', -2) idx FROM DUAL; -- 13 반환
```

---

### 문자열 결합 함수
#### CONCAT 함수
- `CONCAT(char1, char2)`: 두 개의 문자열을 결합
- `||` 연산자와 동일한 역할 수행

```sql
SELECT employee_id, first_name,
       CONCAT(employee_id, '번 사원명은'),
       employee_id || '번 사원명은' || first_name,
       CONCAT(employee_id, '번 사원명은') || CONCAT(first_name, '이다'),
       CONCAT(employee_id || '번 사원명은', CONCAT(first_name, '이다'))
FROM EMPLOYEES2;
```

---

### 문자 변환 함수
- **INITCAP**: 문자열의 첫 번째 문자를 대문자로 변환
- **LOWER**: 문자열을 소문자로 변환
- **UPPER**: 문자열을 대문자로 변환

```sql
SELECT email, INITCAP(email), 
       LOWER(email), 
       FIRST_NAME, UPPER(FIRST_NAME)
FROM EMPLOYEES2;
```

---

### 문자열 치환 함수
#### REPLACE 함수
- `REPLACE(char1, str1, str2)`: 문자열의 특정 문자를 다른 문자로 변환 (대소문자 구분)

```sql
SELECT REPLACE('oracleclub','oracle','db'), -- dbclub
       REPLACE('OracleClub','oracle','DB'), -- OracleClub (대소문자 구분)
       REPLACE('OracleClub','Oracle','DB')  -- DBClub
FROM DUAL;
```

#### TRANSLATE 함수
- `TRANSLATE(char1, str1, str2)`: 여러 개의 문자 단위로 변환 (1:1 매핑)

```sql
SELECT REPLACE('oracleclub','oracle','ABCDEF'),  -- ABCDEFclub
       TRANSLATE('oracleclub','oracle','ABCDEF') -- ABCDEFDEub
FROM DUAL;
```

