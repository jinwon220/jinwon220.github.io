---
title: ETC(6)_AWT_Frame
author: JinWon
layout: post
category: Java
---

# AWT_Frame

메모장, 계산기 만들 수 있도록  java 지원하는 자원 <br>
AWT, SWING

~~~java
class MyFrame extends Frame{
	public MyFrame(String title) {
		super(title);
	}
}

//이벤트 구현 (이벤트 핸들러 클래스)
class BtnClick_Handler implements ActionListener{

	@Override
	public void actionPerformed(ActionEvent e) {
		//코드 마음대루루루루루루루루루루
		//개발자 마음대루루루루루루루루루
		System.out.println("원하는 행위");
	}
	
}
public class Ex11_AWT_Frame {
	public static void main(String[] args) {
		MyFrame my = new MyFrame("Loing");
		my.setSize(350, 300);
		my.setVisible(true);
		my.setLayout(new FlowLayout());
		
		Button btn = new Button("나 버튼");
		Button btn2 = new Button("two button");
		Button btn3 = new Button("three button");
		
		//버튼을 클릭하면  >> 행위
		//메서드를 찾는다 : actionPerformed (button 을 클릭하면 호출 되는 함수) API
		//ActionListener 인터페이스가 actionPerformed 를 가지고 있다.
		//이벤트 핸들러를 만든다 : ActionListener 를 구현하는 클래스
		//actionPerformed 함수에 대한 구현 >> override
		//Buttom 붙이기 (Listener사용)

		
		//익명클래스 일회성 코드
		btn.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				System.out.println("인터페이스 익명타입으로 직접구현");
			}
		});
		
		BtnClick_Handler handler = new BtnClick_Handler();
		
		//클래스 이벤트 핸들러를 만들면 (공통기능) >> 재사용 가능
		btn2.addActionListener(handler);
		btn3.addActionListener(handler);
		
		my.add(btn);
		my.add(btn2);
		my.add(btn3);
		
	}

}
~~~