---
title: DataBase_Oracle(3)
author: JinWon
layout: post
category: DB
---

- DQL (data query language) : select
- DDL(데이터 정의어) : create, alter, drop : 객체생성, 수정, 삭제

- class Board{private int boardid;}
~~~sql
create table board ( 
 boardid number not null, --필수 입력(숫자)
 title varchar2(20) not null, --한글 한 자 2byte >> 10자, 영문자 특수문자 공백 >> 20자
 content varchar2(2000) not null,
 hp varchar2(20)
);

desc board;

--insert, update, delete 작업시에는 (rollback, commit) 반드시 수행

INSERT INTO board(boardid, title, content) 
VALUES(100, '오라클', '참 쉽네'); 

select * from board;
--실반영 : commit
--취소 : rollback
commit;

insert into board(boardid, title, content)
values(200, '자바', '그립다...');

select * from board;
--실수했다 이거 아니야..
rollback;

insert into board(boardid, title, content, hp)
values(200, '자바','그립다...','010...');

commit;
~~~

---

- 문자열 검색
- 우편번호 검색
- 주소검색 : '역삼' 검색하면 역삼이란 글자가 들어있는 모든 문장
- 문자열 패턴 검색 (like 연산자)
---
- like 연산자 (와일드 카드 문자 (% : 모든 것 , _ : 한문자) 결합..
select boardid, nvl(hp,'EMPTY') as "hp" from board;
- A가 들어있는 이름을 모두 검색
---
- 'A%' A로 시작하는
- '%A' A로 끝나는
---
~~~sql
-- LL이 들어있는
select *
from emp
where ename like '%LL%';

--A라는 글자가 붙어있던 띄어있던 A가 두번 들어있는
select *
from emp
where ename like '%A%A%';

--A가 두번째 단어인 사람
select ename
from emp
where ename like '_A%';

--글자기 5자 이고 S가 마지막에 끝나는 사람의 이름
select ename
from emp
where ename like '____S';

--오라클 과제(regexp_like)
select * from emp where REGEXP_LIKE (ename, '[A-C]');
--정규 표현식 (표준 : java , oracle , script ...)
--조별 스터디 (중간 프로젝트 : 검증 (정규표현식))
~~~

---

- 데이터 정렬하기
- order by 컬럼명 : 문자 , 숫자 , 날짜
- 오름차순 : asc 낮은순
- 내림차순 : desc 높은순

~~~sql
--오름차순 (낮은순)
select * from emp order by sal;
select * from emp order by sal asc;
--내림차순 (높은순)
select * from emp order by sal desc;

--문자열도 적용가능
select ename from emp order by ename;

--입사일이 가장 늦은 순으로 정렬해서 사번, 이름, 급여, 입사일 출력
select empno, ename, sal, hiredate from emp order by hiredate desc;
~~~

~~~
실행순서
select   3
from     1
where    2
order by 4(select결과를 정렬)
~~~