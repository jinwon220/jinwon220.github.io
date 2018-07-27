---
title: DataBase_Oracle(6)
author: JinWon
layout: post
category: DB
---

# CASE문

~~~
CASE 조건 WHEN 결과1 THEN 출력1
                 WHEN 결과2 THEN 출력2
                 WHEN 결과3 THEN 출력3
                 WHEN 결과4 THEN 출력4
                 ELSE 출력5
END "컬럼명"                 
~~~

~~~sql
create table t_zip(
  zipcode number(10)
);

insert into t_zip(zipcode) values(2);
insert into t_zip(zipcode) values(31);
insert into t_zip(zipcode) values(32);
insert into t_zip(zipcode) values(33);
insert into t_zip(zipcode) values(41);
commit;

select * from t_zip;


select '0' || to_char(zipcode),
      case zipcode when 2 then '서울'
                   when 31 then '경기'
                   when 32 then '강원'
                   when 41 then '충청도'
                   else '기타지역'
      end as "regionname"
from t_zip;
~~~

---

1. 
~~~sql
case 컬럼명 when 결과 then 출력     (= 에 대한 비교)
            when 결과 then 출력
            else 출력
end
~~~
2. 
~~~sql
case when 컬럼명 조건비교구문 then 출력   (ex: sal < 2000 )
    when 컬럼명 조건비교구문 then 출력
    else 출력
end
~~~

~~~
사원테이블에서 사원급여가 1000달러 이하면 4급
1001 달러 2000달러 이하면 3급
2001 달러 3000달러 이하면 2급
3001 달러 4000달러 이하면 1급
4001 달러 이상이면 특급을 부여하는 데이터를 출력하세요
~~~
~~~sql
select case when sal <= 1000 then '4급'
            when sal between 1001 and 2000 then '3급'
            when sal between 2001 and 3000 then '2급'
            when sal between 3001 and 4000 then '1급'
            else '특급'
       end as "급여등급" , empno, ename, sal
from emp;
~~~

- 문자 >> 숫자 >> 날짜 >> 변환(to_char(), to_number(), to_date() >> 일반함수 (nvl(), ~case) >>

# [집계함수]
1. count(*)
    - 라인단위로 읽어서 몇건이 있는가 확인 
    - count(컬럼명) >> 데이터 건수
2. sum()
3. avg()
4. max()
5. min() ...  등

- 집계함수는 반드시 (group by 결과) 같이 사용
- POINT : 모든 집계함수(그룹함수)는 null 값을 무시
- POINT : select 절에 집계함수 외에 다른 컬럼이 오면 반드시 group by 절에 명시되어야 한다.

~~~sql
select count(*) from emp; -- 14건

select count(empno) from emp; --empno 데이터 몇건 있지 (단 null 값 무시) => 14건
select count(comm) from emp; --comm 데이터 몇건 있지 (단 null 값 무시) => 6건

select count(nvl(comm, 0)) from emp; --comm 데이터 null을 0으로 => 14건

--급여의 합
select sum(sal) from emp;

--평균 급여 (급여의 합 / 건수)
select round(avg(sal), 0) from emp; --반올림한 평균 급여

select round(sum(sal)/count(sal), 0) from emp;

--사장님이 회사 수당이 총 얼마 지급되지?(평균)
select sum(comm) as "sum" from emp; -- 4330
--(수당 받는 사람중에 나눈다...721)
select trunc(avg(comm),0) as "gvg" from emp; -- 721
--(사원수에 나눈다...309)
select trunc(avg(nvl(comm, 0)),0) as "gvg" from emp; -- null 값을 무시했기에 null값도 0으로 지정

--검증
--회사의 규정따라 다르다(사원수에 나눈다...309)(수당 받는 사람중에 나눈다...721)
select comm from emp;
select (300+200+30+300+3500+0) / 14 from dual; -- 309
~~~

> POINT : 집계함수의 결과는[컬럼 1개]
~~~sql
select sum(sal), avg(sal), max(sal), count(sal), count(*) from emp;
~~~

### 순서
~~~
select       4번
from         1번
where        2번
group by     3번
order by     5번
~~~

---

~~~sql
--직종별 평균급여가 3000달러 이상인 사원의 직종과 평균급여를 출력하세요
--조건 : 직종별 평균급여 >= 3000
--평균급여 >= 3000 구해질려면 >> 평균급여 >> 시점 >> group by...
--답 > group by 조건
--group by 조건 : having 절

select job, avg(sal)
from emp
group by job
having avg(sal) >= 3000;
~~~

---

# [조인(JOIN)] : 다중테이블에서 데이터를 검색하는 방법

### 종류
1. 등가조인(equi join) => 70%
    - 원테이블과 대응대는 테이블에 있는 컬럼의 데이터를 1:1 매핑
~~~
[ANSI  문법]
문법 : join  on ~조건절
문법 : [inner] join on ~조인의 조건절
~~~

2. 비등가조인(non-equi join) => 의미만 존재 => 문법 등가조인
    - 원테이블과 대응대는 테이블에 있는 컬럼이 1:1 매핑되지 않는 경우
    - ex) emp , salgrade : 사원의 등급 검색 컬럼 2개 사용 (losal , hisal)  

