---
title: DataBase_Oracle(5)
author: JinWon
layout: post
category: DB
---

# 오라클 함수 (오라클.pdf 49 page)

1. 문자형 함수 : 문자를 입력 받고 문자와 숫자 값 모두를 RETURN 할 수 있다.
2. 숫자형 함수 : 숫자를 입력 받고 숫자를 RETURN 한다.
3. 날짜형 함수 : 날짜형에 대해 수행하고 숫자를 RETURN 하는 MONTHS_BETWEEN 함수를 제외하고 모두 날짜 데이터형의 값을 RETURN 한다.
4. 변환형 함수 : 어떤 데이터형의 값을 다른 데이터형으로 변환한다.(to_char(), to_number(), to_date())
5. 일반적인 함수 : NVL(), DECODE()


~~~sql
--문자열 함수
select initcap('the super man') from dual; --단어의 첫글자 대문자

select lower('AAA'), upper('aaaaa') from dual; --aaa, AAAAA
select ename, lower(ename) as " ename" from emp; 

select * from emp where lower(ename) = 'king';

--문자의 개수 (length)
select length('abcd') from dual; --4
select length('홍길동') from dual; --3

select length('홍a길 동') from dual; --5

--결합 연산자 ||
select 'a' || 'b' from dual;
--함수 (함수는 파라미터의 개수가 제한적이라 확장성에는 ||이 권장 사항이다)
select concat('a','b') from dual;

select concat(ename, job) from emp; 
select ename||' '||job from emp;

--부분 문자열 추출
--java(substring)
--oracle(substr)
select substr('ABCDE',2,3) from dual; --BCD
select substr('ABCDE',1,1) from dual; --자기자신 : A
select substr('ABCDE',3,1) from dual; --자기자신 : C
select substr('ABCDE',3) from dual;
select substr('ABCDE',-2,1) from dual; --뒤에서 -1 -2
~~~

# 가명칭 "jumin"
~~~sql
select id || ':' || rpad(substr(jumin,1,7),length(jumin),'*')as"jumin" from member2;
~~~

# rtrim 함수
- [오른쪽 문자] 지워라
- 오른쪽에서 자신이 지정한 문자가 있다면 지워라
    - select rtrim('MILLER', 'ER') from dual;
- ltrim 함수 [왼쪽]
    - select ltrim('MILLLLLLLLLLLLLLLLLER', 'MIL') from dual;
- 주의) 같은 글자가 연속적으로 나오면 같이 지운다.


# 치환함수
~~~sql
select ename, replace(ename, 'A','와우') from emp;
~~~

-------------------------[문자열 함수 END]----------------------------

# [숫자함수]

- round (반올림)
- trunc (절삭함수)
- 나머지 구하는 함수 mod()

~~~sql
-- -3 -2 -1 0(정수) 1 2 3
--    .(정수)을 기준으로 
select round(12.345,0) as "r" from dual; -- 정수부만 남겨라 12
select round(12.567,0) as "r" from dual; -- 13

select round(12.345,1) as "r" from dual; -- 12.3
select round(12.567,1) as "r" from dual; -- 12.6

select round(12.345,-1) as "r" from dual; -- 10
select round(15.345,-1) as "r" from dual; -- 20
select round(15.345,-2) as "r" from dual; -- 0

--trunc (반올림 하지 않고 버려요)
select trunc(12.345,0) as "r" from dual; -- 12
select trunc(12.567,0) as "r" from dual; -- 12

select trunc(12.345,1) as "r" from dual; -- 12.3
select trunc(12.567,0) as "r" from dual; -- 12.5

select trunc(12.567,-1) as "r" from dual; -- 10
select trunc(15.567,-1) as "r" from dual; -- 10

select trunc(15.567,-2) as "r" from dual; -- 0

--나머지
select 12/10 from dual;
select mod(12,10) from dual; --나머지 2
select mod(0,0) from dual;
~~~

----------------------[숫자 함수 END]-------------------------

# 날짜 함수

- 날짜 함수 : sysdate
- 날짜 연산 (POINT)
- Date + Number >> Date
- Date - Number >> Date
- Date - Date >> Number(일수)

~~~sql
Select * from SYS.NLS_SESSION_PARAMETERS where parameter = 'NLS_SESSION_PARAMETERS';

select sysdate from dual;
select hiredate from emp;

select MONTHS_BETWEEN('2018-02-27', '2010-02-27') from dual;

select MONTHS_BETWEEN(sysdate, '2010-01-01') from dual;

select round(MONTHS_BETWEEN(sysdate,'2010-01-01'),1) from dual;
select trunc(MONTHS_BETWEEN(sysdate,'2010-01-01'),1) from dual;

select to_date('2015-04-25') + 1000 from dual;

