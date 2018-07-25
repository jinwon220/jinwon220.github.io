---
title: NetWork(3)_TCP_Echo
author: JinWon
layout: post
category: Java
---

# Echo_Client
~~~java
public class Ex03_TCP_Echo_Client {

	public static void main(String[] args) throws Exception, IOException {
		Socket socket = new Socket("192.168.0.25", 9999);
		System.out.println("서버와 연결 되었습니다.");
		
		//read
		DataInputStream dis = new DataInputStream(socket.getInputStream());
		
		//write
		DataOutputStream dos = new DataOutputStream(socket.getOutputStream());
		
		Scanner sc = new Scanner(System.in);
		
		while(true) {
			System.out.print("서버에 전솔될 내용을 입력 : ");
			String msg = sc.nextLine();
			
			//서버 전송
			dos.writeUTF(msg);
			dos.flush();
			
			if(msg.equals("exit"))break;
			
			//서버로부터
			String servermsg = dis.readUTF();
			System.out.println("Echo 메시지 : " + servermsg);
		}
		System.out.println("클라이언트 종료");
		dis.close();
		dos.close();
		socket.close();
		
	}

}
~~~

# Echo_Server

~~~java
public class Ex03_TCP_Echo_Server {
	public static void main(String[] args) throws IOException{
		ServerSocket serverSocket = new ServerSocket(9999);
		System.out.println("접속 대기중...");
		Socket socket = serverSocket.accept(); //클라이언트가 접속하면...
		System.out.println("연결완료");
		
		//Client write 데이터 서버가 받아서 다시 Client write
		//server >> read, write
		//client >> write, read
		
		//socket 가지고 있는 input , output Stream 을 사용
		
		//read
		InputStream in = socket.getInputStream();
		DataInputStream dis = new DataInputStream(in);
		
		//write
		OutputStream out = socket.getOutputStream();
		DataOutputStream dos = new DataOutputStream(out);
		
		while(true) {
			//read
			//client write 한 데이터가 있다면
			String clientmsg = dis.readUTF();//client가  write 하면 동작
			System.out.println("Client Msg : " + clientmsg);
			
			if(clientmsg.equals("exit"))break; //client "exit" 서버 종료
			
			//메아리 기능
			dos.writeUTF(clientmsg);
			dos.flush(); //close() 있어도 되지만 .... 지금 바로 내보내고 싶기 때문에 flush();바로 출력
		}
		System.out.println("클라이언트 종료 요청(exit) 서버 종료");
		
		dis.close();
		dos.close();
		out.close();
		in.close();
		socket.close();
		serverSocket.close();
	}

}

~~~