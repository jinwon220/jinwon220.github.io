---
title: OOP(9)_Abstract
author: JinWon
layout: post
category: Java
---

# Abstract

### 추상클래스
- 미완성 설계도
- 미완성 클래스 (완성된 코드 + 미완성 코드)
- 미완성 코드 : 미완성 함수 (함수가 {실행블럭} 을 가지고 있지 않다.(없다) <br>
          ex) public void print();
- 완성 과 미완성의 차이 (new를 통해 객체를 생성(완성), 생성하지 못하면(미완성))

1. 추상클래스는 스스로 객체 생성 불가(new 사용 불가)
2. 추상클래스는 결국 완성된 클래스를 만들어서 사용 -> [상속]을 통해서  =>상속이 없으면 추상클래스는 없다
>미완성 자원(추상함수) 완성해라(구현) -> 재정의를 통해(override)

>why? 추상클래스 >> 설계자가 바라보는 진정한 의미 : 강제적 구현을 목적으로 한다.

~~~java
// 상속의 부모타입에 자식타입의 주소값을 넣을 수 있고
// 추상함수를 볼 수 있다 (why? 재정의 된 함수는 자식함수를 따라가기 때문에)
abstract class Abclass{
	int pos;
	void run() {
		pos++;
	}
	//여기까지의 코드는 완성 된 코드
	
	//추상자원 (추상함수)
	abstract void stop(); //{실행블럭}이 없다.
}

class Child extends Abclass{
	@Override
	void stop() { //stop이라는 함수 이름으로 재정의 코드
		this.pos = 0;
		System.out.println("stop : " + this.pos);
	}
}

public class Ex01_abstract_class {
	public static void main(String[] args) {
		// Abclass ab = new Abclass(); 불가
		Child ch = new Child();
		ch.run();
		ch.run();
		System.out.println("현재 pos : " + ch.pos);
		ch.stop();
		
		Abclass ab = ch;
		ab.run();
		System.out.println("현재 pos : " + ch.pos);
		ab.stop();
	}

}
~~~

~~~
스타크래프트
유닛(umit)

unit 공통기능 (이동좌표, 이동, 멈춘다)
unit 이동 방법 다르다(unit 마다 각각의 이동방법이 다르다)
abstract class 이름 {abstract method 강제 구현}
~~~

~~~java
abstract class Unit{
	int x, y;
	void stop() {
		System.out.println("Unit Stop");
	}
	
	//이동
	abstract void move(int x, int y); //{날아가기} , {걸어가기}
}

//move 추상화 -> 구체화 (특수화)
class Tank extends Unit{

	@Override
	void move(int x, int y) {
		this.x = x;
		this.y = y;
		System.out.println("Tank 이동 : " + this.x + ", " + this.y);
	}
	
	//Tank 가 가지는 구체화(특수화) 고유한 기능
	void changeMode() {
		System.out.println("탱크 변환 모드");
	}
	//필요하다면 구현
}

class Marine extends Unit{

	@Override
	void move(int x, int y) {
		this.x = x;
		this.y = y;
		System.out.println("Marine 이동  : " + this.x + ", " + this.y);
	}
	
	void stimpack() {
		System.out.println("스팀팩기능");
	}
}

class DropShip extends Unit{
	
	@Override
	void move(int x, int y) {
		this.x = x;
		this.y = y;
		System.out.println("공중 이동  : " + this.x + ", " + this.y);
	}
	void load() {
		System.out.println("Unit load");
	}
	void unloa() {
		System.out.println("Unit unload");
	}
}

public class Ex02_abstract_class {
	public static void main(String[] args) {
		Tank t = new Tank();
		t.move(100, 200);
		t.stop();
		t.changeMode();
		
		Marine m = new Marine();
		m.move(300, 200);
		m.stop();
		m.stimpack();
		
		//1.Quiz 탱크 3대를 만들고 [같은 좌표]로 이동
		System.out.println("----------");
		int i = 0;
		Tank[] t1 = {new Tank(), new Tank(), new Tank()}; 
		for(Tank a : t1) {
			System.out.printf("[%d]번째 탱크\n", i++);
			a.move(10, 50);
		}
		//2.여러개의 Unit (Tank, Marine, DropShip) 같은 좌표로 이동
		System.out.println("----------");
		i = 0;
		Unit[] unit = {new Tank(), new Marine(), new DropShip()};
		for(Unit u : unit) {
			System.out.printf("[%d]번째 유닛\n", i++);
			u.move(20, 70);
		}
	}
}
~~~