---
title: Javascript(7)
author: JinWon
layout: post
category: JavaScript
---

# JavaScript

~~~javascript
var checkflag = "false"; //페이지내에서 모두 사용가능한 전역변수
		
function checkForm(){
    var list = document.myform.list;
    //list 변수가 Array 인가
    console.log(list);
    console.log(list.length);
    
    if(checkflag == "false"){
        for(var index in list){
            list[index].checked = true;
            //input 태그 >> checked 속성추가
        }
        checkflag = "true";
        
        return "[선택해체하기]"
    }else{
        for(var index in list){
            list[index].checked = false;
        }
        checkflag = "false";
        return "[전체선택]"
    }
    
}

////////////////////////////////////////////////////////////

var checkflag2 = "false"; //페이지내에서 모두 사용가능한 전역변수
function checkForm2(list){
    if(checkflag2 == "false"){
        for(var index in list){
            list[index].checked = true;
            //input 태그 >> checked 속성 추가
        }
        checkflag2 = "true";
        
        return "[선택해제하기]"
    }else{
        for(var index in list){
            list[index].checked = false;
        }
        checkflag2 = "false";
        
        return "[전체선택]"
    }
}

/* var name = "";
var email = "";

function initValue(frm){
    name = frm.username.value;
    email = frm.checkmail.checked; //체크 유무
}
*/
function copySend(frm){
    console.log(frm);
    console.log(frm.username.value);
    console.log(frm.checkmail.checked);
    if(frm.copy.checked){ //check 가 되었다면
        //initValue(frm);
        //frm.copyname.value = name;
        //frm.copymail.checked = email;
        frm.copyname.value = frm.username.value;
        frm.copymail.checked = frm.checkmail.value;
    }else{
        alert("선택해지");
        frm.copyname.value="";
        frm.copymail.checked = false;
    }
}

function evtproductor(frm){
    console.log(frm);
    console.log(frm.productor.value); //select 태그 value : A000, A001 ...
    console.log("index : " + frm.productor.selectedIndex);
    //select 태그의 안에 option 은 내부적으로 배열로 관리
    //배열이기 때문에 첨자(index) >> selectedIndex
    //console.log(frm.productor.options[0].value);
    //console.log(frm.productor.options[0].text);
    
    console.log(frm.productor.options[frm.productor.selectedIndex].value);
    console.log(frm.productor.options[frm.productor.selectedIndex].text);
    
    var index = frm.productor.selectedIndex;
    var val = frm.productor.options[index].value;
    var text = frm.productor.options[index].text;
    
    if(index==0){
        frm.valProductor.value="";
        frm.txtProductor.value="";
    }else{
        frm.valProductor.value=val;
        frm.txtProductor.value=text;
    }
}

function evtBrand(){
    //parameter 없어요
    //위와 같은 동작을 하도록 작성하세요
    var index = document.frmdata.brand.selectedIndex;
    var val = document.frmdata.brand.options[index].value;
    var text = document.frmdata.brand.options[index].text;
    
    if(index==0){
        document.frmdata.valBrand.value="";
        document.frmdata.txtBrand.value="";
    }else{
        document.frmdata.valBrand.value=val;
        document.frmdata.txtBrand.value=text;
    }
}
~~~