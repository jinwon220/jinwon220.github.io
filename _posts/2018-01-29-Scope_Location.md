---
title: Ex02_Scope_Location
author: JinWon
layout: post
category: Java
---


* class = 설계도 == 데이터 타입(Data Type) <br>
(new) -> 메모리에 올릴 때 사용하는 메소드

* 객체 = class(설계도) 를 가지고 구체화 된 것.(메모리에 오른 것) <br> class가 객체화 되는 과정 : instance화 (인스턴스화)
 
* 클래스의 구성요소 : 필드(고유, 상태, 참조) <br> 생성자(필드의 초기화를 목적으로 하는 상수) 함수 <br> 접근자(한정자) : public, private, default, protected(상속관계에서만 쓰임)

1. public class Test{}

2. class Test{} >> default 접근자가 생략되어 있다. <br>
(default 접근자는 폴더 위치 : 같은 폴더 안에 있느냐, 다른 폴더에 있느냐, 에 따라 자원 사용여부 판단)

3. default class 언제 사용하는가? (같은 폴더 내)에서 (다른 클래스)의 참조 목적으로 사용


4. 하나의 자바 파일은 여러개의 클래스를 가질 수 있다. (Test.java) <br>
- (참, true) <br>
- Ex01_main.java 라는 파일은 public class Ex01_main 이라는 클래스를 가지고 있다. <br>
- class TTT{} 와 class PPP{} 등 클래스를 더 가질 수 있다. <br>

~~~java
class TTT{ // default class TTT (같은 폴더에 같은 class 명은 존재 할 수 없다.)
	int a; //default
	
	void print() { //default
		System.out.println(a);
	}
} 
class PPP{}

public class Ex01_main {

	public static void main(String[] args) {
		//1.kr.or.bit.common 폴더 안에 있는 클래스를 사용하고 싶을 때
		//또는 import kr.or.bit.common.CommonClass
		//단, class 접근자가 public일 경우
		//kr.or.bit.common.CommonClass c = new kr.or.bit.common.CommonClass();
		// public 
		
		CommonClassPublic common = new CommonClassPublic();
		// common.a  a라는 변수 : public
		// common.b  default 접근  -> 같은 폴더라면 접근  현재 같은 폴더 x 사용 불가
		// common.c  private

	}

}

~~~