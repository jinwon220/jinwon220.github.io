---
title: Thread(3)_Priority
author: JinWon
layout: post
category: Java
---

# Priority
- 우선순위 : CPU 점유율을 높이겠다
- default : 5 (Max : 10, Min : 1)

~~~java
class Pth extends Thread{
	@Override
	public void run() {
		for(int i = 0; i < 1000; i++) {
			System.out.println("--------");
		}
	}
}
class Pth2 extends Thread{
	@Override
	public void run() {
		for(int i = 0; i < 1000; i++) {
			System.out.println("||||||||");
		}
	}
}
class Pth3 extends Thread{
	@Override
	public void run() {
		for(int i = 0; i < 1000; i++) {
			System.out.println("********");
		}
	}
}
public class Ex06_priority {
	public static void main(String[] args) {
		Pth pth = new Pth();
		Pth2 pth2 = new Pth2();
		Pth3 pth3 = new Pth3();
		
		System.out.println(pth.getPriority()); //기본값 5
		System.out.println(pth2.getPriority()); //기본값 5
		System.out.println(pth3.getPriority()); //기본값 5
		
		//숫자가 많이 높을수록 cpu점유율이 높아짐
		//실행을 먼저 완료 할 가능성이 높음
		pth.setPriority(10);  
		pth2.setPriority(3);
		pth3.setPriority(1);
		
		pth.start();
		pth2.start();
		pth3.start();
		
		System.out.println("Main END.........");
	}

}
~~~