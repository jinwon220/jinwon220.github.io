---
title: IO(4)_File_Formet
author: JinWon
layout: post
category: Java
---

# File_Formet

~~~java
public class Ex09_File_Formet {
	public static void main(String[] args) {
		/*
		Format 클래스
		
		화폐단위 처리
		DecimalFormat df = new DecimalFormat("#,###.0");
		String result = df.format(1234567.89);
		System.out.println(result);
		
		날짜형식 처리
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy년 MM월 dd일");
		String strDate = sdf.format(new Date());
		System.out.println(strDate);
		*/
		
		File dir = new File("C:\\Temp");
		File[] files = dir.listFiles(); //모든자원(폴더 또는 파일)
		
		for(int i = 0; i < files.length; i++) {
			File file = files[i];
			String name = file.getName(); //폴더명, 파일명
			
			SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH-mma");
			String attribute = "";
			String size = "";
			
			if(files[i].isDirectory()) {
				attribute="<DIR>";
			}else { //나머지는 파일(a.txt , copy.jpg ...)
				size = file.length() + "byte";
				attribute=file.canRead()?"R":"";
				attribute+=file.canWrite()?"W":"";
				attribute+=file.isHidden()?"H":"";
			}
			System.out.printf("%s  %3s  %10s  %s \n",
							df.format(new Date(file.lastModified()))
							,attribute
							,size
							,name);
		}
	}

}
~~~