---
title: Javascript(4)_validity
author: JinWon
layout: post
category: JavaScript
---

# JavaScript_validity 유효성 검사

### 유효성 검증
- 입력-> 입력한 데이터 서버 전송 -> 서버에서 검증 -> 클라이언틍에게 응답 -> 처리
- 서버의 부담을 줄이고 서버에서 검증할 필요 없는 데이터는
- 클라이언트의 브라우져에서 처리하고 -> 검증 완료 -> 서버로 전송
- javascript 언어를 통해서 검증해라..

1. txtuserid 태그의 정보(객체)를 가지고 와서 검증(입력체크)
~~~javascript
function send(){
//document.forms[0].elements[0]
//form name속성도 없고, input 태그에 name속성이 없는 경우

var userid = document.loginform.txtuserid;
//userid 변수에 : <input type="text" name="txtuserid">
console.log(userid);

var userpwd = document.loginform.txtpwd;
//userpwd 변수에 : <input type="password" name="txtpwd">
console.log(userpwd);

if(userid.value == "" || !(userid.value.length >= 3 && userid.value.length <=10)){
    alert("다시 입력하시오");
    //document.loginform.txtuserid.focus();
    userid.focus(); //커서가 그 박스에서 깜빡이게 해줌
    userid.select(); //크 텍스트 박스에 있는 모든 문자들을 다 드래그 해줌
}else {
    alert("전송완료");
    //전송하는 코드
    document.loginform.action = "Ex09_login.jsp";
    document.loginform.submit(); //서버전송
}
}
~~~