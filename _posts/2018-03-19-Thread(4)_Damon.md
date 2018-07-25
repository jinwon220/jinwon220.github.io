---
title: Thread(4)_Damon
author: JinWon
layout: post
category: Java
---

# Damon

- 한글 작업 (주 작업) ... 보조적으로 (저장) >> Damon ...
- Damon 은  (주 Thread) 와 생명을 같이 함 (주 Thread가 끝나면 Damon도 종료)
~~~java
public class Ex07_Damon_Thread implements Runnable{
	static boolean autosave = false;
	
	public static void main(String[] args) {
		//Ex07_Damon_Thread ex = new Ex07_Damon_Thread();
		//Thread th = new Thread(ex);
		Thread th = new Thread(new Ex07_Damon_Thread());
		th.setDaemon(true);
		th.start();
		//main 도 하나의 Thread (non-damon)
		//main 의 보조는 th (th는 main의 damon쓰레드)
		//th는 main과 운명을 같이한다.
		for(int i = 0; i <= 30; i++) {
			try {
				Thread.sleep(1000);
			} catch (Exception e) {
				System.out.println(e.getMessage());
			}
			System.out.println("Main Thread : " + i);
			if(i == 5) {
				System.out.println(i);
				autosave = true;
			}
		}
	}

	@Override
	public void run() {
		while(true) {
			try {
				Thread.sleep(3000);
			} catch (Exception e) {
				
			}
			if(autosave) { //true라면
				autoSave();
			}
		}
	}
	public void autoSave() {
		System.out.println("문서가 3초 간격으로 자동 저장 되었습니다.");
	}

}
~~~