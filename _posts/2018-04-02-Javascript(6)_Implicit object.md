---
title: Javascript(6)_Implicit object
author: JinWon
layout: post
category: JavaScript
---

# JavaScript_Implicit object (내장객체)

- Date() , Array()
- 내장객체 사용시 new 연산자를 통해서 객체를 만들고 사용...
- var d = new Date();

~~~javascript
var today = new Date();
document.write(today.getFullYear()+"년<br>");
document.write(today.getMonth()+1 +"월<br>");
document.write(today.getDate() +"일<br>");

document.write(today.getHours()+"시<br>");
document.write(today.getMinutes()+"분<br>");
document.write(today.getSeconds()+"초<br>");

//Math
document.write(Math.random() + "<br>");
document.write(parseInt((Math.random()*45))+1 + "<br>");

document.write(Math.round(3.5678) + "<br>");
document.write(Math.max(3,5,7,45,12,5,47,1) + "<br>");

//문자열 관련 함수
var str="ABCDEF";
with(document){//블럭 안에 잇는 모든 문장은  : document 생략
    write(str + "<br>");
    write(str.length + "<br>");
    write(str.charAt(2)+ "<br>");
    write(str.indexOf("D") + "<br>");
    write(str.concat("홍길동") + "<br>");
    
    var str2 = str.replace("E", "ZZZZZZ");
    write(str2 + "<br>");
    write("*****************<br>")
    write(str.substring(2,4) + "<br>");
    write(str.substring(1,1) + "<br>");
    write(str.substring(1,2) + "<br>");
    write(str.substring(1) + "<br>");
    
    var strarr = "A B C D";
    var arr = strarr.split(" ");
    //arr 은 Array 배열 타입
    console.log(typeof(arr)); //object 배열 객체
    //POINT
    //java : for(String s : arr){값을 출력}
    //c# : for(String s in arr){값을 출력}
    
    //javascript : for(var s in arr){s를 출력하면 값이 아니고...}
    for(var index in arr){
        //write(index + "<br>"); 배열의 index(첨자) 값을 출력
        write(arr[index] + "<br>");
    }
}
~~~

~~~javascript
function zipSearch(obj){
    console.log(obj);
    
    var zip = obj.substring(0,7);
    var addr = obj.substr(8);
    //alert(zip+ " / " + addr);
    
    //부모창 : 자식창
    //Ex12_Script_Zipcode.html(부모창) : Ex12_popup.html(자식창)
    
    //자식이 보모창에 접근 하는 방법
    //부모 (opener 객체) >> window.opener >> 부모창의 문서 접근..
    window.opener.mainform.zipcode.value = zip;
    opener.mainform.addr.value = addr; //window생략가능
    window.close(); //self close()
}
~~~