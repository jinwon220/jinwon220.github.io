---
title: Ex01_Basic Operation(연산자)
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


# 연산자, 제어문 

~~~java
public class Ex06_Operation {
	public static void main(String[] args) {
		int sum = 0;

		// 대입 연산자 (+=, -=, *= ...)
		sum += 1; // sum = sum + 1;
		sum -= 1; // sum = sum - 1;
		System.out.println("sum :" + sum);

~~~

> 시나리오 : if문과 대입 연산자 <br>
간단한 학점 계산기 <br>
학점에 대해서 A+ , B- <br>
94점 <br>
95점 보다 크면 A+ <br>
그외는 A- <br>

~~~java
		int score = 97;
		String grade = ""; // 문자열 초기화 ""
		System.out.println("당신의 점수는 : " + score);

		if (score >= 90) {
			grade = "A";
			if (score >= 95)
				grade += "+";
			else
				grade += "-";
		} else if (score >= 80) {
			grade = "B";
			if (score >= 85)
				grade += "+";
			else
				grade += "-";
		} else if (score >= 70) {
			grade = "C";
			if (score >= 75)
				grade += "+";
			else
				grade += "-";
		} else
			grade = "F";
		
		System.out.println("당신의 학점은 : " + grade + "입니다");

~~~

#### TIP) 삼항연산자로 만드는 방법
~~~java
if (score >= 90)
	grade = (score >= 95) ? "A+" : "A-";
else if (score >= 80)
	grade = (score >= 95) ? "B+" : "B-";
else if (score >= 70)
	grade = (score >= 95) ? "C+" : "C-";
else
	grade = "F";
~~~

~~~java
		// switch문
		int data = 100;
		switch (data) {
		case 100:
			System.out.println("100입니다.");
			break;
		case 90:
			System.out.println("90입니다.");
			break;
		case 80:
			System.out.println("80입니다.");
			break;
		default:
			System.out.println("default");
		}

		// break 구문은 없어도 된다.
		// 다만, 빠져나가지 않고 전부 실행한다.
		switch (data) {
		case 100:
			System.out.println("100입니다^^");
		case 90:
			System.out.println("90입니다^^");
		case 80:
			System.out.println("80입니다^^");
		default:
			System.out.println("default^^");
		}

		//////////////////////////////////////
		
		// break 구문을 사용하지 않고
		// 전부 실행했을 때, 사용 활용 방법
		int month = 3;
		String res;

		switch (month) {
		case 1:
		case 3:
		case 5:
		case 7:
		case 8:
		case 10:
		case 12:
			res = "31";
			break;
		case 4:
		case 6:
		case 9:
		case 11:
			res = "30";
			break;
		case 2:
			res = "29";
			break;
		default:
			res = "월이 아닙니다.";
		}
		System.out.println(month + "월은 " + res + "일까지 입니다.");
~~~

난수 (랜덤값 : 임의의 추측 값) <br>
import java.lang.Math (Math 클래스) <br>
default > java.lang > import 내부적으로... <br>
java.lang 안에 있는 자원은 java에서 기본적으로 열어둠 (import 없이 사용가능하다.) <br>
Returns a double value with a positive sign, greater than or equal to 0.0 and less than 1.0
Math.Random() 자원은 Random() 함수가 static 이기 때문에 객체 생성 없이도 사용 가능하다 <br>
결과 : 0.0 <= random < 1.0 의 double 타입의 값 올 추출 

~~~java	
		System.out.println("Math.random() : " + Math.random());
		System.out.println("Math.random() *10 : " + Math.random()*10);
		//0~9까지의 정수
		System.out.println("(int)(Math.random()*10) :" + (int)(Math.random()*10));
		//1~10까지의 정수
		System.out.println("1 ~ 10 랜덤 정수 :" + ((int)(Math.random()*10)+1));
~~~

> 시나리오 : <br>
만들려고 하는 시스템은 백화점 경품 추첨 시스템입니다. <br>
경품 추첨시 1000점이 나오면 <br>
겸품으로 TV, NoteBook, 냉장고, 한우세트, 휴지
경품 추첨시 900점이 나오면 <br>
겸품으로 NoteBook, 냉장고, 한우세트, 휴지
경품 추첨시 800점이 나오면 <br>
겸품으로 냉장고, 한우세트, 휴지
경품 추첨시 700점이 나오면 <br>
겸품으로 한우세트, 휴지
경품 추첨시 600점이 나오면 <br>
겸품으로 휴지
그외 100 ~ 500 까지는 칫솔

사용자가 와서 경품시스템을 사용하게 되면 <br> 랜덤하게 100 ~ 1000 까지의 점수가 나온다.


> TIP) 100~1000 의 100단위 정수 구하는 방법
~~~java
jumsu = (int)(Math.random()*1000);

if(jumsu > 900) 	
	jumsu = 1000;
else if(jumsu > 800)
	jumsu = 900;
else if(jumsu > 700)
	jumsu = 800;
else if(jumsu > 600)
	jumsu = 700;
else if(jumsu > 500)
	jumsu = 600;
else if(jumsu > 400)
	jumsu = 500;
else if(jumsu > 300)
	jumsu = 400;
else if(jumsu > 200)
	jumsu = 300;
else if(jumsu > 100)
	jumsu = 200;
else
	jumsu = 100;
~~~

~~~java
		int jumsu;
		String giveaway = "";
		
		
		jumsu = (int)((Math.random()*10) + 1)*100;

		System.out.println(jumsu);
		switch(jumsu) {
			case 1000:
				giveaway += "TV";
			case 900:
				giveaway += " NoteBook";
			case 800:
				giveaway += " 냉장고";
			case 700:
				giveaway += " 한우세트";
			case 600:
				giveaway += " 휴지";
				break;
			default :
				giveaway = "칫솔";
		}
		System.out.println("점수는 " + jumsu +"이고, 겸품은 " + giveaway + " 입니다");
		
		
	}
}
~~~