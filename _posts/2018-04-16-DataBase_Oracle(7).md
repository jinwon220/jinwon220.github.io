---
title: DataBase_Oracle(7)
author: JinWon
layout: post
category: DB
---

# SubQuery

> 쿼리의 작성 기준 <br>
함수 >>> 조인 >>> subquery (마지막 무기)


### 종류
1. single row subquery : 서브쿼리의 결과가 1개의 row(단일값) : 한개의 값 (단일 컬럼)

2. multi row subquery : 서브쿼리의 결과가 1개 이상의 row : 여러개의 값 (단일 컬럼)

#####  구분하는 이유 (사용되는 연산자가 차이)
- multi row 
- 연산자 (in, not in) (any, all) >> 다중 데이터
- all : sal > 1000 and  sal > 2000 and ....
- any : sal > 1000 or sal > 2000 or ....

### 문법(정의)
1. subquery 는 괄호안에 있어야 한다 -> (select sal from emp)
2. subquery 는 단일 컬럼으로 구성 -> (select sal, deptno from emp) (X) 불가
3. subquery 는 단독으로 실행 가능해야 한다

### subquery 를 가지고 있는 sql문장
1. subquery 먼저 실행
2. subquery 의 결과값을 가지고 main쿼리 실행한다.

~~~sql
--사원테이블에서 jones 의 급여보다 더 많은 급여를 받는 사원의 사번, 이름, 급여를 출력
select empno, ename, sal
from emp
where sal > (select sal from emp where ename = 'JONES');

--
select * from emp
where sal in(select sal from emp where sal > 2000);
--where sal = 2975 or sal = 2850 or sal = 2450 or ....

select * from emp
where sal not in(select sal from emp where sal > 2000);

--부하직원이 있는 사원의 사번과 이름을 출력하세요
select empno, ename
from emp
where mgr in(select mgr from emp)
order by empno;

select empno, ename
from emp where mgr is not null
order by empno;

select * from emp;
--부하직원이 없는 사원

select empno, ename, mgr
from emp
where empno not in(select nvl(mgr, 0) from emp);

--king 에게 보고하는 즉 직속상관이 king 인 사원의 사번, 이름, 직종, 관리자 사번을 출력
select empno, ename, job, mgr
from emp
where mgr in (select empno from emp where ename = 'KING');
~~~

### 집계함수가 subquery 활용...
~~~sql
select *
from emp
where deptno in(select deptno from emp where job = 'SALESMAN')
and sal in(select sal from emp where job = 'SALESMAN');
~~~

---

# [INSERT, UPDATE, DELETE]
~~~
오라클 기준
DDL(데이터 정의어) : create, alter, drop, truncate ( rename, modify )
DML(데이터 조작어) : insert, update, delete 
DQL(데이터 질의어) : select
DCl(데이터 제어어) : 권한 ( grant , revoke )
TCl(트렌잭션) : commit, rollback, savepoint --> 모든게 성공이거나 실패 이다.
~~~

### DML 작업
### 트랜잭션 (transaction) : 하나의 논리적인 작업단위
- 트랜잭션으로 처리되어야 하는 업무
- 은행업무라면....(A라는 계좌에서 돈을 출금 B라는 계좌에 이체)
- [A 출금 ~~~~~~~~ B 이체] 하나의 업무 : 결과 -> 성공, 실패
- commit (A ~ B 가 예외없이 실행되면), rollback (A ~ B 진행중 예외가 발생하면)

~~~
A라는 계좌 (100만원 출금) : update ....
...
...
B라는 계좌 (100만원 입금) : update ....
~~~

~~~sql
--테이블 기본 정보
desc emp;
--오라클 (system table(데이터 사전) 통해서 다양한 정보)
select * from tab; --현재 접속한 계정 (bituser) 볼 수 있는 테이블 목록

select * from tab where tname = 'BOARD'; --테이블생성 전에 그 정보를 확인

select * from col; --bituser 사용자가 관리하는 모든 컬럼 정보

select * from col where tname = 'EMP'; --특정 테이블이 가지는 컬럼 정보

select * from user_tables; -- 관리자, 튜닝 정보...
select * from user_tables where table_name = 'DEPT';
~~~

---

### DML

##### INSERT
~~~sql
INSERT INTO table_name [(column1[, column2, . . . . . ])]
VALUES (value1[, value2, . . . . . . ]);
~~~

~~~sql
create table temp(
 id number primary key, --id 컬럼에 null(x), 중복데이터(x), 유일한 데이터 1건만 보장
 name varchar2(20) -- default null 허용
);
~~~

---

~~~sql
--1. 가장 일반적인...
insert into temp(id, name)
values(100, '홍길동');

select * from temp;
--반영
commit;

--2. 컬럼 리스트 생략(temp(id, name)) -> temp 조건 -> 모든 컬럼에 데이터 삽입시
insert into temp --컬럼리스트 생략
values(200, '김유신');

select * from temp;
commit;
~~~

