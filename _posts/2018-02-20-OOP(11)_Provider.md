---
title: OOP(11)_Provider
author: JinWon
layout: post
category: Java
---

# Provider

User(사용자) : Provider(제공자)

~~~
class A{} , class B{}
A는 B를 사용합니다.
~~~

1. 상속 : A extends B
2. 포함 : member field형태로 : A클래스 안에 B라는 클래스가 들어오는 것 (member field) class A{B b;} -> 관계 
3. 포함 : 함수 (method parameter) : 의존관계 -> class A{void print(B b){}}

~~~java
interface Icall{
	void m();
}

class D implements Icall{
	@Override
	public void m() {
		System.out.println("D Icall 인터페이스의 m() 구현");
	}	
}

class F implements Icall{
	@Override
	public void m() {
		System.out.println("F Icall 인터페이스의 m() 구현");
	}	
}

// 현대적인 프로그램 기법 : interface
class C {
	void method(Icall ic) { //인터페이스 타입으로 parameter : 간접 (현대적인 : 유연한것)
		ic.m();
	}
}
public class Ex05_User_Provider {
	public static void main(String[] args) {
		C c = new C();
		c.method(new D());
		c.method(new F());
	}
}
~~~