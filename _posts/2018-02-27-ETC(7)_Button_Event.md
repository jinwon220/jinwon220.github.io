---
title: ETC(7)_Button_Event
author: JinWon
layout: post
category: Java
---

# Button_Event

~~~java
class Btn_Handler implements ActionListener{
	private TextField txtid;
	private TextField txtpw;
	
	public Btn_Handler(TextField txtid, TextField txtpw) {
		this.txtid = txtid; //주소값(textfield)
		this.txtpw = txtpw;
	}
	
	//버튼을 클릭하면 actionPerformed호출
	@Override
	public void actionPerformed(ActionEvent e) {
		System.out.println("ActionEvent 발생지 객체의 주소 : " + e.getSource());
		if(txtid.getText().equals("hong")) {
			System.out.println("방가방가 : " + txtpw.getText());
		}else {
			System.out.println("hong을 입력해");
		}
	}
}

class LoginForm extends Frame{
	Label lbl_id;
	Label lbl_pw;
	TextField txt_id;
	TextField txt_pw;
	Button btn_ok;
	
	public LoginForm(String title) {
		super(title);
		lbl_id = new Label("ID : ", Label.RIGHT);
		lbl_pw = new Label("PW : ", Label.RIGHT);
		
		txt_id = new TextField(10);
		txt_pw = new TextField(10);
		txt_pw.setEchoChar('*');
		btn_ok = new Button("Login");
		
		this.setLayout(new FlowLayout()); //순서 (add)
		this.setSize(500, 100);
		this.setVisible(true);
		
		this.add(lbl_id);
		this.add(txt_id);
		
		this.add(lbl_pw);
		this.add(txt_pw);
		
		this.add(btn_ok);
		btn_ok.addActionListener(new Btn_Handler(txt_id, txt_pw));
	}
}

public class Ex12_Button_Event {

	public static void main(String[] args) {
		LoginForm login = new LoginForm("로그인");
	}

}
~~~

# Button_Event_InnerClass

~~~java
class LoginForm2 extends Frame{
	Label lbl_id;
	Label lbl_pw;
	TextField txt_id;
	TextField txt_pw;
	Button btn_ok;
	
	public LoginForm2(String title) {
		super(title);
		lbl_id = new Label("ID : ", Label.RIGHT);
		lbl_pw = new Label("PW : ", Label.RIGHT);
		
		txt_id = new TextField(10);
		txt_pw = new TextField(10);
		txt_pw.setEchoChar('*');
		btn_ok = new Button("Login");
		
		this.setLayout(new FlowLayout()); //순서 (add)
		this.setSize(500, 100);
		this.setVisible(true);
		
		this.add(lbl_id);
		this.add(txt_id);
		
		this.add(lbl_pw);
		this.add(txt_pw);
		
		this.add(btn_ok);
		
		class Btn_Handler2 implements ActionListener{
			//버튼을 클릭하면 actionPerformed호출
			@Override
			public void actionPerformed(ActionEvent e) {
				System.out.println("ActionEvent 발생지 객체의 주소 : " + e.getSource());
				if(txt_id.getText().trim().equals("hong")) {
					System.out.println("방가방가 : " + txt_pw.getText().trim());
				}else {
					System.out.println("hong을 입력해");
				}
			}
		}
		
		btn_ok.addActionListener(new Btn_Handler2());
		
		//단점 쓰지 않는것도 다 생성된다.
		//1회성 : windowListener 인터페이스 구현(구현하는 클래스가 없는 익명타입)
		this.addWindowListener(new WindowListener() {
			
			@Override
			public void windowOpened(WindowEvent e) {
				// TODO Auto-generated method stub
				
			}
			
			@Override
			public void windowIconified(WindowEvent e) {
				// TODO Auto-generated method stub
				
			}
			
			@Override
			public void windowDeiconified(WindowEvent e) {
				// TODO Auto-generated method stub
				
			}
			
			@Override
			public void windowDeactivated(WindowEvent e) {
				// TODO Auto-generated method stub
				
			}
			
			@Override
			public void windowClosing(WindowEvent e) {
				//창닫기...
				e.getWindow().setVisible(false);
				e.getWindow().dispose();
			}
			
			@Override
			public void windowClosed(WindowEvent e) {
				// TODO Auto-generated method stub
				
			}
			
			@Override
			public void windowActivated(WindowEvent e) {
				// TODO Auto-generated method stub
				
			}
		});
	}
}

