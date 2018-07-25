---
title: Javascript(3)_function
author: JinWon
layout: post
category: JavaScript
---

# JavaScript_function

### java
- java : public void print(){}
- java : public String print(){return ""}
- java : public int print(int num){return 100+num}

### javascript
- function 함수이름(){} >> void(x) , String return type(x)

~~~javascript
function callConfirm(){ //사용자 정의 함수
    if(window.confirm("작업내용")){ //내장함수
                                //확인, 취소버튼(true, false) 리턴
        window.alert("삭제합니다"); 
    }else{
        alert("취소합니다"); //내장함수 : alert()
    }
}

function showPopup() {
    //https://www.w3schools.com/Jsref/met_win_open.asp
    //표를 확인하고 브라우져를 적용여부를 확인한다
    //window.oprn()
    //window.close() -> close the currect window
    //window.open(URL, name, "값,값,,,(specs)", replace)
    //"specs" 브라우져마다 적용이 다르다
    window.open("Ex06_popup.html", "zipcode", "width=200,height=200");
}

function myFunction() {
    var myWindow = window.open("Ex06_popup.html", "MsgWindow", "width=200,height=100");
    myWindow.document.write("<p>This is 'MsgWindow'</p>");
}

//java : public String goUrlTime(String url){return ""}
function goUrlTime(url) { //함수의 parameter 타입을 갖지 않는다 (원하는 변수명 기술)
    window.setTimeout("location.href='"+url+"'", 3000); // 특정 주소로 이동 , 3000 = (3초 이후에)
    
    //location.href="http://www.daum.net";
}

//javascript return 값
function myfun(a,b) {
    return a+b;
}

var result = myfun(100,200);
console.log("myfun 함수 호출 : " + result);

//익명함수 (함수의 주소를 객체에 할당)
changeColor = function(){
    window.document.bgColor="silver";
    window.document.fgColor="red";
}

var myTime = window.setInterval(function(){ //일회성 함수 (익명함수) seInterval 한번 사용 1회성 함수
    //javascript 객체 (Date 객체 제공)
    //Date d = new Date(); JAVA
    var d = new Date();
    var t = d.toLocaleTimeString();
    //DOM script 사용
    document.getElementById("time").innerHTML = t;
    //<p>시간...</p>
    
}, 1000); //1초마다 실행

function myTimeStop(){
    window.clearInterval(myTime);
}
~~~