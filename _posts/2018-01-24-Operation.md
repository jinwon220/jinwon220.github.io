---
title: Operation
author: JinWon
layout: post
category: Java
---

~~~java
public class Ex05_Operation {

	public static void main(String[] args) {
		int result = 100/100;
		System.out.println(result);
		
		result = 13/2;
		System.out.println(result);//정수만 나옴
		
		result = 13%2; // 나머지 구하는 연산자
		System.out.println(result);
		
		//증가감 연산자 (++, --) 1씩 증가, 감소
		int i = 10;
		++i; //전치 증가
		System.out.println(i);
		i++; //후치 증가
		//변수  한개 전치, 후치 동일
		System.out.println(i);
		
		//Ponit 전치 후치 연산자는 다른 식과 결합 (성질이 나타남)
		int i2 = 5;
		int j2 = 4;
		
		int result2 = i2 + ++j2;
		System.out.println(result2);
		System.out.println("sesult2: " + result2 + " j2 : " + j2);
		// result2 : 10 = 5 + (++4) = 5 + 5 = 10
		result2 = i2 + j2++;
		System.out.println("sesult2: " + result2 + " j2 : " + j2);
		// result2 : 10 = 5 + 5++ = 10 / j2 = 6
~~~

> POINT <br>
자바의 연산 규칙 <br>
정수의 모든 [연산]은 int로 변환 후 처리 <br>
byte + byte => 컴파일러 int + int

~~~java
		byte b = 100;
		byte c = 1;
		// byte d = b + c;
		/*1.*/ byte d = (byte)(b + c);
		//2. int d = b + c;
		
		System.out.println("d : " + d);
~~~

> byte + byte => 컴파일러 int + int <br>
char + char => 컴파일러 int + int <br>
연산처리시!! <br>
POINT : 연산에서 int 보다 작은 타입은 int 변환 (long 제외) <br>
연산시 (byte, char, short -> int)바꾸어서 연산 처리

> 피연산자 중 표현 범위가 큰 타입으로 형변환 <br>
byte + short -> int + int -> int <br>
char + int -> int + int -> int <br>
int + long -> long + long -> long 

> 정수와 실수의 연산 = 실수 <br>
float + int -> float + float -> float <br>
long + float -> float + float -> float <br>
float + double -> double + double -> double

~~~java		
		float num2 = 10.45f;
		int num3 = 20;
		//num2 + num3 연산
		//int result5 = num2 + num3; //error
		//1. int result5 = (int)num2 + num3; //데이터 손실
		//2. int result5 = (int)(num2 + num3); //데이터 손실
		//System.out.println("result5 : " + result5); //데이터 손실 
		float result5 = num2 + num3;
		System.out.println(result5);
		
		char c2 = '!';
		char c3 = '!';
		//c2 + c3 결과는?
		//char result6 = c2 + c3; //error 
		int result6 = c2 + c3; // 아스키 코드표 기준 '!'에 대한 10진수 값 -> 33 + 33
		System.out.println("result6 : " + result6);
		// result6이 가지는 정수값의 문자를 표현하면
		System.out.println((char)result6);
		
		//128부터 char형의 아스키코드는 없기 때문에 ?로 error가 난다.
		
		//제어문
		//중소기업 시험문제 (구구단 출력)
		//별찍기 
		int sum = 0;
		for(int j=1; j<=100; j++) {
			//System.out.println("j : " + j);
			if(j%2 == 0) {
				sum += j; //sum = sum + j;
			}
		}
		System.out.println("sum : " + sum);
		
		//== 연산자 ([값]을 비교하는 연산자)
		if(10 == 10.0f) {//10과 10.0은 타입이 다르지만 값은 같다
			System.out.println("True");
		} else {
			System.out.println("False");
		}
		
		//! 부정연산자
		if('A' != 65) { // char값은 정수와 호완이 되므로 같은 값이다
			System.out.println("같은 값이 아니다");
		} else {
			System.out.println("같은 값이다");
		}
		
		//암기 (Point)
		//삼항 연산자  (조건식)?ture:false;
		int p = 10;
		int k = -10;
		int result8 = (p == k)? p : k;
		System.out.println(result8);
		
		//if문으로 변경 해보기
		if(p==k) result8 = p;
		else result8 = k;
		System.out.println(result8);
~~~

진리표

0 : false <br>
1 : true <br>

|<center>값</center> |<center>값</center>|<center>OR</center>|<center>AND</center>|
|:--------|:--------|:--------|:--------|
|0|0|0|0|
|0|1|1|0|
|1|0|1|0|
|1|1|1|1|

sql 문 (oracle) <br>
select *
from emp
where empno = 1000 and sal > 2000

select *
from emp
where empno = 1000 or sal > 2000

연산자 <br>
| or 연산자 <br>
& and 연산자 <br>
0 과 1 변환해서 bit 연산


~~~java		
		//bit연산 방법 (잘 사용하지 않음)
		int x = 3;
		int y = 5;
		System.out.println("x|y : " + (x|y));
		
		//십진수 -> 2진수 (0과 1로만 이루어진 값으로)
		// 128 64 32 16 8 4 2 1
		//              0 0 1 1 > *3 이진수
		//              0 1 0 1 > *5 이진수
		//OR            0 1 1 1 => 3+2+1 > 7
		//AND           0 0 0 1 => 1
		System.out.println("x&y : " + (x&y));
	}
}
~~~

POINT 논리 연산(&&(and), ||(or)) > 연산자 return boolean <br>

>중요하게 생각하는 이유

&& 논리 조건 <br>
if(10 > 0 && -1 > 1 && 100 > 2 && 1 > -1 && .....) <br>
&&조건에서 한개라도 false면 if를 태우지 않는다. <br>
좋은점 : 빠르게 실행할 확률이 높다.

|| 논리 조건 <br>
if(10 > 0 || -1 > 1 || 100 > 2 || 1 > -1 || .....) <br>
||조건에서 한개라도 true면 if를 태운다.