---
title: OOP(8)_Singleton
author: JinWon
layout: post
category: Java
---

# Singleton

디자인 패턴(생성패턴)
- singleton >> 객체를 하나만 만들어서 공유

하나의 객체를 생성해서[공유하고 싶다면] ...공유 = static

싱글톤패턴(singleton 패턴)
1. 여러개의 객체를 생성(new 연산자)를 막는다.
2. 하나의 객체를 같이 공유 사용
3. 결국 하나의 객체만 만들어 사용하게 하자

~~~
실생활 활용 예)
회사 : 공유프린터(여러대의 컴퓨터와 연결되어있음) , 네트워크 자원...
JDBC 수업 때 활용 하는 코드 할 꺼!!!
~~~
~~~java
public class Singleton {
	//p라는 변수가 singleton객체의 주소를 가지고 있게
	private static Singleton p; //캡슐화 
	
	//생성자 private 접근자 객체를 생성하지 못하게
	private Singleton() {} //캡슐화 
	
	public static Singleton getInstance() {
		if(p == null) { //주소값이 없을 경우 
			//주소값 생성
			p = new Singleton(); //생성자 호출해서 메모리..
		}
		//주소값이 있다면 가지고 있던 주소값 리턴
		return p; //주소값 리턴
	}
}
~~~