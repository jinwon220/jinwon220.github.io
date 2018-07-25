---
title: NetWork(5)_TCP_Multi_Chatt
author: JinWon
layout: post
category: Java
---

# Multi_Chatt_Client
~~~java
public class Ex05_TCP_Multi_Chatt_Client extends JFrame implements ActionListener , Runnable {
	JTextArea ta;//출력창(다중라인)
	JTextField txtinput;//입력창
	DataInputStream in;
	DataOutputStream out;
	

	Ex05_TCP_Multi_Chatt_Client(){
		//UI 구성
		this.setTitle("Multi채팅");
		ta = new JTextArea();
		txtinput = new JTextField();
		JScrollPane sp =  new JScrollPane(ta,ScrollPaneConstants.VERTICAL_SCROLLBAR_ALWAYS,
				                          ScrollPaneConstants.HORIZONTAL_SCROLLBAR_NEVER);
		sp.setAutoscrolls(true);
		ta.setBackground(Color.YELLOW);
		ta.setLineWrap(true);//텍스트 자동 줄바꿈
		ta.setEditable(false);//편집 안되요
		
		txtinput.setText("이름 입력");
		txtinput.requestFocus();
		txtinput.selectAll();
		
		this.add(sp,"Center");
		this.add(txtinput, "South");
		this.setSize(400, 500);
		this.setVisible(true);
		
		this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		
		//TextField 이벤트 처리 (엔터)
		txtinput.addActionListener(this);
		
		//소켓 설정하기
		try {
			 Socket socket = new Socket("192.168.0.50", 9999);
			 in = new DataInputStream(socket.getInputStream());
			 out = new DataOutputStream(socket.getOutputStream());
			 //서버와 연결
			 ta.append("서버에 접속되었습니다\n\r");
			 
			 //Thread 구동
			 Thread client = new Thread(this);
			 client.start();
			 ////////////
		}catch(Exception e) {
			System.out.println(e.getMessage());
		}
		
	}
	
	@Override
	public void actionPerformed(ActionEvent e) {
		//TextField 입력하고 Enter 처리하면
		//txtinput.addAction...(this)
		if(e.getSource().equals(txtinput)) {
			String msg = txtinput.getText();
			try {
				out.writeUTF(msg); //서버로 출력
				//System.out.println("입력값 : "+msg);
				//ta.setText(msg);
				txtinput.setText("");
			}catch(Exception ex) {
				System.out.println(ex.getMessage());
			}
		}
		
		
	}
	
	@Override
	public void run() {
		try {
			String msg = in.readUTF();
			ta.append(msg + "\n\r");
			while(in != null) {
				msg = in.readUTF();
				ta.append(msg +"\n\r");
			}
		}catch (Exception e) {
			System.out.println("접속중 서버와 연결 종료");
			return;
		}
		
	}

	
	
	public static void main(String[] args) {
		Ex05_TCP_Multi_Chatt_Client client = new Ex05_TCP_Multi_Chatt_Client();

	}

	

}
~~~
# Multi_Chatt_Server

- 다중 채팅
- 서버 1개
- 여러명의 클라이언트 접속
- 클라이언트 1명( socket )
~~~java
public class Ex05_TCP_Multi_Chatt_Server {
	
	ServerSocket serversocket = null;
	Socket socket = null;
	//KEY POINT
	HashMap<String,DataOutputStream> ClientMap;
	
	Ex05_TCP_Multi_Chatt_Server(){
		//각 클라이언트 (socket 가지는 출력객체를 관리)
		ClientMap = new HashMap<>();
	}
	//1. 서버 초기화 작업
	public void init() {
		try {
			serversocket = new ServerSocket(9999);
			System.out.println("[서버시작 ... 응답대기 ...]");
			while(true) {
				
			socket = serversocket.accept(); //클라인언트들이 연결을 계속 할 수 있도록 while문 안으로
			
			System.out.println("[" + socket.getInetAddress() + "/" 
			                  + socket.getPort() + "]");
			//코드작업
			//클라이언트 접속시 마다 ....
				Thread client = new MultiServerRev(socket);
				client.start();
			//
			}	
			
			
		}catch (Exception e) {
			System.out.println("Init() : " + e.getMessage());
		}
	}
	
