---
title: Thread(6)_Sync
author: JinWon
layout: post
category: Java
---

# Sync
~~~
동기화라는 단어를 사용할려면 Thread가 들어가야함 
Thread가 없으면 동기화가 아닌 순차적인 접근이다
동기화
한강 화장실(공유 자원) : 여러명의 사용자가 (10명의 사용자 : Thread 10개)
lock -> synchronized
함수단위 lock

한강 비빔밥 축제(공유자원) 동시접근 처리
~~~

~~~java
class Wroom{
	public synchronized void openDoor(String name) {
		System.out.println(name + "님 화장실 입장^^");
		for(int i = 0; i < 100; i++) {
			System.out.println(name + "사용중" + i);
			if(i == 10) {
				System.out.println(name + "님 끙!!");
			}
		}
		System.out.println("시원하다...");
	}
}

class User extends Thread{
	private Wroom wr;
	private String who;
	
	public User(String name, Wroom w) {
		this.who = name;
		this.wr = w;
	}

	@Override
	public void run() {
		wr.openDoor(this.who);
	}
}

public class Ex09_Sync_Thread {
	public static void main(String[] args) {
		//한강둔치
		Wroom w = new Wroom();
		
		//사람들
		User kim = new User("김씨", w);
		User lee = new User("이씨", w);
		User park = new User("박씨", w);
		/*System.out.println(kim.getPriority()); 
		System.out.println(lee.getPriority()); 
		System.out.println(park.getPriority()); 
		kim.setPriority(1);
		lee.setPriority(2);
		park.setPriority(10);*/
		kim.start(); //stack 할당 받고 run 함수 올려놓기
		lee.start();
		park.start();
		
	}

}
~~~

### Quiz
~~~
은행계좌를 하나 가지고 있다
은행 계좌를 통해 입금 , 출금  처리를 할 수 있다

친구 5명(똑같은 카드 )
동시에 계좌 출금

통장 1000만원
ATM 기기에서 동시에  출금

개발자 
누군가 [출금 ~ 행위]끝날때 까지 LOCK 통해서 자원 보호  
~~~
~~~java
class Account{ //계좌
	int balance = 1000; //잔액
	 synchronized void withDraw(int money) { //출금  //동기화
		System.out.println("고객 : " + Thread.currentThread().getName());
		System.out.println("현재 잔액 : " + this.balance);
		if(this.balance >= money) {
			try {
				Thread.sleep(1000); //은행업무 처리되는 과정(인증 ,카드 확인....)
			}catch (Exception e) {
				System.out.println(e.getMessage());
			}
			this.balance -= money;
		}
		System.out.println("인출금액 : " + money);
		System.out.println("인출후 잔액 :" + this.balance);
	}
	
}
class Bank implements Runnable{
	Account acc = new Account();
	@Override
	public void run() {
		while(acc.balance > 0) {
			
			int money = ((int)(Math.random()*3)+1)*100;
			//실 출금 처리
			acc.withDraw(money);
		}
		
	}
	
}



public class Ex10_Sync_Thread {

	public static void main(String[] args) {
		Bank bank = new Bank();
		
		Thread th = new Thread(bank,"Park");
		Thread th2 = new Thread(bank,"Kim");
		Thread th3 = new Thread(bank,"Lee");
		
		th.start();
		th2.start();
		th3.start();
	}

}
~~~