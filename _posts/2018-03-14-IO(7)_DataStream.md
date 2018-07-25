---
title: IO(7)_DataStream
author: JinWon
layout: post
category: Java
---

# DataOutPutStream
- 보조 스트림
- Java API : 8기본 타입 (타입별로 read ,write) 클래스
- DataOutPutStream , DataInputStream 
- 데이터 작업 ,채팅 ...

~~~java
public class Ex13_DataOutPutStream {
	public static void main(String[] args) {
		int[] score = {100,60,55,94,12};
		
		try {
				FileOutputStream fos = new FileOutputStream("score.txt");
				DataOutputStream dos = new DataOutputStream(fos);
				for(int i = 0 ; i < score.length ;i++) {
					dos.writeInt(score[i]); //정수값 write > read(DataInputStream)
					//dos.wirteUTF(str);
				}
				dos.close();
				fos.close();
		}catch (Exception e) {
			// TODO: handle exception
		}
	}

}
~~~

# DataInPutStream

~~~java
public class Ex14_DataInputStream {

	public static void main(String[] args) {
		int sum=0;
		int score=0;
		FileInputStream fis=null;
		DataInputStream dis=null;
		try {
				fis =  new FileInputStream("score.txt");
				dis = new DataInputStream(fis);
				while(true) {
					score = dis.readInt();
					System.out.println("Score int 타입 :" + score);
					sum+=score;
				}
		}catch (Exception e) {
			System.out.println("예외발생 : " + e.getMessage());
			System.out.println("sum결과 : " + sum);
		}finally {
			try {
					dis.close();
					fis.close();
					
			}catch (Exception e) {
				
			}
		}

	}

}
~~~