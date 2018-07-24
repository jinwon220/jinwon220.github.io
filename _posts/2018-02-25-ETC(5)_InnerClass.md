---
title: ETC(5)_InnerClass
author: JinWon
layout: post
category: Java
---

# InnerClass

> http://cafe.naver.com/bitcamp104/571 참고 <br>
> 활용 : awt, swing, android (event 처리) : 버튼 클릭, 마우스 오버

### inner class
- 클래스안에 클래스가 있다

~~~java
class OuterClass{
	public int pdata = 10;
	private int data = 30;
	
	//inner class(자원에 대한 접근을  편하게)
	//member field 가 선언되는 위치에
	class InnerClass{
		void msg() {
			System.out.println("outerclass data : " + data);
			System.out.println("outerclass data : " + pdata);
		}
	}
}

abstract class Person2{
	abstract void eat();
}

//////////////////////////
interface Eatable{
	void eat();
}
class Test{
	void method(Eatable e) {
		e.eat();
	}
}
class Man extends Person2{

	@Override
	void eat() {
		System.out.println("Person2함수의 재정의");
	}
}

public class Ex10_InnerClass {

	public static void main(String[] args) {
		OuterClass outobj = new OuterClass();
		System.out.println("public 자원 : " + outobj.pdata);
		System.out.println("private 자원 : 볼 수 없음");
		
		OuterClass.InnerClass inobj = outobj.new InnerClass();
		inobj.msg(); //outer 클래스에 대한 자원 접근 용이
		
		Man m = new Man();
		m.eat();
		Person2 p2 = m;
		p2.eat();
		
		Person2 p3 = new Man();
		p3.eat();
		
		//abstract class Person2 어차피 강제 구현을 목적으로
		//추상클래스는 객체 생성 불가능
		//Person2 상속하는 클래스 없이도 1회성으로 사용가능한 클래스(이름이 없는 클래스) : 익명클래스
		Person2 p4 = new Person2() {
			@Override
			void eat() {
				System.out.println("익명 타입으로 구현");
			}
		};
		p4.eat();
		
		
		//Today POINT
		Test t = new Test();
		t.method(new Eatable() {
			
			@Override
			public void eat() {
				System.out.println("일회성 자원으로 인터페이스를 직접 구현");
			}
		});
		
	}

}
~~~