3. outer join(equi join + null) => equi join null 처리 안되요
    - outer join (두개의 테이블에서 주 , 종 관계 파악)
~~~
문법 : left outer join  (왼쪽 주 , 오른쪽 종)
       right outer join  (오른쪽 주 , 왼쪽 종)
       full outer join   (left , right join > union 하면)
~~~

4. self join (자기참조) => 의미만 존재 => 문법 등가조인
    - ex) emp 테이블에서 smith 관리자 이름은 무었입니까
    - 하나의 테이블안에서 컬럼이 다른 컬럼을 참조하는 경우

### ANSI 문법 권장
~~~sql
--어떤게 join 조건절이고 어떤것이 from 조건절인지 모호하다....
select m.m1, m.m2, s.s2
from m, s
where m.m1 = s.s1 and m.m1 ='A';

--ANSI 문법 권장
select m.m1, m.m2, s.s2
from m join s
on m.m1 = s.s1;
~~~

### 조인요령
~~~sql
--조인 요령(select *....) >> 필요 컬럼만 추출
--select empno, emp.deptno 가능 하지만 emp. 붙여주자
select emp.empno,
       emp.ename,
       emp.deptno,
       dept.dname       
from emp join dept
on emp.deptno = dept.deptno;
~~~

### 가명칭
~~~sql
--테이블에 가명칭 부여(실제 현업에서는 테이블이름이 길다 ... 별칭 부여)
select e.empno,
       e.ename,
       e.deptno,
       d.dname       
from emp e join dept d
on e.deptno = d.deptno;


select s.s1, x.x2
from s join x
on s.s1 = x.x1;
~~~

### 한개 이상의 테이블
~~~sql
--join 한개 이상의 테이블
--oracle sql join 문법
select m.m1, m.m2, s.s2, x.x2
from m, s, x
where m.m1 = s.s1 and s.s1 = x.x1;

--ANSI문법(권장)
select m.m1, m.m2, s.s2, x.x2
from m join s on m.m1 = s.s1 
       join x on s.s1 = x.x1;
~~~

### 등가조인의 문제점
~~~sql
select * from employees;
select * from departments;
select * from locations;

--employees, departments
--1.사번, 이름(last_name), 부서번호, 부서이름을 출력하세요
select e.employee_id, e.last_name, e.department_id, d.department_name
from employees e join departments d
on e.DEPARTMENT_ID = d.DEPARTMENT_ID
order by e.employee_id;

--문제점 : 사원수 107명 >> 출력 106명(누군가 누락)
select * from employees where department_id is null;

--부서번호가 null인 사람도 같이 출력 하는 방법
--등가조인으로는 해결 할 수 없음......outer join으로 해결
~~~

# 비등가조인

~~~sql
select * from emp;
select * from salgrade;

--사번, 이름, 부서번호, 급여, 급여등급
select e.empno, e.ename, e.deptno, e.sal, s.grade
from emp e join salgrade s
on e.SAL between s.LOSAL and s.HISAL;

--사번, 이름, 부서번호, 부서명, 급여, 급여등급
select e.empno, e.ename, e.deptno, d.dname, e.sal, s.grade
from emp e join salgrade s on e.SAL between s.LOSAL and s.HISAL
           join dept d on e.DEPTNO = d.DEPTNO;

--2.사번, 이름, 부서번호, [부서명], 지역코드, [도서명] 을 출력하세요
select * from employees;
select * from departments;
select * from locations;

select e.EMPLOYEE_ID,
       e.LAST_NAME,
       e.DEPARTMENT_ID,
       d.DEPARTMENT_NAME,
       d.LOCATION_ID,
       l.CITY
from employees e join departments d on e.DEPARTMENT_ID = d.DEPARTMENT_ID
                 join locations l on d.LOCATION_ID = l.LOCATION_ID;
~~~

- outer join (equi join + null(남는 데이터) -> equi join null 처리 안되요
- outer join (두 개의 테이블에서 주, 종 관계 파악)

### 문법
~~~
left outer join (왼쪽 주, 오른쪽 종)
right outer join (오른쪽 주, 왼쪽 주)
full outer join (left, right join > union 하면)
~~~

> 내부적으로 등가조인을 실행하고 (주, 종) 관게를 파악해서 <br>
주인 되는 테이블에있는 남는 데이터를 가지고 오는 것

~~~sql
select *
from m join s
on m.m1 = s.s1;

select *
from m left outer join s
on m.m1 = s.s1;

select *
from m right outer join s
on m.m1 = s.s1;

select *
from m full outer join s
on m.m1 = s.s1;
~~~

### self join(자기참조) -> 문법 >> 등가조인
~~~sql
--하나의 테이블에서 컬럼이 다른 컬럼을 참조하는 경우
select e.empno, e.ename, e2.empno, e2.ename
from emp e join emp e2
on e.mgr = e2.empno;
--사원이 14명인 13명만 출력됨
--해결(null값)
select e.empno, e.ename, e2.empno, e2.ename
from emp e left join emp e2 -- salf join 하나의 테이블을 2개 처리(테이블에 가명칭 부여해서)
on e.mgr = e2.empno;

select *
from m, s; --실행은 되지만 나올 수 있는 모든 경우의 수가 나옴
--where m.m1 = s.s1 
~~~