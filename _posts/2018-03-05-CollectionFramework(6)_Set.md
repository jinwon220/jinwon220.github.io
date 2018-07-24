---
title: CollectionFramework(6)_Set
author: JinWon
layout: post
category: Java
---

# Set Interface

#### Set 인터페이스 구현하는 클래스
- Set 순서(X), 중복(X) 이런 데이터 집합을 다룰 때
- HashSet, TreeSet
- 순서 (넣은 순서를 보장하지 않아요)

~~~java
public class Ex10_Set_Interface {
	public static void main(String[] args) {
		HashSet<Integer> hs = new HashSet<>();
		hs.add(1);
		hs.add(100);
		hs.add(55);
		System.out.println(hs.toString());
		//중복 데이터 처리(POINT)
		boolean bo = hs.add(1);
		System.out.println(bo);
		System.out.println(hs);
		hs.add(2);
		System.out.println(hs);
		
		HashSet<String> hs2 = new HashSet<>();
		hs2.add("b");
		hs2.add("A");
		hs2.add("F");
		hs2.add("c");
		hs2.add("z");
		System.out.println(hs2);
		
		//1. 중복 허락하지 않아요
		String[] obj = {"A", "B", "A", "C", "D", "B", "A"};
		HashSet<String> hs3 = new HashSet<>();
		for(int i = 0; i < obj.length; i++) {
			hs3.add(obj[i]);
		}
		System.out.println(hs3.toString());
		
		//Quiz
		//HashSet을 사용해서 1~45까지의 난수 6개 넣으세요
		//단, 중복값 X
		Set<Integer> set = new HashSet<>();
		while(set.size() < 6) {
			set.add((int)((Math.random()*45)+1));
		}
		System.out.println(set);
		Set<Integer> set2 = new HashSet<>();
		for(;set2.size() < 6;) {
			set2.add((int)((Math.random()*45)+1));
		}
		System.out.println(set2);
		
		
		Set<String> set3 = new HashSet<>();
		set3.add("AA");
		set3.add("DD");
		set3.add("ABC");
		set3.add("FFFF");
		System.out.println(set3);
		//순서 (add 한 순서) 보장 하지 않는다(배열이 아니예요)
		
		Iterator<String> s= set3.iterator();
		while(s.hasNext()) {
			System.out.println(s.next());
		}
		
		//Collections.sort(list); //List 인터페이스 구현한 객체
		//Collections.reverse(list); // 내림차순
		
		//Set 인터페이스 구현 자원 : sort의 의미가 없음(순서가 없기 때문에) => 수의 집합
		//굳이 sort를 하고 싶으면 방법은 있다. 
		List list = new ArrayList(set3);
		//new ArrayList(Collection<> c)
		System.out.println("before 무작위 : " + list);
		Collections.sort(list);
		System.out.println("after 정렬 : " + list);
		
	}
}
~~~

# Set TreeSet
~~~java
public class Ex11_Set_TreeSet {
	public static void main(String[] args) {
		//순서유지(x) , 중복(x)
		Set<String> hs = new HashSet<>();
		hs.add("B");
		hs.add("A");
		hs.add("F");
		hs.add("K");
		hs.add("G");
		hs.add("D");
		hs.add("P");
		hs.add("A");
		System.out.println(hs.toString());
		
		//HashSet 확장 > LinkedHashSet (내부적으로 순서 유지) : Linked(객체가 주소를 가지고 있다) >> node
		//										LinkedList, LinkedHashSet
		Set<String> hs2 = new LinkedHashSet<>();
		hs2.add("B");
		hs2.add("A");
		hs2.add("F");
		hs2.add("K");
		hs2.add("G");
		hs2.add("D");
		hs2.add("P");
		hs2.add("A");
		//Array(배열) X!!!! 주소값들을 연결한 것!!!
		System.out.println(hs2.toString());
		
		//자료구조 (순서(x), 중복(x), 정렬(o))
		//검색 하거나, 정렬을 필요로 하는 자료구조 (알고리즘)
		//TreeSet
		//데이터 트리(이진트리) : 정렬되고 많은 양의 데이터 저장 효율적
		//검색 속도
		//TreeSet을 사용 해서 로또 를 구현하세요
		//1~45난수 >> 6개 >> 중복값(x) >> 정렬(o)
		//결과 출력  (Iterator)
		System.out.println("--------------");
		
		
		Set<Integer> lottos = new TreeSet<>();
		while(lottos.size() < 6) {
			lottos.add((int)((Math.random()*45)+1));
		}
		System.out.println(lottos);
		
		Iterator<Integer> readline = lottos.iterator();
		int i = 1;
		while(readline.hasNext()) {
			System.out.println(i++ +"번째 수 : " + readline.next());
		}
		
		
	}

}
~~~