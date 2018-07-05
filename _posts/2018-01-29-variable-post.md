---
title: VARIABLE
author: Jin Won
layout: post
---
<!-- Text stuff -->
# VARIABLE
>## class = 설계도 <br> 객체 = 설계도를 기반으로 메모리에 오른 것
>	1. 설계도 용도 만드는 클래스  kr.or.test.Emp
>	2. 실행을 위해서 만드는 클래스 (실행점: main() 함수) : Ex01_variable
>	3. 함수 : public static void main(String[] args){} : 프로그램 시작점, 진입점
>>	TIP) C# > void Main() -	class Ex01_variable 

>## 변수 : variable
>###	변수 Scope (유효범위) : 선언되는 위치에 따라서
>	1. instance variable : 객체변수 (설계도가 가지고 있는 변수) class Test{선언되는 위치} <br>
>	2. local variable     : 지역변수 (함수안에 있는 변수) class Test{void run(){선언되는 위치}} <br>
>	3. static variable    : 공유변수 <br>
~~~java
//설계도 == class
class Test{
	int iv = 500;
  //instance variable : 객체변수(설계도가 가지고 있는 변수)
  //이 변수는 초기화를 하지 않아도 된다(기본값 : default)를 가지고 있다
  //why? 왜? 초기화를 하지 않아도 사용가능 할까요
  //답) 
  String sv; // 객체변수
  
  void print() {
    int lv = 100; // local variable
  } 
  void write() {
    System.out.println("iv:전역변수 : " + iv);

    //Error
    //lv 변수는 print(){내부에 있는 변수}
    //System.out.println("lv:지역변수 : " + lv);
  }
}
~~~