public class Ex13_Button_Event_InnerClass {
	public static void main(String[] args) {
		LoginForm2 login = new LoginForm2("로그인");
	}

}
~~~

# Button_Event_Final
~~~java
class LoginForm3 extends Frame{
	Label lbl_id;
	Label lbl_pw;
	TextField txt_id;
	TextField txt_pw;
	Button btn_ok;
	
	public LoginForm3(String title) {
		super(title);
		lbl_id = new Label("ID : ", Label.RIGHT);
		lbl_pw = new Label("PW : ", Label.RIGHT);
		
		txt_id = new TextField(10);
		txt_pw = new TextField(10);
		txt_pw.setEchoChar('*');
		btn_ok = new Button("Login");
		
		this.setLayout(new FlowLayout()); //순서 (add)
		this.setSize(500, 100);
		this.setVisible(true);
		
		this.add(lbl_id);
		this.add(txt_id);
		
		this.add(lbl_pw);
		this.add(txt_pw);
		
		this.add(btn_ok);
		
		//최종
		btn_ok.addActionListener(new ActionListener() {
			
			@Override
			public void actionPerformed(ActionEvent e) {
				System.out.println("ActionEvent 발생지 객체의 주소 : " + e.getSource());
				if(txt_id.getText().trim().equals("hong")) {
					System.out.println("방가방가 : " + txt_pw.getText().trim());
				}else {
					System.out.println("hong을 입력해");
				}
			}
		});
		/*
		class Btn_Handler3 implements ActionListener{
			//버튼을 클릭하면 actionPerformed호출
			@Override
			public void actionPerformed(ActionEvent e) {
				System.out.println("ActionEvent 발생지 객체의 주소 : " + e.getSource());
				if(txt_id.getText().trim().equals("hong")) {
					System.out.println("방가방가 : " + txt_pw.getText().trim());
				}else {
					System.out.println("hong을 입력해");
				}
			}
		}
		*/
		//btn_ok.addActionListener(new Btn_Handler3());
		
		this.addWindowListener(new WindowAdapter() {

			@Override
			public void windowClosing(WindowEvent e) {
				e.getWindow().setVisible(false);
				e.getWindow().dispose();
			}
			
		});
		
		/*
		//1회성 : windowListener 인터페이스 구현(구현하는 클래스가 없는 익명타입)
		//결국 인터페이스를 구현하기 때문에 사용하지도 않는 함수도 구현하게 된다.
		
		this.addWindowListener(new WindowListener() {
			
			@Override
			public void windowOpened(WindowEvent e) {
				// TODO Auto-generated method stub
				
			}
			
			@Override
			public void windowIconified(WindowEvent e) {
				// TODO Auto-generated method stub
				
			}
			
			@Override
			public void windowDeiconified(WindowEvent e) {
				// TODO Auto-generated method stub
				
			}
			
			@Override
			public void windowDeactivated(WindowEvent e) {
				// TODO Auto-generated method stub
				
			}
			
			@Override
			public void windowClosing(WindowEvent e) {
				//창닫기...
				e.getWindow().setVisible(false);
				e.getWindow().dispose();
			}
			
			@Override
			public void windowClosed(WindowEvent e) {
				// TODO Auto-generated method stub
				
			}
			
			@Override
			public void windowActivated(WindowEvent e) {
				// TODO Auto-generated method stub
				
			}
		});
		*/
	}
}

public class Ex14_Button_Event_Final {
	public static void main(String[] args) {
		LoginForm3 login = new LoginForm3("로그인");
	}

}
~~~