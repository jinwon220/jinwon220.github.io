---
title: Javascript(9)_DOM
author: JinWon
layout: post
category: JavaScript
---

# JavaScript_DOM

~~~
When a web page is [loaded],
the browser creates a Document Object Model of the page.
The HTML DOM model is constructed as a tree of Objects:
    
JavaScript can change all the HTML elements in the page
JavaScript can change all the HTML attributes in the page
JavaScript can change all the CSS styles in the page
JavaScript can remove existing HTML elements and attributes
JavaScript can add new HTML elements and attributes
JavaScript can react to all existing HTML events in the page
JavaScript can create new HTML events in the page
    
DOM Script 사용해서 >> 위 작업을 수행
~~~

- onload 이벤트 발생 시점 : body 태그 안에 있는 모든 요소가 브라우져에 - loading 되고 나서 발생
~~~
<body onload=call()>
window.onload = call() 코드 방시도 적용 가능

DOM 처리를 위한 [ 함수 학습하기 ]
https://www.w3schools.com/js/js_htmldom_document.asp
~~~
~~~javascript		
window.onload = function(){ //함수 이름이 없어요(익명함수 : 일회성) : 재활용 안하겠다.
    //page 요소 loading 된 후에 함수가 자동실행되게 하겠다.
    //body 안쪽을 제어 할 수 있다.
    //alert("gogo");
    var out = "";
    out += "<ul>";
        out += "<li>javascript</li>";
        out += "<li>jquery</li>";
        out += "<li>oracle</li>";
    out += "<ul>";
    document.body.innerHTML=out;
    
    var header = document.createElement("h1");
    //<h1></h1>
    var textnode = document.createTextNode("Hello");
    
    header.appendChild(textnode);
    //<h1>hello</h1>
    
    document.body.appendChild(header);
    
    //img 태그 생성하고 src 속성을 만들고 body append
    var img = document.createElement("img"); //<img>
    img.setAttribute("src", "images/bono2.gif"); //<img src="images/bono2.gif">
    img.setAttribute("width", "100");
    img.setAttribute('hieght', '100'); //'' ""상관없음
    //<img src="images/bono2.gif" width="100px" height="100">
    document.body.appendChild(img);
}
~~~

~~~javascript
window.onload = function(){
    var pele = document.createElement("p");
    var hrele = document.createElement("hr");
    var h3ele = document.createElement("h3");
    var textnode = document.createTextNode("hello world");
    var divele = document.createElement("div");
    var textnode2 = document.createTextNode("HI HI HI");
    
    //<hr title="수평선">
    hrele.setAttribute("title", "수평선");
    
    //<h3>hello world</h3> //기존에 있는 node를 자식요소로
    h3ele.appendChild(textnode);
    
    //<div></div>
    divele.appendChild(textnode2);
    
    //body 요소에 추가
    document.body.appendChild(pele).appendChild(divele);
    //<body><p><div>HI HI HI<div></p></body>
    
    document.body.appendChild(hrele);
    //<body><p><div>HI HI HI<div></p> <hr title="수평성"> </body>
    document.body.appendChild(h3ele);
    //<body><p><div>HI HI HI<div></p> <hr title="수평성"> <h3>hello world</h3> </body>
    
}
~~~

~~~javascript
/*
window.onload =function(){
    document.getElementById("b1").onclick =function(){
        //동적으로 생성되는 테이블
    }
    document.getElementById("b2").onclick =function(){
        //생성된 테이블 삭제
    }
}
*/
function createTable(){
    //동적으로 생성되는 테이블
    /*
    2행 2열 (text 값)
    <table id="tab">
        <tr><td></td><td></td></tr>
        <tr><td></td><td></td></tr>
    </table>
    <tr><td></td><td></td></tr>
    만들어진 table body > div >  의 자식요소로 추가
                
    */
    var intRow = parseInt(document.getElementById("txtrow").value);
    var intColumn = parseInt(document.getElementById("txtcolumn").value);
    
    var eletable = document.createElement("table");
    eletable.setAttribute("id", "tab");
    //<table id="Tab"></table>
    
    for(var i = 0 ; i < intRow ; i++){
        var elerow = document.createElement("tr");
        for(var j = 0 ; j < intColumn ; j++){
            if( i == 0){
                var eCell = document.createElement("th");
                var eText = document.createTextNode((i+1)+"행" + ","+(j+1)+"열");
                eCell.appendChild(eText);
                elerow.appendChild(eCell);
                //<tr><th>1행1열</th></tr>
            }else{
                var eCell = document.createElement("td");
                var eText = document.createTextNode((i+1)+"행" + ","+(j+1)+"열");
                eCell.appendChild(eText); // 추가
                elerow.appendChild(eCell);
            }
        }
        //tr 생성시 
        eletable.appendChild(elerow); //Table tr add
    }
    //body 
    document.getElementById("div").appendChild(eletable);

}

function deleteTable(){
    //생성된 테이블 삭제
    //1. Table id = "tab";
    //같은 id 가진 Table 여러개 있다.
    //var tab = document.getElementById("tab"); //하나의 요소만 선택 (제일 먼저 만나는 녀석 한놈만)
    //document.getElementById("div").removeChild(tab);
    //삭제 (먼저 생성된 것부터 >> 형부터 삭제 >> 같은ID라면)
    
    //////////////////////////////////////////////////////////////////////////////
        
    //2.테이블의 아이디가 없다면
    //document.getElementsByName(name)
    //document.getElementsByClassName(name)
    var tables = document.getElementsByTagName("table"); //page 안에 있는 table 요소 다가지고 와요..(list)
    //console.log(tables);
    //console.log(tables.length);			
    //console.log(tables[tables.length-1]);
    
    ///////////////////////////////////////////////////////////////////////////
    
    //마지막에 생성된 테이블 부터 삭제하고 시퍼요
    if(tables.length > 0){
        document.getElementById("div").removeChild(tables[tables.length-1]);
    }else{
        alert("모두 삭제 되었습니다.");
    }
}
~~~

### DOM(성능 조금... 다루기 편함) <-> SAX(event 기반) (성능 조금 나은데... 다루기 어려워요)

~~~javascript
window.onload=function(){
    var menode;
    menode = document.getElementById("me");
    menode.style.backgroundColor="yellow"; //css 제어
    
    var parentnode = menode.parentNode; //body
    parentnode.style.backgroundColor="skyblue";
    
    var grandnode;
    grandnode = parentnode.parentNode; //html
    grandnode.style.backgroundColor="green";
    
    var my = document.getElementById("mychild");
    console.log(my);
    console.log(my.firstChild.nodeName); //요소이름(태그이름)
    console.log(my.firstChild.innerText); //value 속성 없어요
    console.log(my.firstChild.nextSibling.innerText); //형제 Text
    console.log(my.childNodes.length); //모든 LI 요소 개수
    console.log(my.lastChild.innerText); //모든 LI 요소 개수
    
}
~~~