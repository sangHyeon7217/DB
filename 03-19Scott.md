```sql
-- system 계정 접속
conn SCOTT/1234;

-- scott 계정 role 부여 및 비밀번호 변경
grant connect, resource, unlimited tablespace to SCOTT identified by tiger;

-- SCOTT 계정 접속 후 DEFAULT TABLESPACE와 TEMPORARY TABLESPACE 설정
alter user SCOTT default tablespace USERS;
alter user SCOTT temporary tablespace TEMP;

-- SCOTT 계정 접속
connect SCOTT/TIGER;

-- DEPT 테이블 삭제
drop table DEPT;

-- DEPT 테이블 생성 : 부서 정보
create table DEPT (
    DEPTNO NUMBER(2) constraint PK_DEPT primary key, -- 부서번호
    DNAME VARCHAR2(14), -- 부서명
    LOC VARCHAR2(13)  -- 위치
);

-- EMP 테이블 삭제
drop table EMP;

-- EMP 테이블 생성 : 사원 정보
create table EMP (
    EMPNO NUMBER(4) constraint PK_EMP primary key, -- 사원번호
    ENAME VARCHAR2(10), -- 사원명
    JOB VARCHAR2(9), -- 업무
    MGR NUMBER(4), -- 상사의 사원번호
    HIREDATE DATE, -- 입사일자
    SAL NUMBER(7,2), -- 급여
    COMM NUMBER(7,2), -- 커미션
    DEPTNO NUMBER(2) constraint FK_DEPTNO references DEPT -- 부서번호
);

-- DEPT 테이블에 부서 정보 입력
insert into DEPT values (10, 'ACCOUNTING', 'NEW YORK');
insert into DEPT values (20, 'RESEARCH', 'DALLAS');
insert into DEPT values (30, 'SALES', 'CHICAGO');
insert into DEPT values (40, 'OPERATIONS', 'BOSTON');

-- EMP 테이블에 사원 정보 입력
insert into EMP values (7369, 'SMITH', 'CLERK', 7902, to_date('17-12-1980', 'dd-mm-yyyy'), 800, NULL, 20);
insert into EMP values (7499, 'ALLEN', 'SALESMAN', 7698, to_date('20-2-1981', 'dd-mm-yyyy'), 1600, 300, 30);
insert into EMP values (7521, 'WARD', 'SALESMAN', 7698, to_date('22-2-1981', 'dd-mm-yyyy'), 1250, 500, 30);
insert into EMP values (7566, 'JONES', 'MANAGER', 7839, to_date('2-4-1981', 'dd-mm-yyyy'), 2975, NULL, 20);
insert into EMP values (7654, 'MARTIN', 'SALESMAN', 7698, to_date('28-9-1981', 'dd-mm-yyyy'), 1250, 1400, 30);
insert into EMP values (7698, 'BLAKE', 'MANAGER', 7839, to_date('1-5-1981', 'dd-mm-yyyy'), 2850, NULL, 30);
insert into EMP values (7782, 'CLARK', 'MANAGER', 7839, to_date('9-6-1981', 'dd-mm-yyyy'), 2450, NULL, 10);
insert into EMP values (7788, 'SCOTT', 'ANALYST', 7566, to_date('13-JUL-87') - 85, 3000, NULL, 20);
insert into EMP values (7839, 'KING', 'PRESIDENT', NULL, to_date('17-11-1981', 'dd-mm-yyyy'), 5000, NULL, 10);
insert into EMP values (7844, 'TURNER', 'SALESMAN', 7698, to_date('8-9-1981', 'dd-mm-yyyy'), 1500, 0, 30);
insert into EMP values (7876, 'ADAMS', 'CLERK', 7788, to_date('13-JUL-87') - 51, 1100, NULL, 20);
insert into EMP values (7900, 'JAMES', 'CLERK', 7698, to_date('3-12-1981', 'dd-mm-yyyy'), 950, NULL, 30);
insert into EMP values (7902, 'FORD', 'ANALYST', 7566, to_date('3-12-1981', 'dd-mm-yyyy'), 3000, NULL, 20);
insert into EMP values (7934, 'MILLER', 'CLERK', 7782, to_date('23-1-1982', 'dd-mm-yyyy'), 1300, NULL, 10);

-- BONUS 테이블 삭제
drop table BONUS;

-- BONUS 테이블 생성
create table BONUS (
    ENAME VARCHAR2(10), -- 사원명
    JOB VARCHAR2(9), -- 업무
    SAL NUMBER, -- 급여
    COMM NUMBER -- 커미션
);

-- SALGRADE 테이블 삭제
drop table SALGRADE;

-- SALGRADE 테이블 생성 : 급여 등급
create table SALGRADE (
    GRADE NUMBER, -- 등급
    LOSAL NUMBER, -- 최저 급여
    HISAL NUMBER -- 최고 급여
);

-- SALGRADE 테이블에 데이터 입력
insert into SALGRADE values (1, 700, 1200);
insert into SALGRADE values (2, 1201, 1400);
insert into SALGRADE values (3, 1401, 2000);
insert into SALGRADE values (4, 2001, 3000);
insert into SALGRADE values (5, 3001, 9999);

commit;
```

