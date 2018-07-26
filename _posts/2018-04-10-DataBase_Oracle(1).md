---
title: DataBase_Oracle(1)
author: JinWon
layout: post
category: DB
---

# 설치

1. 오라클 소프트웨어 다운로드
    * http://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/index.html

2. 11g express 버전 (무료버전) 설치

3. 설치 (관리자 권한 : SYSTEM, SYS 계정 : 암호설정 >> 1004

4. sqlplus 기본 프로그램 접속확인

5. SqlDeveloper 무료 툴을 설치
    * 유료 툴 -> 토드 , 오렌지 , sqlgate

6. Tool을 통해서 Oracle 접속
    * HR 계정 암호 1004 >> unlock
    * BITUSER, 1004 >> 계정 생성

~~~sql
-- USER SQL
CREATE USER bituser IDENTIFIED BY 1004 
DEFAULT TABLESPACE "USERS"
TEMPORARY TABLESPACE "TEMP";

-- QUOTAS
ALTER USER kosta QUOTA UNLIMITED ON USERS;

-- ROLES
GRANT "CONNECT" TO bituser ;
GRANT "RESOURCE" TO bituser ;

-- SYSTEM PRIVILEGES
~~~

# Select

~~~sql
--1. 사원테이블에서 모든 데이터를 출력하세요
select * from emp;
SELECT * FROM EMP; -- 쿼리문은 대소문자 구별x

--2. 특정 컬럼 데이터 추출하기
select empno, ename, job, sal from emp;
select empno from emp;
select hiredate, sal, comm from emp;

--3. 컬럼에 가명칭(alias) 부여하기
select empno 사번, ename 이름
from emp;

select empno "사  번", ename "이            름"
from emp;

--정식(ansi 문법) >> 권장
select empno as "사  번", ename as "이            름"
from emp;
~~~

- Oracle : 문자열(문자데이터 엄격하게 대소문자 구분)
- 소문자 : a, 대문자 : A 다른 문자 취급
~~~sql
select * from emp where ename = 'king'; --x
select * from emp where ename = 'KING'; --o
select * from emp where ename = 'King'; --x
~~~

- Oracle : 연산자(결합 연산자) >> || >> 'hello' || 'world' >> 'helloworld;
- Java : +연산자 (숫자+숫자 = 연산)
- Java : +연산자 (문자+문자 = 결합)
- TIP) ms-sql : + 연산, 결합

~~~sql
select '사원의 이름은' || ename || '입니다' as "ename"
from emp;
~~~

### POINT 테이블이 가지는 컬럼의 기본 타입
- 컬럼이 숫자타입일까, 문자타입일까
- 가장 기본적인
- desc emp;

### 형변환 가능
~~~sql
select '사원의 이름은' || sal || '입니다' as "ename"
from emp;

select empno || ename as "결합" --내부적으로 자동 형변환(숫자 -> 문자)
from emp;

select empno + ename as "결합" --실제 연산 작업 Error > "invalid number"
from emp;
~~~

### DISTINCT (중복제거)
##### 사장님 우리회사에 직종이 몇개나 있나?
- 중복 데이터 제거 : 키워드 (distinct)
~~~sql
select distinct job from emp;

select distinct deptno from emp;

--distinct 의 원리 (컬럼이 1개 이상)... grouping 
select distinct job, deptno from emp order by job;
select distinct deptno, job from emp order by deptno;
~~~