select sysdate + 100 from dual;
~~~

-----------------------[날짜 함수 END]--------------------------

# [변환함수]

- Oracle : 문자, 숫자, 날짜
- to_char()
    - 숫자 -> 문자 (숫자를 문자로)
    - 날짜 -> 문자 (날짜를 문자로)

- 형식정의 때문에 60%
- to_date() : 문자 -> 날짜 >> select '2018-12-12' + 1000 >> select to_date('2018-12-12')...
- 자동형변환 된다.
~~~sql
select '100' + 100 from dual;
select to_number('100') + 100 from dual;
~~~

---

### 오라클 기본 타입(데이터 타입)
~~~
create table 테이블명 (컬럼명 타입);
create table member (age number); n건의 데이터 insert
~~~
- java -> class memeber{ int age; } >> member m = new memeber(); 1rjs
- java -> List<member> list = new ArrayList<>(); list.add(new member()) n건

### 문자 타입

- char(20) -> 20byte -> 한글10자/ 영문자,특수문자,공백 20자 -> 고정길이 문자열
- varchar2(20) -> 20byte -> 한글10자/ 영문자,특수문자,공백 20자 -> 가변길이 문자열
~~~
char(20) -> '홍길동' -> 20byte 모두 사용
varchar2(20) -> '홍길동' -> 20byte 중 6byte만 사용

고정된데이터 : 남, 여 >> 처리 >> char(2)로 사용해라 (권장)
결국 >> varchar2(2)

성능 상의 문제)
char() .... varchar2() 보다 검색상 우선....

결국 문제는 한글
unicode(2byte) :  한글, 영문자, 특수문자, 공백 >> 2byte
nchar(20) >> 20은 문자의 개수 >> 실제 byte*2 >> 40byte
nvarchar2(20) >> 20개의 문자

한글, 영문자, 특문, 공백 20자
~~~

---

1. to_number() : 문자를 숫자로 
~~~sql
select 1 + 1 from dual; --2

select '1' + 1 from dual; --자동형변환

select to_number('1') + 1 from dual;

select '1' + '1' from dual; -- + 는 무조건 연산 (||이 문자열 결합)
~~~
---------------------------------------------------------
2. to_char()
    - 숫자를 문자로 (숫자 -> 문자(형식문자))
    - 날짜를 문자로 (날짜 -> 문자(형식문자))

~~~sql
select sysdate from dual;
--YYYY YY MM DD ... 정의되어 있어요

select sysdate || '일' from dual;
--원칙
select to_char(sysdate) || '일' from dual;

select sysdate,
to_char(sysdate, 'YYYY')||'년' as "YYYY",
to_char(sysdate, 'YEAR') as "YEAR",
to_char(sysdate, 'MM')||'월' as "MM",
to_char(sysdate, 'DD')||'일' as "DD",
to_char(sysdate, 'DAY') as "DAY",
to_char(sysdate, 'DY')||'요일' as "DY"
from dual;
~~~

---
# [일반함수]
- 프로그램적인 성향이 강하다..
- nvl(), nvl2() --> null처리 함수
- decode() 함수 --> java if..
- case() 함수 --> java switch..
~~~sql
select comm, nvl(comm, 0) from emp;

create table t_emp(
 id number(6),
 job varchar2(20)
);

insert into t_emp(id, job) values(100, 'IT');
insert into t_emp(id, job) values(200, 'SALES');
insert into t_emp(id, job) values(300, 'MGR');
insert into t_emp(id, job) values(400, NULL);
insert into t_emp(id, job) values(500, 'MGR');
commit;

select * from t_emp;
~~~

1. nvl()
~~~sql
select id, job, nvl(job, 'Empty...') from t_emp;
~~~
2. nvl2() , null인 경우 처리, null이 아닌 경우 처리
    - job 이 null 아니면 앞 / null이면 뒤
~~~sql
select id, job, nvl2(job,'AAA', 'BBB') from t_emp;

select id, job, nvl2(job,job || '입니다', 'empty') from t_emp;
~~~
3. decode POINT (일반 SQL 제어문이 없다 if, switch)
    - decode(표현식, 조건1, 결과1, 조건2, 결과2, 조건3, 결과3 ...... , 조건에 해당x결과)
    - 통계적 데이터 표현에 주로 사용..
~~~sql
select id, job, decode(id, 100, 'IT...',
                           200, 'SALES...',
                           300, 'MGR...',
                           'ETC...') as "dexodejob"
from t_emp;

select job, decode(job, 'IT', job, 'SALES', job, 'MGR', job, 'empty') from t_emp;
--활용
select count(decode(job, 'IT', 1)) as "IT", count(decode(job, 'SALES', 1)) as "SALES", 
count(decode(job, 'MGR', 1)) as "MGR" from t_emp;
~~~