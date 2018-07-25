---
title: NetWork(4)_TCP_Chatt
author: JinWon
layout: post
category: Java
---

# Chatt_Client
- Inner Class 사용해서 구현
- ClientSend
- ClinentReceive
~~~java
public class Ex04_TCP_Chatt_Client {
	Socket socket = null;
	public static void main(String[] args) {
		Ex04_TCP_Chatt_Client client = new Ex04_TCP_Chatt_Client();
	}

	Ex04_TCP_Chatt_Client(){
		try {
			socket = new Socket("192.168.0.45", 9999);
			System.out.println("서버와 연결 되었습니다.");
			new ClientSend().start();
			new ClinentReceive().start();
		}catch (Exception e) {
			
		}
	}
	class ClientSend extends Thread{

		@Override
		public void run() {
			BufferedReader br = null;
			PrintWriter pw = null;
			try {
				//buffer >> Line() 단위로 read
				//InputStreamReader(System.in) 스캐너 역할도 같이 한다
				br = new BufferedReader(new InputStreamReader(System.in));
				pw = new PrintWriter(socket.getOutputStream(), true);
				
				while(true) {
					String data = br.readLine();
					if(data.equals("exit")) break;
					pw.println(data); //접속한 클라이언트에게 메시지 보내기
				}
				System.out.println("server send 작업종료");
			} catch (Exception e) {
				System.out.println(e.getMessage());
			} finally {
				try {
					pw.close();
					br.close();
				} catch (IOException e) {
					System.out.println(e.getMessage());
				}
			}
			
		}
	}
	
	class ClinentReceive extends Thread{
		//연결된 socket 을 통해서 read 해서
		//cmd 결과를 출력
		@Override
		public void run() {
			BufferedReader br = null;
			//PrintWriter pw = null;
			try {
				//buffer >> Line() 단위로 read
				//InputStreamReader(System.in) 스캐너 역할도 같이 한다
				
				//br = new BufferedReader(new InputStreamReader(System.in));
				//pw = new PrintWriter(Ex04_TCP_Chatt_Server.socket.getOutputStream(), true);
				br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
				
				String data = null;
				while((data = br.readLine()) != null) {
					System.out.println("Server가 보낸 메시지 [ " + data + " ]");
				}
				System.out.println("server receive 작업종료");
			} catch (Exception e) {
				System.out.println(e.getMessage());
			} finally {
				try {
					br.close();
				} catch (IOException e) {
					System.out.println(e.getMessage());
				}
			}
		}
	}
}

~~~

# Chatt_Server

### Echo : 순차적인 데이터 처리 (Ex01 ~ Ex03) 
--------------------
- Server : read, write
- Client : read, write
- 동시에 (Thread)....
- read (Thread)
- write (Thread)

~~~java
//1:1 채팅은 입력과 출력 동시에 이루어짐 
public class Ex04_TCP_Chatt_Server {
	static ServerSocket serverSocket = null;
	static Socket socket = null;
	
	public static void main(String[] args) {

		try {
			serverSocket = new ServerSocket(9999);
			System.out.println("접속 대기중...");
			socket = serverSocket.accept(); //클라이언트가 접속하면...
			System.out.println("연결완료");
			
			//ServerSend 객체나
			//ServerReceive 객체나
			//socket 객체의 주소가 필요
			//why : socket을 통해서 read, write를 해야 하기 때문에
			
			//ServerSend send = new ServerSend();
			//send.start();
			new ServerSend().start();
			
			//ServerReceive receive = new ServerReceive();
			//receive.start();
			new ServerReceive().start();
			
		} catch (Exception e) {
			System.out.println(e.getMessage());
		} finally {
			try {
				serverSocket.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}

}

//고민 : socket (read, write 객체)
//Write : socket > outputStream
class ServerSend extends Thread{

	@Override
	public void run() {
		BufferedReader br = null;
		PrintWriter pw = null;
		try {
			//buffer >> Line() 단위로 read
			//InputStreamReader(System.in) 스캐너 역할도 같이 한다
			br = new BufferedReader(new InputStreamReader(System.in));
			pw = new PrintWriter(Ex04_TCP_Chatt_Server.socket.getOutputStream(), true);
			
			while(true) {
				String data = br.readLine();
				if(data.equals("exit")) break;
				pw.println(data); //접속한 클라이언트에게 메시지 보내기
			}
			System.out.println("server send 작업종료");
		} catch (Exception e) {
			System.out.println(e.getMessage());
		} finally {
			try {
				pw.close();
				br.close();
			} catch (IOException e) {
				System.out.println(e.getMessage());
			}
		}
		
	}
}

//Read : socket > inputStream
class ServerReceive extends Thread{
	//연결된 socket 을 통해서 read 해서
	//cmd 결과를 출력
	@Override
	public void run() {
		BufferedReader br = null;
		//PrintWriter pw = null;
		try {
			//buffer >> Line() 단위로 read
			//InputStreamReader(System.in) 스캐너 역할도 같이 한다
			
			//br = new BufferedReader(new InputStreamReader(System.in));
			//pw = new PrintWriter(Ex04_TCP_Chatt_Server.socket.getOutputStream(), true);
			br = new BufferedReader(new InputStreamReader(Ex04_TCP_Chatt_Server.socket.getInputStream()));
			
			String data = null;
			while((data = br.readLine()) != null) {
				System.out.println("Client가 보낸 메시지 [ " + data + " ]");
			}
			System.out.println("server receive 작업종료");
		} catch (Exception e) {
			System.out.println(e.getMessage());
		} finally {
			try {
				br.close();
			} catch (IOException e) {
				System.out.println(e.getMessage());
			}
		}
	}
}
~~~