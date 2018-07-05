---
title: Ex01_Basic-Staterment
author: JinWon
layout: post
category: Java
---

# 반복문 (while, do~while)

> while 과의 차이점은 <br> do~while() : 강제적 실행 구문(무조건 한번은 실행)

~~~java
import java.util.Scanner;

public class Ex10_Statement {

	public static void main(String[] args) {
		int i = 10;
		while(i>=10) { //조건 true 블럭안의 구문 실행
			System.out.println(i); //10
			i--; //반드시 명시적인 증가감
			System.out.println(i); //9
		}
		
		//1~100까지의 합
		int sum=0;
		int j = 1;
		while(j<=100) {
			//j++; (주의사항^^ 만약 j를 0으로 했으면 여기가 정답)
			sum += j;
			j++; // 명시적인 증가감
		}
		System.out.println("1~100까지의 합 : " + sum);
		
		//while 사용해서 구구단
		
		int k = 2;
		int p = 1;
		
		while (k<=9) {
			p = 1;
			while(p<=9) {
				System.out.printf("[%d]*[%d]=[%d]\t", k, p, k*p);
				p++;
			}
			k++;
		}
		System.out.println();
		
		//do{실행문} while(조건식)
		//메뉴 구성
		//1. 인사메뉴
		//2. 급여메뉴
		
		Scanner sc = new Scanner(System.in);
		int inputdata =0;
		do {
			System.out.println("숫자를 입력하시오(0~10)");
			inputdata = Integer.parseInt(sc.nextLine()); // 문자를 숫자로
		}while(0 >= inputdata || inputdata > 10);
		// 10보다 큰 값이 오면 true 다시 do문을 실행시키겠다.
		// 0~10까지의 값이 입력되면 false -> do~while문 탈출
		System.out.println("당신이 입력한 숫자는 : " + inputdata);
		
	}

}

~~~