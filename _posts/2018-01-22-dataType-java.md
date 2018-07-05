---
title: DataType
author: JinWon
layout: post
category: Java
---

자바는 시스템에서 8가지 기본타입 제공(자료형) + (참조타입 String) <br>
8가지 기본 타입 (값을 저장하는 타입) <br>
숫자->정수 (음의정수, 0, 양의정수) -> byte(-128~127) <br>
-> char(한 문자를 표현하는 자료형 : 한 문자(2byte), unicode 형식)
-> short <br>
-> int(-21억 ~ 21억) : **Java 정수의 연산 기본 타입 (4byte)** <br>
-> long(8byte) : int보다 큰 값은 long 사용 <br>
실수 (부동소수점) -> float(4byte), double(8byte) : **정밀도가 높다 (double) : java 실수 연산의 기본**
           
논리->boolean (true, false) : 프로그램의 논리제어

<br>

> 궁금한점 : 문자열 표현 어디갔지 ???? : 홍길동, 안녕하세요 담을 타입... <br>

string 는 클래스 (설계도) > String s = new String("안녕하세요"); -> 설명대로라면 이렇게 써야하는게 정석 <br>
하지만 앞 예제를 살펴보면 : String s = "안녕하세요";

**String은 자료형처럼 사용한다.(아스키코드 이용)**

값타입 -> 숫자, 논리 <br>
참조타입 -> 클래스, 배열

class == 설계도이다 == Type <br>
class 작은 타입을 모아 놓은 큰 타입이다. <br>
#### class 반드시 메모리에 적재하고 (올려놓고) 사용

new 연산자를 사용하면...
~~~java
class Car { // Car라는 Type 생성 : Type 변수명 : Car c;
	String color;
	int data;
}

