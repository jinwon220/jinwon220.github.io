---
title: Javascript(1)
author: JinWon
layout: post
category: JavaScript
---

# JavaScript_Basic

### Javascript 블럭 주석	
- javascript (언어: Web : 코드해석(웹 브라우져) : 라인단위(해석))
- javascript (oop 객체지향언어 : 내부적으로(class 개념))

### Javascript 사용
1. html의 content , attribute [변경] , [삭제] , [추가] (동적으로)
2. css content [변경], [삭제], [추가](동적으로)
3. 유효성 검증 (id 체크, 주민번호 검증)
4. 동적인 웹페이지 구성
5. 전 세계적으로 1위
    - javascript framework >> jquery.js , angular.js , Node.js(server) , react.js , vue.js
    - 장점 : 개발시간 단축 , 유지보수 
    - 단점 : 종속(lib) >> framework 만들 수 있는 javascript 합습 되야 한다.
6. 문법
    - 1. 대소문자를 엄격히 구분
    - 2. 종결자(;)
    - 3. 타입, 연산자, 제어문 ....
> 참고) java 코드와 비슷한 것들이 많음...
    
7. 사용법 (CSS와 동일 : common.css)
    - in-line (테그 안쪽에 표현 : <p onclick="<script....")
    - internal (<head><script..../head>)
    - external (common.js) -> link 방식으로 사용

### internal
~~~javascript
function call() {
    //java : public void call(){System.out.println('')}
    alert('internal');
}
~~~

- System.out.println();
- javascript를 해석 하고, 결과를 출력 하는 곳은 (웹 브라우져)
- 웹 브라우져(해석기 + script API 지원)
- window 객체 (브라우져 객체 가지고 있고...)
- window.document 객체
- document 객체활용 하면 (화면 출력, 값 처리)
- window.document.write("출력하기<br>");
- document.write("window객체 생략가능<br>");
~~~javascript
console.log("웹 브라우져 출력창 제공");
console.log("웹 브라우져의 cmd 창에 원하는 결과 확인");
console.log("디버깅, 결과확인, 오류메시지");

document.write("<br><b>Hello World</b><br>");
document.write("<table border='1'>");
document.write("<tr><td>AA</td><td>BB</td><td>CC</td></tr>");
document.write("</table>");

//console.log() 검증데이터 확인
console.log(100+200);
~~~

### [Javascript 순차적으로 실행된다 ^^ (순서가 정말 중요하다!!)]

~~~
Cannot read property 'elements' of undefined
순차적인 line 단위를 해서 (form 가 read 해서 메모리에 올라와 있어야 하는데...)
var element = window.document.forms[0].elements[0];
alert(element);
~~~

~~~javascript
//함수는 line단위의 해석이여도 호출이 되지 않았기 때문에 오류가 나지 않는다.
function data(){ //함수의 정보 read 하는데 안에 있는 코드를 실행하지 않아요>> 실해은 호출에 의해서만
    var element = window.document.forms[0].elements[0];
    alert(element);
}
~~~

~~~javascript
//시점 : form 테그를 read 한 후에.....
var element = window.document.forms[0].elements[0];
console.log(element); //element 에 담긴 값은 : <input type="text" name="userid" value="hong">
console.log(element.value);
console.log(element.value.length);

//웹 브라우져 객체를 가지고 있다...
//window.alert("정상");
//alert("생략가능");

//객체가 계층 형태를 이루고 있다.
//form 태그가 이름을 가지고 있다면 (name = "myform")
//window.document.폼이름.요소이름
//window.document.폼이름.요소이름.value 
var ele = window.document.myform.userid;
//ele 변수안에 inputElement 객체가 >> <input type="text" name="userid" value="hong">
var value = window.document.myform.userid.value;

console.log("input 태그 : " + ele);
console.log("input 태그 값 : " + value);

if(value.length < 5){
    alert("다시입력해");
    ele.value="";
}
~~~

# DOM 모델을 사용하는 javascript

~~~javascript
function print(){
    var element = document.getElementById("demo");
    console.log(element);
    element.innerHTML = "HELLO WORLD";
}
~~~