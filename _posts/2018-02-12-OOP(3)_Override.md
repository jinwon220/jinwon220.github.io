---
title: OOP(3)_Override
author: JinWon
layout: post
category: Java
---

# Override

TODAY POINT : [상속관계]에서 override : 상속관계에서 메서드 재정의
- 상속관계에서 자식클래스가 부모클래스에 메서드를 재정의
- 재정의 (함수) (상속관계) (부모꺼를) (자식이)
- 상속이 없으면 override가 없다.
- 재정의 의미 : 틀의 변화가 아니라 내용의 변화. -> void 함수명{ 괄호안에 내용만 변경 }

### 문법) override
1. 부모함수이름과 동일
2. 부모함수와 parameter 동일
3. 부모함수와 return type 동일
4. 결국 {괄호 안에 있는 내용만 수정} 

~~~java
class Point2{
	int x = 4;
	int y = 5;
	String getPosition() {
		return "x:" + this.x + " y:" + this.y;
	}
}
class Point3D extends Point2{
	//x, y 상속관계 부모 자원 사용
	int z = 6;
	//String getPosition3() {
	//	return "x:" + this.x + " y:" + this.y + " z:" +this.z;
	//}

	//POINT : @Override >> Annotation
	//Annotation은 Java code만으로 전달할 수 없는 부가적인 정보를 컴파일러나 개발툴로 전달할 수 있다.
	@Override
	String getPosition() {
		//return super.getPosition();
		return "x:" + this.x + " y:" + this.y + " z:" +this.z;
	}
	
}

public class Ex04_inherit_override {
	public static void main(String[] args) {
		Point3D point = new Point3D();
		//String result = point.getPosition3();
		//System.out.println(result);
		
		String result = point.getPosition();
		System.out.println(result);
	}
}
~~~

~~~java
class Test2{
	void print() {
		System.out.println("부모함수 (Test2)");
	}
}

class Test3 extends Test2{
	@Override
	void print() {
		System.out.println("자식함수 (Test3) 개발자 마음대로");
	}
	//오버로딩(parameter 개수와 타입을 달리해서)
	void print(String s) {
		System.out.println("나는 오버로딩 함수~"+s);
	}
}

public class Ex05_inherit_override {
	public static void main(String[] args) {
		Test3 t = new Test3();
		t.print(); //재정의 된 함수
		t.print("오버로딩");
		String str = t.toString();
		System.out.println("재정의 하지 않은 toString() : " + str);
		System.out.println("toString() 함수가 default 호출 : " + t);
		//내부적으로 t.toString() 같은 결과 >> t 변수 출력하면
		
		Emp e = new Emp(1000, "홍길동");
		String str2 = e.toString();
		System.out.println(str2);
	}
}
~~~