---
title: DataBase_Oracle(8)
author: JinWon
layout: post
category: DB
---

# [DDL] create, alter, drop (기준은 테이블)

~~~sql
select * from user_tables where lower(table_name) = 'board';
select * from board;
drop table board;
create table board(
  boardid number,
  title varchar2(500),
  content varchar2(2000),
  regdate date
);

desc board;

--편한 방법 아래 2개 정도 ,,,,,,,,,,
--board 라는 테이블 이 있나?
select * from user_tables where lower(table_name) = 'board'; 
--board 라는 테이블에 제약조건이 있는 타입이 있는가?
select * from user_constraints  where lower(table_name) = 'board'; 

--Oracle 11g 가상컬럼(조합 컬럼)
--학생성적 : 국어, 영어, 수학, [총점] 컬럼이 있어요
--국어, 영어, 수학 데이터만 insert 되면 (국어 + 영어 + 수학)

create table vtable(
 no1 number,
 no2 number,
 no3 number GENERATED ALWAYS as (no1 + no2) VIRTUAL
);

insert into vtable(no1, no2) values(100, 50);
insert into vtable(no1, no2) values(80, 60);

select * from vtable;

--no3 컬럼에 데이터 직접 넣을 수 있을 까요
insert into vtable(no1, no2, no3) values(80, 60, 200); --불가 ERROR
--가상 테이블에는 직접 입력 불가!!!!!!

select COLUMN_NAME, DATA_TYPE, DATA_DEFAULT
from user_tab_columns where table_name = 'VTABLE';

--실무에 사용되는 형식의 코드
--제품정보 (입고일) 기준으로 분기데이터 (4분기)
--입고일 : 2018-03-02 --> 1분기

create table vtable2(
  no number, --순번
  p_code char(4), --제품코드(A001, B002...)
  p_date char(8), --입고일(20180202)
  p_qty number, --수량
  p_bungi number(1) --가상컬럼
          GENERATED ALWAYS as (
            CASE WHEN substr(p_date,5,2) in ('01','02','03') THEN 1
                 WHEN substr(p_date,5,2) in ('04','05','06') THEN 2
                 WHEN substr(p_date,5,2) in ('07','08','09') THEN 3
                 ELSE 4
            END
          ) VIRTUAL
);

insert into vtable2(p_date) values('20180102');
insert into vtable2(p_date) values('20180126');
insert into vtable2(p_date) values('20180403');
insert into vtable2(p_date) values('20181101');
insert into vtable2(p_date) values('20181201');
commit;

select * from vtable2;
~~~

---

# DDL 테이블 다루기...(제약정보)....

1. 테이블 생성하기

~~~sql
create table temp6(id number);
~~~

2. 테이블 생성했는데 컬럼 추가하기
    - 기존 테이블에 컬럼 추가하기

~~~sql
desc temp6;

alter table temp6
add ename varchar2(20);

desc temp6;
~~~

3. 기존 테이블에 있는 컬럼의 이름 잘못표기(ename -> username)
    - 기존 테이블에 있는 컬럼의 이름 바꾸기 (rename)

~~~sql
alter table temp6
rename column ename to username;

desc temp6;
~~~

4. 기존 테이블에 있는 컬럼의 Type 수정
    - (modify)

~~~sql
alter table temp6
modify(username varchar2(200));

desc temp6;
~~~

5. 기존 테이블에 있는 기존 컬럼 삭제하기

~~~sql
alter table temp6
drop column username;

desc temp6;
~~~

6. 테이블 데이터 삭제 : delete
    - 테이블 처음 만들면 처음 크기 -> 데이터 넣으면 -> 데이터 크기
    - ex) 처음 크기 1M -> 10만건 -> 크기+100M = 101M -> 10만건 delete -> 크기 101M
    - 테이블 데이터 삭제, 공간의 크기를 줄일 수있을까?
    - truncate(where 조건 사용 X)
    - ex) 처음 크기 1M -> 10만건 -> 크기+100M = 101M -> truncate -> 현재 테이블 크기 1M

~~~sql
drop table temp6;

desc temp;
~~~

---
# [테이블 제약 설정하기]

- 데이터베이스 무결성 확보
- 제약 ( constraint ) : insert, update 주로 적용

~~~
*NOT NULL(NN) : 열은 NULL 값을 포함할 수 없습니다.

*UNIQUE key(UK) : 테이블의 모든 행을 [유일하게 하는 값]을 가진 열(NULL을 허용)
  unique 제약은 null 값을 가질 수 있다 -> null 값을 못하게 하려면 (not null을 포함 해줘야함)

*PRIMARY KEY(PK) 유일하게 테이블의 각행을 식별(NOT NULL 과 UNIQUE 조건을 만족)
  not null 하고 unique 한 제약 ( 내부적으로 index 가 자동 설정 )
  index => 검색의 속도를 상향 시킴( 책 맨 뒷장의 (단어 + ?page) 생각하면 됨 )
  
*FOREIGN KEY(FK) 열과 참조된 열 사이의 외래키 관계를 적용하고 설정합니다.
  (참조제약) [테이블]과 [테이블]간의 관계설정
  
