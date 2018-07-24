---
title: CollectionFramework(2)_ArrayList
author: JinWon
layout: post
category: Java
---

# ArrayList

> ToDay KeyPoint (ArrayList)
- ArrayList 통해서 객체 다루기

~~~java
public class Ex02_ArrayList {
	public static void main(String[] args) {
		ArrayList arrayList = new ArrayList();
		arrayList.add(100);
		arrayList.add(200);
		arrayList.add(300);
		
		System.out.println(arrayList.toString());
		for(int i=0; i<arrayList.size(); i++) {
			System.out.println(arrayList.get(i));
		}
		/*
		for(Object object : arrayList) {
			System.out.println(object);
		}
		*/
		System.out.println("현재[0]" + arrayList.get(0));
		arrayList.add(0, 1111); //들여쓰기 (밀어내서 쓰기)
		//비순차적인 데이터 추가 삭제 (:<) -> 중간 이나 첫부분에 추가 삭제
		//순차적인 데이터 추가 삭제 (:D) -> 마지막쪽 추가 삭제
		System.out.println("현재[0]" + arrayList.get(0));
		System.out.println(arrayList.toString());
		
		//데이터 삽입 (add) : 중간에 -> 데이터 이동
		//처음, 중간 (비순차적인) 데이터 추가, 삭제 하는 작업은 성능상 좋지 않다
		//순차적인 데이터 추가 , 삭제  좋아요
		
		//ArrayList 함수 활용
		System.out.println(arrayList.contains(200));
		System.out.println(arrayList.contains(333));
		
		System.out.println(arrayList.isEmpty()); //너 비어 있니(true, false)
		arrayList.clear();
		System.out.println(arrayList.isEmpty()); //clear >> size == 0 >> true
		
		arrayList.add(101);
		arrayList.add(102);
		arrayList.add(103);
		System.out.println(arrayList.toString());
		
		//0번째 방에 있는 데이터 삭제
		Object value = arrayList.remove(0); //필요하다면 지우는 값을 돌려 받을 수 있다.
		System.out.println("value : " + value);
		System.out.println(arrayList.toString());
		
		ArrayList list = new ArrayList();
		list.add("가");
		list.add("나");
		list.add("다");
		list.add("가");
		
		System.out.println("ArrayList:순서유지 : " + list);
		list.remove("가"); // 값을 주면 앞에서 찾아서 삭제
		System.out.println("ArrayList 삭제  : " + list);
		
		//집중
		//List 인터페이스를 부모 타입으로
		List li = new ArrayList();
		li = new Vector();
		//void move(List li){}
		
		li.add("가");
		li.add("나");
		li.add("다");
		li.add("라");
		
		List li4 = li.subList(0, 2); // new ArrayList() >> add("가"), add("나")
		System.out.println(li);
		System.out.println(li4);
		
		ArrayList alist = new ArrayList();
		alist.add(50);
		alist.add(1);
		alist.add(7);
		alist.add(40);
		alist.add(7);
		alist.add(15);
		
		System.out.println("before : " + alist);
		//Arrays.sort(); 보조클래스
		Collections.sort(alist);
		
		System.out.println("after : " + alist);
	}

}
~~~

# ArrayList_Object_KeyPoint

~~~java
public class Ex03_ArrayList_Object_KeyPoint {
	public static void main(String[] args) {
		//정적배열(Array)
		//사원 1명을 만드세요
		Emp e = new Emp(100, "김유신", "군인");
		System.out.println(e);
		
		//정적배열(Array)
		//사원 2명을 만드세요
		Emp[] e2 = {new Emp(101, "김씨", "IT"),
					new Emp(102, "박씨", "SALES")};
		
		for(Emp em : e2) {
			System.out.println(em);
		}
		///////////////기존 배열(정적) 복습///////////////
		
		//ArrayList 를 사용해서
		//사원 2명을 만드세요
		ArrayList e3 = new ArrayList(2);
		e3.add(new Emp(103, "이씨", "GG"));
		e3.add(new Emp(104, "정씨", "Good"));
		System.out.println(e3);
		
		//for문을 사용해서 사원데이터 정보를 출력하는 toString() 사용 금지
		//for문을 개선된 for문 X 일반 for문을 통해서 출력
		for(int i = 0; i < e3.size(); i++) {
			//System.out.println(e3.get(i).toString());
			//System.out.println(((Emp)e3.get(i)).toString());
			Emp m = (Emp)e3.get(i);
			System.out.println(m.getEmpno()+" / " + m.getEname() + " / " + m.getJob());
		}
		//개선된 for문
		for(Object obj : e3) {
			Emp m = (Emp)obj;
			System.out.println(m.getEmpno());
		}
		//Object 불편
		//generic (100%사용)
		ArrayList<Emp> e4 = new ArrayList<>();
		e4.add(new Emp(105, "A", "IT"));
		e4.add(new Emp(106, "B", "SALES"));
		
		for(Emp em : e4) {
			System.out.println(em.getEmpno() + " / " + em.getEname() + " / " + em.getJob());
		}
	}

}
~~~

# ArrayList_Parameter

~~~java
class EmpData{
	private ArrayList elist;
	private int[] numbers;
	
	EmpData(){
		this.elist = new ArrayList();
		this.numbers = new int[10];
	}
	
	//getter
	public ArrayList getElist() {
		return this.elist;
	}
	//setter
	public void setElist(ArrayList elist) {
		this.elist = elist;
	}
	public int[] getNumbers(){
		return this.numbers;
	}
	public void setNumbers(int[] numbers) {
		this.numbers = numbers;
	}
}

public class Ex04_ArrayList_Parameter {
	public static void main(String[] args) {
		EmpData empData = new EmpData();
		System.out.println(empData);
		System.out.println(empData.getElist().toString());
		
		ArrayList li = new ArrayList();
		li.add(100);
		li.add(200);
		li.add(300);
		
		empData.setElist(li);
		System.out.println(empData.getElist().toString());
	}

}
~~~