### ERROR
~~~sql
--1. error
insert into temp(id, name)
values(100, '아무개'); -- 넣을 수 없음 이미 기본키인 id에 100이라는 데이터가 들어있기 때문에
--unique constraint (BITUSER.SYS_C007003) violated --> 기본키(pk) 제약 위반

--2. error
insert into temp(name)
values('아무개'); --id는 기본키(pk)(중복(x), null(x))
--cannot insert NULL into ("BITUSER"."TEMP"."ID") --> 기본키(pk) null(x)
~~~

---

### insert 데이터 어느정도
- 일반 SQL 은 제어문 없어요

- 재미^^
- pl-sql
~~~sql
create table temp2(id varchar2(20));
~~~
~~~sql
begin
 for i in 1..1000 loop
  insert into temp2(id) values('A'||to_char(i));
 end loop;
end;
~~~

---

# 옵션 TIP)

1. 대량 데이터 INSERT 하기(1건 이상)
~~~sql
create table temp4(id number);
create table temp5(num number);

insert into temp4(id) values(1);
insert into temp4(id) values(2);
insert into temp4(id) values(3);
insert into temp4(id) values(4);
insert into temp4(id) values(5);
insert into temp4(id) values(6);
insert into temp4(id) values(7);
insert into temp4(id) values(8);
insert into temp4(id) values(9);
insert into temp4(id) values(10);
commit;

--요구사항 : temp4 테이블에 있는 모든 데이터를 temp5에 넣고 싶어요.
--insert into 테이블명 (컬럼리스트) select 구문 (values 구문 대신에)
--단, 컬럼개수 타입이 일치 한다면...

insert into temp5(num)
select id from temp4;
--values

select * from temp5;
commit;
~~~

2. Insert
    - 테이블이 없는 상황에서 [테이블자동생성][대량 데이터 삽입]
    - 단, 제약 정보는 복사가 안되요(PK(기본키), FK(외래키) ... )

~~~sql
--emp 테이블과 같은 구조를 가지고 같은 데이터를 갖는 실습 테이블
create table copyemp
as select * from emp;

select * from copyemp;
--
create table copyemp2
as select empno, ename, job, sal from emp where deptno = 30;

select * from copyemp2;

--질문) 구조(틀)만 복사하고
--     데이터는 복사하고 싶지 않아요
create table copyemp3
as select * from emp where 1=2;
--select * from emp where 1=1; 1=1은 항상 참
--select * from emp where 1=2; 1=2는 항상 거짓

select * from copyemp3;
--주의 사항 (단점)
--시스템 테이블 사용 (제약 정보 확인하기)
select * from user_constraints where table_name = 'EMP';
select * from user_constraints where table_name = 'COPYEMP';

create table pktest(id number primary key);
insert into pktest(id) values(100);
commit;

create table pktest2
as select * from pktest;

select * from pktest;
select * from pktest2;

select * from user_constraints where table_name = 'TEMP';
select * from user_constraints where table_name = 'PKTEST'; --제약조건이 있음
select * from user_constraints where table_name = 'PKTEST2'; --제약조건은 복사가 안됨
~~~

----------[INSERT END]----------

# [UPDATE]

~~~sql
UPDATE table_name
SET column1 = value1 [,column2 = value2, . . . . . . .]
[WHERE condition];
~~~

~~~sql
UPDATE table_name
 SET (column1, column2, . . . . ) = ( SELECT column1,column2, . . .
                                      FROM table_name
                                      WHERE coundition)
[WHERE condition];
~~~

~~~sql
update copyemp
set sal = 0;

select * from copyemp;

rollback;

update copyemp
set job = 'NOT...'
where deptno = 20;

select * from copyemp order by deptno;
rollback;

--update (subquery 사용 : 데이터 변경)
update copyemp
set sal = (select sum(sal) from copyemp);

select * from copyemp;
rollback;

update copyemp
set ename = 'AAAA', job = 'BBBB', hiredate = sysdate
where deptno = 10;

select * from copyemp where deptno = 10;

rollback;
~~~

----------[UPDATE END]----------

# [DELETE]
~~~sql
delete from copyemp;

select * from copyemp;

rollback;

delete from copyemp where deptno = 20;

select * from copyemp where deptno=20;

rollback;
~~~

----------[DELETE END]----------

- APP(JAVA) ---> JDBC API ---> ORACLE
### CRUD작업
- Create : insert
- Read : select (전체조회, 조건조회(row 1건))
- Update : update
- Delete : delete
- (Create, Update, Delete) 트랜잭션을 동반(commit, rollback)

~~~
--전체조회, 조건조회, 삭제, 수정, 삽입 (개발자)
--함수 최소 5개
public List<Emp> getEmpList(){ 안에 쿼리문 select * from emp }
public Emp getEmpListByEmpno(int empno){ select * from emp where empno = ..}
public int inserEmp(Emp emp)(insert into emp values(...)}

위 작업 : JDBC
~~~