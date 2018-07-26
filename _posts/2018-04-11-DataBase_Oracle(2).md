---
title: DataBase_Oracle(2)
author: JinWon
layout: post
category: DB
---

# 연산자

- 오라클 언어(SQL)
- Java 와 같은 언어다 (연산자)
- Java 거의 동일 (% 나머지 >> 오라클 % 검색패턴으로 사용) >> 별도의 함수 나머지 Mod() 함수
- 산술(+ , - , * , / ....) Mod()함수

~~~sql
-- 사원테이블에서 사원의 급여를 100달러 인상한 결과를 출력하세요
select * from emp;
--1.컬럼의 타입(Type) >> number
desc emp;
select empno, ename, sal, sal+100 as "인상급여"
from emp;

select 100 + 100 from dual; -- 데이터 TEST 임시 dual;
select 100 || 100 from dual; -- 숫자 문자로 자동 형변환(결합)
select '100' + 100 from dual; -- 문자 숫자 형변환
select 'A100' + 100 from dual; -- 문자 숫자 형변환 했지만 숫자로 사용할 수 없음 ERROR
~~~

### 비교연산자
- < , > , <= ....
- Java 같다(==), 할당(=)
- Oracle (=) 같다, (!=) 같지 않다

### 논리 연산자
- AND, OR, NOT

~~~sql
SELECT [DISTINCT]  {*, column [alias], . . .}   
FROM  table_name   [WHERE  condition]  
[ORDER BY {column, expression} [ASC | DESC]]; 
~~~

### 조건절
- 조건절 (원하는 row 가지고 오겠다)
~~~sql
select *
from emp
where sal >= 3000;

select empno, sal
from emp
where  sal >= 3000;

--이상, 이하(=포함)
--초과, 미만

--사번이 7788번인 사원의 사번, 이름, 직종, 입사일을 출력하세요.
select empno, ename, job, hiredate
from emp
where empno = 7788;

--사원의 이름이 king인 사원의 사번, 이름, 급여 정보를 출력하세요
select empno, ename, sal
from emp
where ename = 'KING'; -- 대소문자 엄격하게 구분 (varchar2(20)...)
~~~

### 논리 (AND, OR, NOT)
~~~sql
--급여가 2000달러 이상이면서 직종이 manager 인 사원의 모든 정보를 출력하세요
select *
from emp
where sal >= 2000 and job = 'MANAGER';
~~~

### 튜닝 기본 (실행순서)
- select 3번
- from   1번
- where  2번

# 오라클 날짜
- (서버의 날짜)
- 시스템 (웹사이트) 제작 : 날짜 >> sysdate
- 게시판 글쓰기 : insert into board(writer, title, content, regdate)
    - values('홍길동', '방가방가', '피곤해요', sysdate);
> TIP) ms-sql >> select getdate()

~~~sql
select sysdate from dual;

select hiredate from emp;
desc emp;
~~~

- 오라클 시스템 정보를 담고 테이블
- 환경설정 정보
~~~sql
select * from  SYS.NLS_SESSION_PARAMETERS;
--NLS_DATE_FORMAT  ->	 RR/MM/DD
--NLS_DATE_LANGUAGE  ->	 KOREAN
--NLS_TIME_FORMAT  ->	 HH24:MI:SSXFF
select * from  SYS.NLS_SESSION_PARAMETERS where parameter = 'NLS_DATE_FORMAT';
~~~

- 형식을 변환
- 현재 접속한 사용자(session) 기준으로 적용
- 다른 곳에서 bituser로 다른 사용자가 접속하면  >> NLS_DATE_FORMAT  ->	 RR/MM/DD
~~~sql
alter session set nls_date_format='YYYY-MM-DD HH24:MI:SS';
--bituser를 접속해체하고 다시 접속하면 
--바뀐 값이 아닌 기본값으로 돌아간다. (RR/MM/DD)

select sysdate from dual;
select hiredate from emp;
~~~