*CHECK(CK) 참이어야 하는 조건을 지정함(대부분 업무 규칙을 설정)
  설정한 범위 내의 값만 입력받겠다 (gender : 컬럼에 '남' 또는 '여' 라는 데이터만 입력)
  ex) where gender in ('남', '여')
~~~

### 제약을 만드는 시점
1. 테이블 만들면서 바로 생성 (create table ....)
2. 테이블 생성 이후 (alter table ... add constraint....) -> 자동 생성 툴

~~~sql
select * from user_constraints where table_name = 'EMP';
~~~

### 줄임표현

~~~sql
create table temp7(
  --줄임표현
  --id number primary key, 권장하지 않음 -> 제약의 이름이 자동등록 -> SYS_C006997(제약이름)
  id number constraint pk_temp7_id primary key, --관용적인 표혐 : pk_테이블명_컬럼명
  name varchar2(20) not null,
  addr varchar2(50)
);
~~~

---

# 테이블 생성 후에 제약 걸기
~~~sql
create table temp9 (id number);

--기존 테이블에 제약 추가하기
--주의) 입력된 데이터가 있다면 >> 10, 10 두건 >> pk 제약(x) >> 데이터 삭제 >> 제약
alter table temp9
add constraint pk_temp9_id primary key(id);

select * from user_constraints where table_name = 'TEMP9';

select * from  temp9;

alter table temp9
add ename varchar2(20);

--테이블 기본 정보
desc temp9;

--테이블 제약 정보
select * from user_constraints where table_name = 'TEMP9';

--not null 제약 추가
alter table temp9
modify(ename not null);

desc temp9;
~~~

# [check]
- where 조건 과 도일한 형태 >> where gender in ('남', '여');
~~~sql
create table temp10(
  id number constraint pk_temp10_id primary key, --pk는 테이블당 1개(묶어서 1개)
  name varchar2(20) not null, --필수 입력
  jumin char(6) constraint uk_temp10_jumin unique, -- 유니크한 값 (필요하다면 not null 추가 가능)
  addr varchar2(20), --추가입력(보조입력)
  age number constraint ck_temp10_age check(age >= 19) -- where gender in ('남', '여');
);

--테이블 기본 정보
desc temp10;
--테이블 제약 정보
select * from user_constraints where table_name = 'TEMP10';

insert into temp10(id,name,jumin,addr,age)
values(100, '홍길동', '123456', '서울시 강남구', 25);

select * from temp10;

insert into temp10(id,name,jumin,addr,age)
values(200, '홍길동', '123457', '서울시 강남구', 25);

insert into temp10(id,name,jumin,addr,age)
values(300, '홍길동', '123458', '서울시 강남구', 18); --check(x) 19이상 이여야 함

commit;

select * from temp10;

~~~

# [참조 제약] >> 테이블과 테이블 과의 계약 >> 관계 (RDB)

~~~sql
create table c_emp
as
  select empno, ename, deptno from emp where 1=2;
  
create table c_dept
as
  select deptno, dname from dept where 1=2;
  
select * from c_emp;
select * from c_dept;

--참조키(c_emp (deptno) fk -> c_dept(deptno) 컬럼을 참조 하도록...
--참조를 당할려면 (데이터 빌려 줄 려면) >> 신용 >> 최소한 unique 또는 primary key확보
--2번 실행)
alter table c_emp
add constraint fk_emp_deptno foreign key(deptno) references c_dept(deptno);

--순서 c_dept (deptno) pk 걸고
--그리고 c_emp (deptno) 참조
--1번 실행)
alter table c_dept
add constraint pk_dept_deptno primary key(deptno);

insert into c_dept(deptno, dname) values(100, '인사팀');
insert into c_dept(deptno, dname) values(200, '관리팀');
insert into c_dept(deptno, dname) values(300, '회계팀');
commit;

select * from c_dept;
select * from c_emp;

--사원 입사
insert into c_emp(empno, ename)
values(100, '홍길동');--fk (not null 강제 x) null 값 가능

select * from c_emp;

insert into c_emp(empno, ename, deptno)
values(200, '김유신', 500); -- fk제약 위반(부서는 100, 200, 300) 없는데 500 위반

insert into c_emp(empno, ename, deptno)
values(200, '김유신', 300);

select * from c_emp;
select * from c_dept;
-- deptno 라는 관계에서 (c_dept(부모:master) - c_emp(자식:detail))
commit;

delete from c_dept where deptno = 300;--(x) c_emp의 데이터에서 deptno 300번 을 참조 하고 있어서
delete from c_dept where deptno = 200;--(o) deptno 200을 참조하고 있는 것이 없기 때문에 가능
--300번 삭제 >> 참조하고 있는 쪽 (c_emp 부터 삭제하고) >> c_dept >> 300부서 삭제
~~~

~~~
column datatype [CONSTRAINT constraint_name]
REFERENCES table_ name (column1[,column2,..] [ON DELETE CASCADE])
column datatype,
. . . . . . . ,
[CONSTRAINT constraint_name] FOREIGN KEY (column1[,column2,..])
REFERENCES table_name (column1[,column2,..] [ON DELETE CASCADE])
~~~