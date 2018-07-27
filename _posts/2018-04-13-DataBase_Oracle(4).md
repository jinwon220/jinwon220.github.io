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