---
title: ETC(4)_Wrapper
author: JinWon
layout: post
category: Java
---

# Wrapper

- 기본 타입(값타입) : 8가지  >> JAVA API 제공
- 8가지 기본 타입에 대해서 객체 형태로 사용가능 하도록 만든 것
- wrapper class (참조 타입)

### 기본형 타입 변수도 때로는 객체형태로 다루어져야 하는 경우
1. 매개변수로 객체가 요구 될때
2. 기본형 값이 아닌 객체로 저장되어야 할때
3. 객체간의 비교가 필요할 때

> 이때, wrapper 사용하면 ... <br>
wrapper 클래스에는 각 타입의 정보(최소크기, 최대크기 상수값 형태로 저장)

~~~
ex) Integer.parseInt(s);
	ArrayList<Integer> li = new ArrayList<>(); >> parameter
	System.out.println(Integer.MIN_VALUE);
	System.out.println(Integer.MAX_VALUE);
~~~

~~~java
public class Ex09_Wrapper_Class {
	public static void main(String[] args) {
		int i =100;
		Integer n = new Integer(500);
		System.out.println("i = " + i);
		System.out.println("n = " + n); //재정의 toString
		n = i;
		System.out.println(n);
		
		//parameter 값타입이 객체형태로 상속 
		//활용도가 제일 높은 코드
		//Generic 사용시 (값은 wrapper 클래스 사용)
		ArrayList<Integer> li = new ArrayList<>();
		li.add(100);
		li.add(200);
		for(int r : li) {
			System.out.println(r);
		}
		Integer i2 = new Integer(100);
		Integer i3 = new Integer(100);
		System.out.println(i2 == i3); //false 주소가 다르기 때문에
		System.out.println((int)i2 == (int)i3); //true
		System.out.println(i2.equals(i3)); //true
		//Object 가지는 equals 함수 (값 비교) Integer 클래스에서 재정의
		
		Integer t = new Integer(500);
		integerMethod(t);//t 참조변수 주소값
		intMethod(t); //t라는 참조변수가 가지는 값
	}
	static void integerMethod(Integer i) {
		System.out.println("Integer param : " + i);
	}
	static void intMethod(int i) {
		System.out.println("int param : " + i);
	}

}
~~~