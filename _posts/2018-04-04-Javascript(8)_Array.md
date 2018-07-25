---
title: Javascript(8)_Array
author: JinWon
layout: post
category: JavaScript
---

# JavaScript_Array

~~~javascript
//while
var i = 1;
var sum = 0;
while(i <= 10){
    sum+=i;
    i++;
}
document.write("sum : " + sum + "<br>");

var sum2 = 0;
for(var i = 0; i<=10; i++){
    if(i%3 == 0){
        console.log(i);
        console.log(0%3);
        continue; //3의 배수는 누적하지 않겠다.
        //break;
    }
sum2+=i;
}
document.write("sum2 : " + sum2 + "<br>");

var a = 1;
do{
    document.write(a + "<br>");
    a++;
}while(a<11);
//////////////////////////////////////////////////
//배열 (암기) : POINT
/*
JAVA
int[] arr = new int[10];
int[] arr2 = new int[]{10,20,30};
int[] arr3 = {10,20,30};
*/

var arr = [10,20,30]; //int[] arr3 = {10,20,30};
for(var i = 0; i<3; i++){
    document.write(arr[i] + "<br>");
}

//javascript 배열 생성 기본 방법
var eng = new Array(3); //int[] arr = new int[3];
eng[0] = 100;
eng[1] = 200;
eng[2] = 300;
for(var i=0; i<eng.length; i++){
    document.write(eng[i] + "<br>");
}

//javascript 배열 기본 형식(Array())
//1. int[] arr = new int[10];
var array = new Array(10);

//2. int[] arr2 = new int[]{10,20,30};
var array2 = new Array(10,20,30);

//3. int[] arr3 = {10,20,30};
var array3 = [10,20,30];
~~~

- java : 정적(배열크기 고정) > int[] arr = {10, 20, 30};
- java : collection 객체 > ArrayList list = new ArrayList();
- list.add("A"), list.get()....

~~~javascript
//javascript (Array : 정적, collection(동적))
var array = ["포도", "사과"];
document.write(array.toString() + "<br>");
for(var index in array){
    document.write(array[index] + "<br>");
}

//array[0] = 포도 , array[1] = 사과
array[2] = "바나나"; //동적으로 데이터 추가
document.write(array + " / " + array.length + "<br>");

array[10] = "애플망고";
document.write(array + " / " + array.length + "<br>");

document.write(array[9] + " / " + array.length + "<br>");
array[9] = "배";
document.write("초기화: " + array[9] + " / " + array.length + "<br>");

var array2 = ["one","two","three"];
document.write(array2.length+"<br>");
array2.length=2; //강제로 length값 지정
for(var index in array2){
    document.write(array2[index]+"<br>");
}

array2.length = 4;
document.write(array2.toString() + "<br>");

//Point
array2.push("Four");
document.write("push : " + array2.toString() + "<br>");
document.write("push : " + array2.length + "<br>");
//one,two,,,Four
document.write(array2.pop()+ "<br>");
document.write(array2.pop()+ "<br>");
document.write(array2.pop()+ "<br>");
document.write(array2.pop()+ "<br>");
document.write("pop : " + array2.length + "<br>");
~~~