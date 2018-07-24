---
title: CollectionFramework(1)_Vector
author: JinWon
layout: post
category: Java
---

# Vector

### Collection FrameWork
### [다수의 데이터를 다루는 표준화된 인터페이스를 구현하고 있는 클레스의 집합]

~~~
Collection 인터페이스 -> List 인터페이스(상속) -> 구현(ArrayList...)
				 -> Set 인터페이스(상속) -> 구현(HashSet , TreeSet...)

Map 인터페이스 -> 구현(HashMap...)
~~~
---------------------------------------------------------------
~~~
--List
순서가 있는 데이터 집합
--Set
순서가 없는 데이터 집합
~~~

----List----
--
List Interface (줄을 서시오) <br>
: 순서(O), 중복(O) -> 내부적으로 데이터(자료, 객체) -> Array(배열) [0][1][2][3]

1. Vector(구버전) (잘 사용하지 않음) -> 동기화(멀티 쓰레드) -> 보장(Lock) default -> 보호 (성능이 느려짐)
2. ArrayList(신버전) (많이 사용함) -> 동기화(멀티 쓰레드) -> 보장(Lock)x -> 필요에 따라 (Lock) 사용 -> 좀 더 빠른 성능

ex) 공중화장실이 1개 (100명이 동시에) -> 화장실에 한명이 들어가면 기다렸다가 순서대로 들어가야함 <br>
ex) 비빔밥 축제(100인분/한통에) (100명이 동시에) -> 한통에 들어있는 비빔밥을 100명이 동시에 와서 먹을 수 있음

-------------------------------------
### Array (배열)
- 방의 개수가 고정(한번 방의 크기를 고정하면 변경 X(불가))
- int[] arr =new int[10]; -> 10개  필요에 따라 11개로 늘릴 수 없음!! 
- int[] arr =[10, 20, 30];-> 3개  필요에 따라 4개로 늘릴 수 없음!!

### Array (고정, 정적)
1. 배열의 크기 고정 : Car[] cars = {new Car(), new Car()}; 방 2개 고정
2. 배열의 접근 : index(첨자) 접근 >> cars[0], cars[0].name
3. 초기에 설정한 크기는 변경 불가 
------------------------

### List (동적) 인터페이스를 구현한 클래스
1. 배열의 크기를 동적 확장, 축소가 가능  => 배열의 재할당(배열을 새로 만들어서 주소를 옮김)
2. 순서를 유지(Array)사용, 순서->index를 가진다, 중복 값 허용
3. 데이터 저장 공간(Array)사용
4. List 인터페이스를 구현하는 모든 자원 클래스(new 를 통해 객체 생성)
5. 구현 클래스의 default Type(자료구조) : Object타입  >> 장점(자료넣기) >> int, Car .. 
   - 단점(빼낼대에 타입 체크 -> 빈번한 casting)
   - casting(x) >> Object obj = .... 가능은 하지만 너무 많은 코딩필요 (무식한 방법)
   
6. 위 Object 타입 단점을 극복하고자 ->  제너릭<> (타입을 강제)

~~~java
public class Ex01_Vector {
	public static void main(String[] args) {
		Vector v = new Vector();
		System.out.println("초기 default 용량 : "+v.capacity()); //10개의 default
		v.add("AA");
		v.add("BB");
		v.add("AA"); //중복가능
		v.add(100); //방마다 모든 타입 가능(Object 타입)
		
		System.out.println(v); // v.toString() default
		System.out.println(v.toString()); //toString()을 재정의 한것이다. toString()은 원래 주소값
		
		//Array >> length() >> Car[index]
		
		//List 계열 : size() 데이터 개수
		for (int i = 0; i < v.size(); i++) {
			System.out.println(v.get(i));//get return Type >> Object
		}
		//개선된 for문
		for (Object o : v) {
			System.out.println(o);
		}
		//제너릭(Object 배제..>> 타입을 강제)
		Vector<String> v2 = new Vector<String>(); // new Vector<>(); <>안 String 앞에 지정 했다면 생략가능
		v2.add("Hello");
		v2.add("World");
		v2.add("King");
		for(String str : v2) {
			System.out.println(str);
		}
		
		System.out.println(v2.toString()); //v2 = v2.toString()
		System.out.println(v2.get(2));
		System.out.println("size : " + v.size());
		System.out.println("capacity : " + v.capacity());
		
		Vector v3 = new Vector();
		System.out.println(v3.capacity());
		v3.add("A");
		v3.add("A");
		v3.add("A");
		v3.add("A");
		v3.add("A");
		v3.add("A");
		v3.add("A");
		v3.add("A");
		v3.add("A");
		v3.add("A");
		System.out.println(v3.capacity());
		v3.add("A");
		System.out.println("size : " + v3.size());
		System.out.println("11번째  : " + v3.capacity());
		
	}
}
~~~