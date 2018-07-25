---
title: Thread(2)_Multi
author: JinWon
layout: post
category: Java
---

# Single_Multi

> Thread : 프로세스에서 하나의 최소 실행단위 (method)

### Thread 생성방법
1. Thread 클래스를 상속 -> class Test extends Thread{}
	- 반드시 Thread 상속할 경우 run()함수 재정의

2. implements Runnable 구현 -> class Test implements Runnable
	- 반드시 run() 추상함수 재정의

> POINT : Thread_2 implements Runnable -> Thread(X) (Thread가 될 수 있는 요건만 가지고 있다.)
~~~
왜 2가지 제공
class Test extends Car implements Runnable : Test가 다른 클래스를 상속할 수 있기 때문에 구현 방법이 있다.
~~~
- Thread 추상 클래스가 아닌 일반 클래스
- 추상클래스 : new (x)
- 인터페이스로 구현한 것을 new할 수 없기 때문에

~~~java
class Thread_1 extends Thread{
	
	@Override
	public void run() { //새로운 static의 처음 올라가는 함수
		for(int i = 0; i < 1000; i++) {
			System.out.println("Thread_1 : " + i + this.getName());
		}
		System.out.println("Thread_1 run() END");
	}
}
class Thread_2 implements Runnable{

	@Override
	public void run() {
		for(int i = 0; i < 1000; i++) {
			System.out.println("Thread_2 : " + i);//+ this.getName()불가
		}
		System.out.println("Thread_2 run() END");
	}
}

public class Ex02_Multi_Thread {
	public static void main(String[] args) {
		System.out.println("Main Start");
		
		//1번
		Thread_1 th = new Thread_1();
		th.start(); //POINT > stack 생성하고 stack run()을 올려놓는 역할 -->> start()
		
		//2번
		Thread_2 th2 = new Thread_2();
		Thread thread = new Thread(th2); //일반 클래스
		thread.start();
		
		for(int i = 0; i < 1000; i++) {
			System.out.println("main : " + i);//+ this.getName()불가
		}
		System.out.println("main END");
	}

}
~~~