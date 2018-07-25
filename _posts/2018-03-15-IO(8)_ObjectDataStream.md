---
title: IO(8)_ObjectDataStream
author: JinWon
layout: post
category: Java
---

# ObjectDataOutPutStream
- 객체를 파일에 write
~~~java
public class Ex15_ObjectDataOutPutStream {
	public static void main(String[] args) {
		String filename = "UserData.txt";
		try {
			FileOutputStream fos = new FileOutputStream(filename);
			BufferedOutputStream bos = new BufferedOutputStream(fos);
			
			//객체(직렬화)
			ObjectOutputStream out = new ObjectOutputStream(bos);
			//
			UserInfo u1 = new UserInfo("superman", "super", 500);
			UserInfo u2 = new UserInfo("scott", "tiger", 50);
			
			//직렬화
			out.writeObject(u1);
			out.writeObject(u2);
			//
			
			out.close();
			bos.close();
			fos.close();
			System.out.println("파일생성 -> buffer -> 직렬화객체 -> write");
		}catch(Exception e) {
			System.out.println(e.getMessage());
		}
	}

}
~~~

# ObjectDataInPutStream
- UserData.txt 파일에서 질렬화된 객체를 read (역직렬화) 다시 조립
~~~java
public class Ex16_ObjectDataInPutStream {
	public static void main(String[] args) {
		String filename = "UserData.txt";
		
		FileInputStream fis = null;
		BufferedInputStream bis = null;
		ObjectInputStream in = null;
		
		try {
			fis = new FileInputStream(filename);
			bis = new BufferedInputStream(fis);
			in = new ObjectInputStream(bis);
			
			//역직렬화 readObject()의 return type은 Object(다운캐스팅해주자!!)
			//UserInfo r1 = (UserInfo)in.readObject(); //역직렬화 : 복원
			//UserInfo r2 = (UserInfo)in.readObject(); //역직렬화 : 복원
			
			//System.out.println(r1.toString());
			//System.out.println(r2.toString());
			
			//객체 단위 read 객체가 없으면 null 값 반환
			Object users = null;
			int count = 0;
			while((users = in.readObject()) != null) {
				System.out.println(users);
				System.out.println(++count);
			}
		} catch (Exception e) {
			
		} finally {
			try {
				in.close();
				bis.close();
				fis.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}

}
~~~