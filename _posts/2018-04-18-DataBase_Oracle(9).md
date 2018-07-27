---
title: DataBase_Oracle(9)
author: JinWon
layout: post
category: DB
---

# view(가상테이블)
- 가상 테이블
- view 객체( create ... )
- create view 뷰이름 as [view가_보는_목록 (select 구문)]
- view 는 테이블처럼 사용 가능 (가상 테이블) -> 실제 emp, dept 같은 물리적인 테이블이 아니다.
- view 는 메모리상에만 존재하는 가상 테이블 (subquery -> in line view -> from (select..)
- view 는 sql문장 덩어리

### view 가상 테이블
- 사용법 : 일반 테이블과 사용법이 동일 from 절 where...
- DML( insert , update , delete ) - View 를 통해서 DML 가능 -> 실제 view를 통해 볼 수 있는 실제테이블 데이터 변경

###view 사용
- 개발자의 편리성 (join, subquery) 쉽게 접근 사용 (가상 테이블)
- 복잡한 쿼리 단순화
- 보안 (권한 별로 처리) -> 노출하고 싶은 데이터만 모아서 view 생성 -> 제공

~~~sql
create view v_001
as
  select empno, ename from emp;
  
select * from v_001;
select * from v_001 where empno=7788;

create view v_002
as
  select e.empno, e.ename, e.deptno, d.dname
  from emp e join dept d
  on e.deptno = d.deptno; -- 복잡한 쿼리를 단순화 할 수 있다.

select * from v_002;

create view v_003
as
  select deptno, avg(sal) as avgsal
  from emp
  group by deptno;

select * from v_003;
select *
from emp e join  v_003 s
on e.deptno = s.deptno
where e.sal > s.avgsal;
~~~

- 만약 원하는 데이터가 테이블로 존재한다면
- join 하기 편할텐데 >> in line view , view

~~~
CREATE [OR REPLACE] [FORCE | NOFORCE] VIEW view_name [(alias[,alias,...])]
AS Subquery
[WITH CHECK OPTION [CONSTRAINT constraint ]]
[WITH READ ONLY]

OR REPLACE 이미 존재한다면 다시 생성한다.
FORCE Base Table 유무에 관계없이 VIEW 을 만든다.
NOFORCE 기본 테이블이 존재할 경우에만 VIEW 를 생성한다.
view_name VIEW 의 이름
Alias Subquery 를 통해 선택된 값에 대한 Column 명이 된다.
Subquery SELECT 문장을 기술한다.
WITH CHECK OPTION VIEW 에 의해 액세스 될 수 있는 행만이 입력,갱신될 수 있다.
Constraint CHECK OPTON 제약 조건에 대해 지정된 이름이다.
WITH READ ONLY 이 VIEW 에서 DML 이 수행될 수 없게 한다.
~~~

~~~sql
--1.create or replace v_007 as 구문...(수정, overwirte)
--view 는 컬럼명이 필요하다 as ""

create or replace view v_emp
as
  select empno, ename, deptno from emp where deptno = 20;

select * from v_emp;
select * from v_emp where deptno = 10; -- 볼 수 없다

--view 가상 테이블 >> DML(insert, update, delete ....)
--view [통한] 바라볼 수 있는 데이터에 대해서 DML 가능
--단일 테이블만 (DML) >> view를 통해서...

delete from v_emp;--deptno번호가 20인 사원들

select * from emp;--실제로는 emp 테이블 >> deptno번호가 20번인 사원 삭제
select empno, ename, deptno from emp where deptno = 20;
rollback;
~~~

# 시퀀스 객체 
- 순번 추출 (채번기)
- 자동으로 번호를 생성하는 객체

~~~
CREATE  SEQUENCE  sequence_name  
[INCREMENT  BY  n]  
[START  WITH  n]  
[{MAXVALUE n | NOMAXVALUE}]  
[{MINVALUE n | NOMINVALUE}]  
[{CYCLE | NOCYCLE}]  
[{CACHE | NOCACHE}];

sequence_name  SEQUENCE 의 이름입니다. 
INCREMENT  BY  n 정수 값인 n 으로 SEQUENCE 번호 사이의 간격을 지정. 
이 절이 생략되면 SEQUENCE 는 1 씩 증가. 
START  WITH  n  생성하기 위해 첫번째 SEQUENCE 를 지정. 
이 절이 생략되면 SEQUENCE 는 1 로 시작. 
MAXVALUE n  SEQUENCE 를 생성할 수 있는 최대 값을 지정. 
NOMAXVALUE   오름차순용 10^27 최대값과 내림차순용-1 의 최소값을 지정. 
MINVALUE n  최소 SEQUENCE 값을 지정. 
NOMINVALUE  오름차순용 1 과 내림차순용-(10^26)의 최소값을 지정. 
CYCLE | NOCYCLE  최대 또는 최소값에 도달한 후에 계속 값을 생성할 지의 여부를 
    지정. NOCYCLE 이 디폴트. 
CACHE | NOCACHE  얼마나 많은 값이 메모리에 오라클 서버가 미리 할당하고 유지 
    하는가를 지정. 디폴트로 오라클 서버는 20 을 CACHE.  
~~~

~~~sql
--게시판
/*
create table board(
  boardid number primary key,
  title varchar2(50)......
);
boardid >> 중복값, null 값이 (x)
개발자 : 데이터를 insert 할 때 중복값이 아니라는 것에 대한 보장...

where boardid = 10; 하나의 row 를 return 하는 것을 보장

게시판 글쓰기
insert into board (...) values(, '홍길동','방가방가')

*/

create table kboard(
  num number constraint pk_kboard_num primary key,
  title varchar2(50)
);

--처음 글을 쓰면 1 이라는 값이 , 그 다음 부터 글을 쓰면 2, 3, 4 ... 순차적인 값

--처음 글 >> 1
insert into kboard(num, title)
values((select nvl(max(num), 0)+1 from kboard), '처음');
--다음 글 부터 >> 2, 3, 4 ...
insert into kboard(num, title)
values((select nvl(max(num), 0)+1 from kboard), '두번째');

insert into kboard(num, title)
values((select nvl(max(num), 0)+1 from kboard), '세번째');

commit;

select * from kboard;

delete from kboard where num=1;

insert into kboard(num, title)
values((select nvl(max(num), 0)+1 from kboard), '네번째');
~~~

~~~
----------------------------------------
번호를 추출 중복값이 안나오게 순차적인 값
----------------------------------------
~~~

### sequence 객체
~~~sql
create sequence board_num;

select * from user_sequences;
~~~

> 1.4.1 NEXTVAL 과 CURRVAL 의사열 
가) 특징 
1. NEXTVAL 는 다음 사용 가능한 SEQUENCE 값을 반환 한다. 
2. SEQUENCE 가 참조될 때 마다, 다른 사용자에게 조차도 유일한 값을 반환한다. 
3. CURRVAL 은 현재 SEQUENCE 값을 얻는다. 
4. CURRVAL 이 참조되기 전에 NEXTVAL 이 사용되어야 한다. 