public class Ex03_DataType {
	public static void main(String[] args) {
		Car c = new Car();
		System.out.println("c라는 변수의 값 : " + c); //참조타입

		Car c2;
		c2 = new Car();
		
		System.out.println("c2라는 변수의 값 : "+ c2);
		
		Car c3 = c; // 참조타입의 할당은 : 주소값 할당
		System.out.println(c + " : " + c3);
		System.out.println(c3 == c);
		c3.data = 3333;
		System.out.println(c.data);
		c.data = 4444;
		System.out.println(c3.data);
		// c3.data , c.data (바라보는 주소가 같다)
		
		int i = 200; // 선언과 할당
		System.out.println("i라는 변수의 값 : " + i); //값타입
		
		int j; // 선언
		j = 1000; // 할당(초기화)
		
		int a, b; // 단점 : 각각 초기화
		a = 10;
		b = 11;
		
		System.out.println(a + " : " + b);
		
		int k = 1111;
		int k2  = k; // k2 초기화
		k2 = 2222; // k2 초기화
		//이때 k값을 출력하면?? 2222
		
		//int (-21 ~ 21억)
		long number = 1000000000; // 문제 없음
		//long number2 = 10000000000;  error
		long number2 = 10000000000L; // error
		//10000000000 정수값 100억 (int 값의 범위를 벗어남)
		//**정수의 리터럴 값 (기본 타입) : int**
		//int 범위를 벗어나는 정수리터럴 값은 별도의 접미사를 통해서 값의 범위를 명시해야 함. 
		System.out.println(number + "/" + number2);
		
		//char : 정수값을 가지고 있다(2byte)
		//문자를 표현하는 자료형
		//1. [한문자] > 2byte
		//2. 한글 한 자: 2byte
		//3. 영문자, 특수문자, 공백 : 1byte
		//한글 한 자, 영문 한 자 표현 2byte로 표현 (unicode)
		
		//java 문자열 : "가" : String s = "가";
		//java 문자 : '가' : Char c = '가'; 아스키 코드와 연결 가능
		
	    char single = '가';
	    System.out.println(single);
	    char ch = 'A';
	    System.out.println(ch);
	    System.out.println((int)ch); //타입변환(casting) 아스키코드표 10진수 값 : 65
	                    //명시적 형변환
	    char ch2 = 'a';
	    System.out.println(ch2);
	    System.out.println((int)ch2); // 아스키코드표 10진수 값 : 97
	    
	    int ch3 = 65;
	    char ch4 = (char)ch3; //문자 : 'A' 명시적 형변환
	    System.out.println(ch4);
	    
	    //ch4 : char 타입
	    //cint2 : int 타입
	    int cint2 = ch4; // 암시적(묵시적) 형변환
	    System.out.println(cint2);
	    //Today Point
	    //변수가 가지고 있는 [값을 보지말고] [변수타입]을 보세요
	    //타입의 크기를 보자
	    //[큰 타입]에는 [작은 타입] 값을 넣을 수 있다.
	    //[작은 타입]에는 [큰 타입] 값을 넣을 수 없다.
	    //char <- int > char <- (char)int
	    //int  <- char 가능  | 내부적으로  int <- (int)char
	    
	    
	    // 손실 되는 형변환
	    int intValue = 103029770;
		byte byteValue = (byte)intValue;
		
		System.out.println(byteValue);// 형변환을 했을 때
		// 큰 타입의 값이 작은 타입의 값의범위를 초과 하였기 때문에
		// 전혀 다른 값이 나옴 ( 쓰래기 값 )
	    
	    
		//String (문자열) 타입
		//String 값 타입이 아니고 클래스 타입(new 통해서 메모리 올리고 사용)
		//원칙 : String str = new String("홍길동");
		//POINT : String 일반 값타입 사용해도 문제 X (int, double 타입 처럼 사용)
		
		String str = "hellow world";
		System.out.println(str);
		
		String str2 = str + "졸려요"; //DB(오라클 : 산술 + , 결합 ||)
		System.out.println(str2);
		
		//Tip) java 특수문자 사용하기
		//문자를 인지하기 위해서 : \(역슬래시) 를 사용
		char sing ='\'';
		System.out.println(sing);
		
		//홍"길"동 이라는 문자열을 출력
		String name = "홍\"길\"동";
		System.out.println(name);
		
		String str3 = "1000";
		String str4 = "10";
		String result = str3 + str4;
		System.out.println(result);
		
		//c:\Temp 라는 문자열 출력(특수문자의 활용)
		String path = "c:\\Temp";
		System.out.println(path);
		
		//실수(부동소수점)
		//float(4byte)
		//double(8byte) : **실수의 기본 리터널은 double (정밀도가 높다(소수이하 자리를 더 많이 표현))
		
		//float f = 3.14; 에러
		float f = 3.14F; //접미사 (F, f) 리터널 값의 정의
		float f2 = 1.123456789f; //**반올림 처리
		System.out.println("f2 : " + f2); // 1.1234568
		// 대략 7번째 자리까지 표시 8번째 자리에서 반올림
		
		double d = 1.123456789123456789;
		System.out.println("d :" + d);
		// 대략 16번째 자리까지 표시 17번째 자리에서 반올림
		
		//Test
		//double d2 = 100; //암시적 형변환 (int -> double)
		// == double d2 = 100.0;
		double d2 = (double)100;
		
		
		//Quiz
		double d3 = 100;
		int i5 = 100;
		
		//int result2 = d3 + i5; Error 고쳐보자
		
		//1. int result2 = (int)d3 + i5; // (값이 커지면) 데이터 손실이 생길 수 있다.
		//2. double result2 = d3 + i5;
		//3. int result2 = (int)(d3 + i5); // (값이 커지면) 데이터 손실이 생길 수 있다.
		
		//Today Point
		//작은 타입 + 큰 타입 => 큰 타입
		//타입 간 형변환 > 변수가 가지는 값을 보지 말고 변수의 타입으로 판단하자
		//명시적 형변환(casting)
		
		int i6 = 100;
		byte b2 = (byte)i6;
		System.out.println(i6);
		
		byte b3 = 20;
		int i7 = b3; // 암시적 형변환 (내부적으로 (int)b3)
		
		
		
	}
}
~~~