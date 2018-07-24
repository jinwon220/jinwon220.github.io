---
title: ETC(1)_Exception
author: JinWon
layout: post
category: Java
---

# Exception

### 오류
1. 에러(error) : 네트워크 장애, 메모리, 하드웨어
2. 예외(Exception) : 개발자 코드 처리 (로직 제어 ...> 예측가능)

> 예외처리 목적 : 프로그램을 정상적으로 수정하는 것이 아니고 문제가 발생시 비정상적으로 종료 못하게 하는 것

~~~
문제가 발생될 수 있는 코드
try{
	문제가 될 수 있는 코드
}catch(Exception e){
	처리(문제를 인지를 하고...)
	관리자에게 메일 보낼까?
	로그 파일에 기록 할까?
}finally{
	예외가 발생하던, 발생하지 않던 (의무적)강제적으로 수행되는 구문
}
~~~

~~~java
public class Ex01_Exception {
	public static void main(String[] args) {
		/*
		System.out.println("Main Start");
		System.out.println("Main Logic 처리");
		System.out.println(0/0); // 비정상 종료(문제 발생 시점부터 그 이하 코드 실행 안되요)
		System.out.println("Main End");
		*/
		System.out.println("Main Start");
		System.out.println("Main Logic 처리");
		try {
			System.out.println(0/0); // 비정상 종료(문제 발생 시점부터 그 이하 코드 실행 안되요)
		}catch (Exception e){
			//처리하는 코드
			//System.out.println(e.getMessage());
			e.printStackTrace();
		}
		System.out.println("Main End");
	}
}
~~~

~~~java
public class Ex02_Exception {
	public static void main(String[] args) {
		int num = 100;
		int result = 0;
		/*
		try {
			for (int i = 0; i < 10; i++) {
				result = num / (int) (Math.random() * 10);// 난수 (0~9)
				System.out.println("result : " + result + " i:" + i);
			}
		} catch (Exception e) { //안좋은 방법..(가독성 DOWN)
			System.out.println("Exception...");
		}
		System.out.println("MainEND");
		*/
		/*
		try {
			for (int i = 0; i < 10; i++) {
				result = num / (int) (Math.random() * 10);// 난수 (0~9)
				System.out.println("result : " + result + " i:" + i);
			}
		} catch (Exception e) { 
			System.out.println("Exception...");
		} catch (ArithmeticException e) { //오류 Exception 에러가 모든 에러를 가져가기 때문에 할일이 없어 오류가 난다
			System.out.println("연산 예외 발생");
		}
		System.out.println("MainEND");
		*/
		
		try {
			for (int i = 0; i < 10; i++) {
				result = num / (int) (Math.random() * 10);// 난수 (0~9)
				System.out.println("result : " + result + " i:" + i);
			}
		} catch (ArithmeticException e) {
				System.out.println("연산 예외 발생");
		} catch (Exception e) {
			System.out.println("Exception...");
		}
		System.out.println("MainEND");
		
		//연산에 관련된 예외는 ArithmeticException 잡고 나머지 Exception 처리
		//하위 예외는 상위(부모) 앞에....
	}
}
~~~

# Exception_finally
~~~java
public class Ex03_Exception_finally {
	static void startInstall() {
		//System.out.println("INSTALL");
	}

	static void copyFiles() {
		System.out.println("COPY FILES");
	}

	static void fileDelete() {
		System.out.println("DELETE FILES");
	}

	public static void main(String[] args) {
		try {
			copyFiles();
			startInstall(); // 설치 중단 되거나, 설치 완료 되거나 -> DISK 설치 파일 삭제를 원함
			
			//개발자(사용자)가 강제로 예외를 처리할 수 있습니다
			//사용자 예외 던지기 (예외 객체를 개발자가 직접 생성하고 new해라)
			//IOException io = new IOException("Install 처리 중 문제 발생");
			//throw io; // catch 가 처리
			throw new IOException("Install 처리 중 문제 발생");
		} catch (Exception e) {
			System.out.println("예외 메시지 출력하기  : "+e.getMessage());
		} finally { // 예외가 발생하거나, 발생 안하거나 강제적(의무적)으로 실행되는 블럭;
			fileDelete();
		}
		System.out.println("실행");
		//주의사항
		//*****함수 종료(return;)있어도 finally 블럭이 있으면 {실행}하고   종료 *****
	}
	
}
~~~

# Exception_throws
~~~java
public class Ex04_Exception_throws {

	public static void main(String[] args) {
		
		try {
			ExClass ex = new ExClass();
			ex.call();
		} catch (Exception e) {
			e.printStackTrace();
		}
		//클래스 설계시 내가 가지고 있는 자원을 사용하는 개발자에게 강제로 예외처리를 하도록 하는 방법
		//생성자, 함수뒤에 [throws 예외클래스명, 예외클래스명, 예외클래스명]
		
		//JAVA API 제공 클래스 들은 throws 를 가지고 있다. (IO)
		//public FileInputStream(String name)
        //throws FileNotFoundException
		/*
		try {
			FileInputStream fi = new FileInputStream("C:\\temp\\a.txt");
		}catch(FileNotFoundException e) {
			e.printStackTrace();
		}
		*/
	}

}
~~~