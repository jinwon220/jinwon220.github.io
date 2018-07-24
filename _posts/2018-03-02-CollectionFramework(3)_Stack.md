---
title: CollectionFramework(3)_Stack
author: JinWon
layout: post
category: Java
---

# Stack

~~~java
public class Ex05_Stack_Collection {
	public static void main(String[] args) {
		// LIFO
		// Stack stack = new Stack();
		// stack.push("A");
		// stack.push("B");
		// System.out.println(stack.pop());
		//
		// System.out.println(stack.pop());
		// System.out.println(stack.pop()); 에러 더이상 pop할게 없음

		Mystack my = new Mystack(1);
		int top = my.top();
		System.out.println(top);
		my.push(1);
		top = my.top();
		///////////////////////////////////
		
		// JAVA API 제공하느 Stack 클래스
		Stack<Coin> coinBox = new Stack<Coin>();

		// 동전케이스에 동전을 끼움
		// 100원, 50원, 500원, 10원 순서로 넣는다.
		coinBox.push(new Coin(100));
		coinBox.push(new Coin(50));
		coinBox.push(new Coin(500));
		coinBox.push(new Coin(10));

		while (!coinBox.isEmpty()) {
			Coin coin = coinBox.pop();
			System.out.println("꺼내온 동전 : " + coin.getValue() + "원");
		}
	}
}
~~~