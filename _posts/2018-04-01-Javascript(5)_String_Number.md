---
title: Javascript(5)_String_Number
author: JinWon
layout: post
category: JavaScript
---

# JavaScript_문법 (문자열, 숫자)

1. eval()
    - 문자형 수식을 계산
    - 문자형 숫자 -> 숫자
~~~javascript
document.write("eval()" + eval(str) + "<br>");
~~~

2. isNaN()=> Not a Number -> 값이 문자 : true (너 숫자 아니니?) , 숫자 : false
~~~javascript
document.write(isNaN("12345") + "<br>"); //숫자 (false)
document.write(isNaN("12345A") + "<br>"); //문자 (true)
document.write(isNaN("대한민국") + "<br>"); //문자 (true)
document.write(isNaN('12345') + "<br>"); // 숫자 (false)
~~~

~~~
javascript 문자열 함수 학습하기
https://www.w3schools.com/js/js_string_methods.asp


javascript 숫자 함수 학습하기
https://www.w3schools.com/js/js_number_methods.asp 
Number()	Returns a number, converted from its argument.
parseFloat()	Parses its argument and returns a floating point number
parseInt()	Parses its argument and returns an integer
~~~

~~~javascript
var i = "100";
var j = "200";
document.write("결합 : " + (i+j)); //java , javascript + (산술, 결합)
document.write("<br>숫자변환 : " + (Number(i)+ Number(j)));
~~~

~~~
javascript EVENT
https://www.w3schools.com/js/js_events.asp

onchange	An HTML element has been changed
onclick		The user clicks an HTML element
onmouseover	The user moves the mouse over an HTML element
onmouseout	The user moves the mouse away from an HTML element
onkeydown	The user pushes a keyboard key
onload		*****The browser has finished loading the page*****

이벤트(this 버튼 자신)
<button onclick="this.innerHTML=Date()">The time is?</button>

onblur() > 포커스가 요소를 떠날 때
onfocus() > 포커스가 요소에 들어올 때
~~~

~~~javascript
/*
function myfunction(){
    //alert("onblur");
    var jumin = document.myform.jumin1;
    if(isNaN(jumin.value)){
        alert("숫자 입력해");
        jumin.focus();
        jumin.select();
    }
}
function myfocus(){
    //alert("onfocus");
    alert("앞자리 주민번호 \n 입력 잘하셨어요")
}
*/

function focusFunction(){
    document.getElementById("myinput").style.background="yellow";
}
function blurFunction(){
    document.getElementById("myinput").style.background="red";
}
function selectTag(){
    //onchange >> select 태그에 값 변화가 있으면
    alert(document.myform.sal.value);
}

//point
function changeColor(obj){
    console.log(obj);
    obj.style.backgroundColor="gold";
}
function changeColor2(obj){
    console.log(obj);
    obj.style.backgroundColor="white";
}
~~~