### 오라클 날짜 표기 : '날짜'
~~~sql
--'1980-12-17' 인정 권장
select * from emp
where hiredate = '1980-12-17';
--'1980/12/17' 인정 권장
select * from emp
where hiredate = '1980/12/17';
--'1980.12.17' 인정
select * from emp
where hiredate = '1980.12.17';
--'19801217' 인정
select * from emp
where hiredate = '19801217';
--숫자만 맞으면 인정 권장을 사용하자
~~~

### Quiz
~~~sql
select * from emp
where hiredate = '80/12/17';--날짜 형식x :YYYY
--위 쿼리는 RR-MM-DD 형식에 맞은거

--사원의 급여가 2000달러 이상이고 4000달러 이하인 모든 사원의 정보
select * 
from emp
where sal >= 2000 and sal <= 4000;

--연산자 : 컬럼명 between A and B (=을 포함)
select * 
from emp
where sal between 2000 and 4000;

--사원의 급여가 2000달러 이상이고 4000달러 (미만)인 모든 사원의 정보
select *
from emp
where sal between 2000 and 3999;


--부서 번호가 10번 또는 20번 또는 30번인 사원의 사번, 이름, 급여, 부서번호를 출력하세요
select * 
from emp
where deptno = '10' or deptno = '20' or deptno = '30';
~~~

# IN 연산자 (조건 or 조건 or ....)
~~~sql
select * 
from emp
where deptno = '10' or deptno = '20' or deptno = '30';

--부서 번호가 10번 또는 20번이 아닌 사원의 사번, 이름, 급여, 부서번호를 출력하세요
--부정 !=
select empno, ename, sal, deptno
from emp
where deptno != 10 and deptno != 20;

--NOT IN 연산자 (and 부정값, and 부정값)
select empno, ename, sal, deptno
from emp
where deptno not in (10,20);
~~~

~~~
--------------------------------
--btween A and B / in / not in--
--------------------------------
~~~

# NULL

- POINT : 값이 없다(데이터가 없다) >> null
- 필요악 >> null
~~~sql
create table member(
 userid varchar2(20) not null,
 name varchar2(20) not null,
 hobby varchar2(20) --default null(null값을 허용하겠다) : 필수 입력사항이 아니다.
);

select * from member;
insert into member(userid,hobby) values('hong', '농구');--name not null;(에러)
insert into member(userid,name) values('hong', '홍길동');
insert into member values('jun', '전우치', null);
insert into member values('hong', '홍길동', '농구');
--insert into member(userid, name, hobby) values('hong','홍길동','농구'); 이것도 가능
commit; --실제반영

delete from emp;
select * from emp;
rollback;
~~~

~~~
------------------------
--null의 대한 비교
-- is null / is not null
------------------------
~~~
~~~
사원테이블에서 사번, 이름, 급여, 수당, 총급여를 출력하세요
총급여(급여 + 수당)

select empno, ename, sal, comm, sal+comm as "총급여" from emp;
총급여가 null값이 나올 수 있다.
~~~

### nvl(컬럼명, null값을 변경할 값)
~~~sql
select empno, ename, sal, comm,sal + nvl(comm, 0) as "총급여" from emp;
~~~

#### POINT  null
- null 과의 모든 연산은 그 결과가 null이다
- ☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆
- 위 문제를 해결 null 처리 함수 : nvl() ☆☆☆☆☆☆☆☆☆
- ☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆
- ms-sql : convert()
- my-sql : IFNULL()

~~~sql
select 1000 + null from dual;
select 1000 + nvl(null, 0) from dual;

select comm, nvl(comm, 0) from emp;
--문자로도 변경 가능
select 'data' || nvl(null, 'HIHI') from dual;

--사원의 급여가 1000이상이고 수당을 받지 않는 사원의 사번, 이름, 직종, 급여, 수당을 출력하세요
select empno, ename, job, sal, comm from emp where sal >= 1000 and comm is null;
~~~