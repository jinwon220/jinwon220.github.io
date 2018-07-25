---
title: Javascript(2)
author: JinWon
layout: post
category: JavaScript
---

# JavaScript

- 변수생성 > 타입설정 > 초기환
- 모든 변수의 타입은 > var (실제 타입은 변수가 값을 가질 때....)
    - var a;
    - a = "홍길동"; 문자열 , 홍길동이란 값으로 초기화
- 자바 스크립트 타입을 가지고 있다(정수, 실수, boolean, 문자)
- 내부적으로 String, number, Boolean, Array, Object .. 설정 됨

~~~javascript
var a;
console.log(a); //undefined
                //JAVA 언어 : public void print(){int a; syso..} 초기화 되지 않아...
                
a = 100;
console.log(a);
console.log(typeof(a)); //변수의 타입정보를 확인 할 수 있다.

var i, j;

i = 100;
j = 200;

var result = i + j;
console.log("result : " + result);

var intnum = 100; //정수
var dnum = 12.345; //실수
var flag = true; //boolean
var str = null; //값이 없다.
var str2 = "ABC"; //문자타입

console.log(typeof(intnum)); //number
console.log(typeof(dnum)); //number
console.log(typeof(flag)); //boolean
console.log(typeof(str)); //object
console.log(typeof(str2)); //string

//연산자
//산술 (+ , - , / , * , %)
var num1 = 10;
var num2 = 3;
document.write(num1/num2 + "<br>"); // 실수 범위... 
document.write(num1%num2 + "<br>"); // 나머지

//관계(== , != , >= , <= , > , < ....)
var a = 3;
var b = 5;
document.write((a==b) + "<br>");
document.write((a!=b) + "<br>");
document.write((a > b) + "<br>");

//논리 (&& , ||)
document.write((10 > 5) && (1!=3) + "<br>");
document.write((10 > 5) || (3!=3) + "<br>");

//삼항연산자
var result2 = (4%2==0)? "짝수":"홀수";
document.write("<br>" + result2 + "<br>");

//대입 연산자 (+= , -= , *= , /=)
var p = 10;
var k = 5;
// p = p + k;
p += k;
document.write("결과 : " + p);

var a = 8;
if(a % 2 == 0){
    document.write("짝수 : " + a + "<br>");
}else{
    document.write("홀수 : " + a + "<br>");
}

var c = 7;
if(c % 2 == 0){
    document.write("2의 배수 " + "<br>");
}else if(c % 3 == 0){
    document.write("3의 배수 " + "<br>");
}else{
    document.write("기타 " + "<br>");
}

var sum = 0;
for(var i = 1; i <= 100; i++){
    sum += i;
}
document.write(sum + "<br>");

for(var i = 1; i <= 9; i++){
    for(var j = 1; j <= 9; j++){
        document.write(i + "*" + j + "=" + i*j + "&nbsp&nbsp");
    }
    document.write("<br>");
}

for(var i = 1; i <= 5; i++){
    document.write("&nbsp");
    for(var j = i; j<= 5; j++){
        document.write("*");
    }
    document.write("<br>");
}
~~~