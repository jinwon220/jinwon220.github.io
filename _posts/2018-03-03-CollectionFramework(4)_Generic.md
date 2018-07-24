---
title: CollectionFramework(4)_Generic
author: JinWon
layout: post
category: Java
---

# Generic

- JDK 1.5 version 부터 지원
- Java, C# 필수기능

1. Object 타입 저항 -> Object 타입 탈피
2. 타입 안전성 (타입 강제성)
3. 형변환(casting) 문제 해결 : (Car)obj;

~~~java
class Person{
	int age = 100;
}

//Generic 클래스 설계 >> 타입을 강제해서 객체를 생성
//MyGen<String> m = new MyGen<String>();
class MyGen<T>{ //Type Parameter
	T obj;
	void add(T obj) {
		this.obj = obj;
	}
	T get() {
		return this.obj;
	}
}

/*
//MyGen<String> m = new MyGen<String>();
  
class MyGen<String>{ //Type Parameter
	String obj;
	void add(String obj) {
		this.obj = obj;
	}
	T get() {
		return this.obj;
	}
}
*/

public class Ex06_Generic {
	public static void main(String[] args) {
		MyGen<String> my = new MyGen<>();
		my.add("Hello");
		String result = my.get();
		System.out.println(result);
		
		//Person 타입을 강제하는
		//MyGen 타입의 객체 생성..
		MyGen<Person> myobj = new MyGen<>();
		myobj.add(new Person());
		Person p = myobj.get();
		System.out.println(p.age);
		
		//Quiz (Generic x)
		ArrayList list = new ArrayList();
		list.add(500);
		list.add(new Person());
		list.add("홍길동");
		
		for(Object o : list) {
			if(o instanceof Person) {
				Person pe = (Person)o;
				System.out.println(pe.age);
			}else System.out.println(o);
		}
		
		//위 처럼 사용하지 말자
		//위 처럼 사용하지 말자
		 ArrayList<Person> alist = new ArrayList<>();
		 alist.add(new Person());
		 System.out.println(alist.get(0).age);
		 
		///////////////////////////////////
		//JAVA API 제공하느  Stack 클래스
		Stack<Coin> coinBox = new Stack<Coin>();
		
		// 동전케이스에 동전을 끼움
		// 100원, 50원, 500원, 10원 순서로 넣는다. 
		coinBox.push(new Coin(100));
		coinBox.push(new Coin(50));
		coinBox.push(new Coin(500));
		coinBox.push(new Coin(10));
		
		while(!coinBox.isEmpty()){
		Coin coin = coinBox.pop();
		System.out.println("꺼내온 동전 : " + coin.getValue() + "원");
		}
	}

}
~~~