~~~sql
--실채번
select board_num.nextval from dual;

--현재값...(채번 하지 않고 정보만)
select board_num.currval from dual;

create table tboard(
  num number constraint pk_tboard_num primary key,
  title varchar2(50)
);

create sequence tboard_num;

insert into tboard(num, title) values(tboard_num.nextval, '글쓰기');

select tboard_num.currval from dual; -- nextval 을 한번도 하지 않았기 때문에 에러

select * from tboard;

delete from tboard where num = 5;

insert into tboard(num, title) values(tboard_num.nextval, '5번 삭제 후 생성');

commit;
~~~

- sequence 는 rollback 되는 자원이 아니다!!!

~~~
게시판(10개)
상품 게시판, 관리자 게시판, 회원게시판 .....
테이블 10개 순번은 sequence 객체 하나로 사용해도 된다...
~~~

~~~sql
--TIP
--ms-sql : create table board(boardnum int identity(1,1) , title varchar(20))
--         insert into board(title) values('제목')
--2012버전 부터 : 오라클 처리 sequence 를 사용 가능

--my-sql : create table board(boardnum int auto_increment , title varchar(20))
--         insert into board(title) values('제목')

create sequence seq_num
start with 10
increment by 2;

select seq_num.nextval from dual;
select * from user_sequences;
~~~

### 오라클 sequence
- 100건 insert -> sequence -> 100번 채번 -> 50건 채번 -> insert하면 101번 생성
- 1,2,3,4,5,6......50,101 > 51건

### 조회 (게시판)
- 최신글을 먼저 ... 나중에 쓴 글이 먼저
- 글번호 (순번)
~~~
최신글 순으로 보여 주세요
select * from board order by boardunm desc;
     (화면출력)
101   --51
50    --50
49    --49
48    --48
...
~~~

### 개발자 (sequence , rownum)
- rownum 의사 컬럼 : 실제 물리적으로 존재하는 컬럼이 아니고 논리적인 존재
- rownum : 실제로 테이블에 존재하지 않는 컬럼(행 번호 부여)
- rowid : 주소값(행이 실제로 저장되는 내부 주소값) -> 인덱스 만들어 질때 사용...

~~~sql
select empno, ename from emp;

select rownum as 순번, empno from emp;
~~~

### Top-n 쿼리
- 테이블에서 조건에 맞는 상위 (TOP) 레코드에 n개 추출 ...
- 근거 : 정렬 (기준)

--- 
- ms-sql : select top 10 --> select top 10, * from emp order by sal desc

- oracle : rownum(의사컬럼) : 기준이 필요하다

1. 정렬 쿼리를 만든다
2. 쿼리에 순번을 부여하고 조건절 제시

~~~sql
--1단계 정렬쿼리
select e.*
from (
      select * from emp order by sal desc
     ) e;

--2단계
select rownum, e.*
from (
      select * from emp order by sal desc
     ) e
where rownum <= 10;
~~~

~~~
select rownum as "순번", e.* --select 한 결과에 순번을 붙이는 것
from (
      select * from emp order by sal desc
     ) e
where "순번" < 5; --불가!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
~~~

~~~sql
select e.* --select 한 결과에 순번을 붙이는 것
from (
      select * from emp order by sal desc
     ) e
where rownum < 5; --내부적인 처리 순서 때문에 크다는 표현 (row num 다시 만들어서)
~~~

---

~~~sql
-------------------------------
--10 보다 큰 번호 출력
select * 
from 
(
  select  rownum as n ,e.*
  from (
        select *
        from emp order by sal desc
       ) e
) a 
where  n  > 10; 

------------------------------------------------------------------------
--게시판(페이징 처리 원리)
--100건의 데이터(row)

--총 100건 
--pagesize (ms-sql : page, oracle : bolock?(볼락))
--pagesize = 10 정의 (화면 한 페이지에 볼 수 있는 데이터 건수 10개)
--건수 / pagesize -> 페이지의 개수 -> 10개 -- 104건이라면 => 11개
--pagecount => 10개

--1~100
--1~10    11~20   ...   91~100
--1page   2page   ...   10page
~~~