	//2. 접속된 모든 클라이언트에게 (soket) 메시지를 전달하는 함수
	//Map(key ,value)
	//key => 사용자ID (고유한 값) ex) kglim , hong , kim , lee ..
	//ClientMap<"hong" 각각의  socket객체에서 얻어낸 DataOutPutStream객체 저장>)
	//ClientMap<"kim"  각각의  socket객체에서 얻어낸 DataOutPutStream객체 저장>)
	void sendAllMsg(String msg) {
		Iterator<String> ClientKeySet = ClientMap.keySet().iterator();
		while(ClientKeySet.hasNext()) {
			try {
				DataOutputStream clientout = ClientMap.get(ClientKeySet.next());
				//각각의 접속한 client 에게 메시지 출력
				clientout.writeUTF(msg);
			}catch(Exception e) {
				System.out.println("Send AllMsg :" +  e.getMessage());
			}
		}
	}
	
	
	//3. 접속된 유저 리스트 목록 관리하기
	String showUserList(String name) {
		//문자열 누적 (String name ="김" ;  name+="박" ; name+="이"
		//대체 해서 : StringBuilder
		StringBuilder output = new StringBuilder("<<접속자 목록>>\n\r");
		Iterator<String> users = ClientMap.keySet().iterator();
		while(users.hasNext()) {
			try {
				String key = users.next(); //kim , park ,,,,,(접속한 ID)
				if(key.equals(name)) { //목록을 요청한 ID
					key+="(*)";
				}
				output.append(key+"\n\r");
				
			}catch(Exception e) {
				System.out.println("showUserList 예외 : " + e.getMessage());
			}
		}
		output.append("<<" + ClientMap.size() + ">>" + "명 접속중 \n\r");
		return output.toString();
	}

	
	//4. 궛속말 기능
	void SendToMsg(String fromname, String toname , String toMsg) {
		try {
			 ClientMap.get(toname).writeUTF("귓속말 from (" + fromname +")=>"+ toMsg);
			 ClientMap.get(fromname).writeUTF("귓속말 to (" + toname +")=>"+ toMsg);
		}catch (Exception e) {
			System.out.println("SendToMsg 예외 : " + e.getMessage());
		}
	}
	
	
	//Thread 사용 (보내기 , 받기)
	//inner class 형태
	class MultiServerRev extends Thread{
		Socket socket = null;
		DataInputStream in;
		DataOutputStream out;
		
		public MultiServerRev(Socket socket) {
			this.socket = socket;
			try {
				in = new DataInputStream(this.socket.getInputStream());
				out = new DataOutputStream(this.socket.getOutputStream());
			}catch(Exception e) {
				System.out.println("MultiServerRev 예외 :" + e.getMessage());
			}
		}
		
		@Override
		public void run() {
			//기본 클라이언트 : in.readUTF() , out.WriteUTF()
			String name=""; //클라이언트 이름 저장(key)
			try {
					//연결된 socket을 통해서 Client 메시지 전달
					out.writeUTF("이름을 입력하세요");
					name = in.readUTF();
					out.writeUTF("현재 채팅방에 입장하셨습니다");
				
					//서버에 접속된 모든 사용자에(socket) 입력된 내용 전달
					sendAllMsg(name +"님 입장^^");
					
					//KEY POINT
					ClientMap.put(name, out); //Map(이름, 각각 socket 출력객체)
					System.out.println("서버 모니터링 : 현재 접속자는 수 " + ClientMap.size() +"명");
					
					//추가기능(대화)
					while(in != null) {
						String msg = in.readUTF();
						
						//채팅장에 참여하고 있는 접속자에게 전달
						if(msg.startsWith("/")) {
							if(msg.trim().equals("/접속자")) {
								out.writeUTF(showUserList(name)); //접속자 목록 출력
							}else if(msg.startsWith("/귓속말")) {
								String[] msgArr = msg.split(" ",3); // /귓속말 홍길동 방가방가
								if(msgArr == null || msgArr.length < 3) {
									out.writeUTF("HELP:사용법\n\r /귓속말 [상대방이름] [메시지]");
								}else {
									String toName = msgArr[1];
									String toMsg  = msgArr[2];
									if(ClientMap.containsKey(toName)) {
										//메시지 보내기
										//특정 상대에게 메시지 보내기
										SendToMsg(name, toName, toMsg);
									}else {
										out.writeUTF("입력한 사용자가 없습니다");
									}
								}
							}else {
								out.writeUTF("잘못된 명령어 입니다");
							}
						}else {
							//전체 사용자에게 write
							sendAllMsg("[" + name + "]" + msg);
						}
					}//while end
					
			}catch (Exception e) {
				System.out.println("Thread run 예외 :" + e.getMessage());
			}finally {
				//Client 종료 (퇴장)
				ClientMap.remove(name);
				sendAllMsg(name + "님 퇴장하였습니다");
				System.out.println("서버 모니터링 : 현재 접속자는 " + ClientMap.size() +"명" );
			}
		}
	}
	
	
	
	public static void main(String[] args) {
		Ex05_TCP_Multi_Chatt_Server server = new Ex05_TCP_Multi_Chatt_Server();
		server.init();

	}

}
~~~