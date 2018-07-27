---
title: DataBase_Oracle(4)
author: JinWon
layout: post
category: DB
---

# 연산자

- 합집합(union) : 테이블과 테이블의 데이터를 합치는 것 (중복값은 배제)
- 합집합(union all) : 중복값 허용

~~~sql
create table UTA(name varchar2(20));
insert into uta values('AAA');
insert into uta values('BBB');
insert into uta values('CCC');
insert into uta values('DDD');
commit;
select * from uta;


create table UT(name varchar2(20));
insert into ut values('AAA');
insert into ut values('BBB');
insert into ut values('CCC');
commit;
select * from ut;

select * from ut
union --중복 값 배제
select * from uta;

select * from ut
union all --중복 값 허용
select * from uta;
~~~

#  union 규칙

1. 대응되는 컬럼의 타입이 동일
~~~sql
select empno, ename from emp
union
select dname, deptno from dept; -- 에러

select empno, ename from emp
union
select deptno, dname from dept;

--실무 > subquery 사용해서 unoin 한 테이블 가상테이블 처럼 사용
select empno, ename
from (
 select empno, ename from emp
 union
 select deptno, dname from dept
) order by empno desc;
~~~

2. 대응되는 컬럼의 개수가 동일 (null 착한일)

~~~sql
select empno, ename, job, sal from emp
union
select deptno, dname, loc, null from  dept;
-----
select empno, ename, job, sal from emp
union
select deptno, dname, loc, nvl(null, 0) from  dept;
~~~