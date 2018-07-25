---
title: NetWork(2)_TCP
author: JinWon
layout: post
category: Java
---

# Client
- server IP : 192.168.0.25
- port : 9999
~~~java
public class Ex02_TCP_Client {
	public static void main(String[] args) throws Exception, IOException {
		Socket socket = new Socket("192.168.0.25", 9999);
		System.out.println("서버와 연결 되었습니다.");
		
		//서버에서 보낸 메세지 받기
		InputStream in = socket.getInputStream();
		DataInputStream dis = new DataInputStream(in);
		
		String servermsg = dis.readUTF();
		System.out.println("서버에서 보낸 메시지 \n" + servermsg);
		
		dis.close();
		in.close();
		socket.close();
	}

}
~~~

# Server
~~~
TCP 서버
server(process) - client(process)
~~~
- 서버 : 192.168.0.25
- 포트 : port : 9999
~~~java
public class Ex02_TCP_Server {
	public static void main(String[] args) throws IOException {
		ServerSocket serverSocket = new ServerSocket(9999);
		System.out.println("접속 대기중...");
		Socket socket = serverSocket.accept(); //클라이언트가 접속하면...
		System.out.println("연결완료");
		
		//열결이 되면
		//서버 : 클라이언트 (read, write)
		//socket 과 socket 끼리 연결
		//socket(input, output Stream) 을 가지고 있다.
		
		OutputStream out = socket.getOutputStream();
		DataOutputStream dos = new DataOutputStream(out); //8가지 기본 타입 + 알파
		dos.writeUTF("문자데이터Byte 통신 : 연습");
		
		System.out.println("서버 종료");
		
		dos.close();
		out.close();
		socket.close();
		serverSocket.close();
		
	}

}
~~~