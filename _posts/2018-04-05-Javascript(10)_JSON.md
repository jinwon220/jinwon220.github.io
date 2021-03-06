---
title: Javascript(10)_JSON
author: JinWon
layout: post
category: JavaScript
---

# JavaScript_JSON

~~~
자바 설계도(클래스) => 재사용성
class Product{
    private String carname="pony";
    
    public Product(){}
    public Product(String carname){
        this.carname = carname;
    }
    public void print(){
        System.out.prinln(this.carname);
    }
}
~~~

~~~
메모리 load ... (new)
Product p = new Product();
Product p2 = new Product("pony2");
p.print();
p2.print();
/////////////////////////////////////////////////
javascript >> 객체지향 언어(OOP)
~~~

### 클래스 정의 3가지 방법

1. 프로토타입 방식 : 일반적인 클래스 제작 방법
        인스턴스마다 공통된 메서드를 공유해서 사용하는 장점 Jquery 도 prototype 방식으로 설계


2. 함수 방식 :  간단한 클래스 제작 시 사용
                                인스턴스마다 메서드가 독립적으로 만들어지는 단점


3. 리터럴 방식  : 클래스 만드는 용도는 아니며 주로 여러개의 매개변수를 그룹으로 묶어 함수의 매개변수로 보낼때
            정의와 함께 인스턴스가 만들어지는 장점이 있음 단 인스턴스는 오직 하나

4. ECMA6 버전부터 : class 키워드 제공
    ~~~
    class Person {
        constructor(name) {
        this._name = name;
        }

        sayHi() {
        console.log(`Hi! ${this._name}`);
        }
    }
    ~~~

### [javascript 객체 생성]
1. 오브젝트 리터럴 방식 (객체를 만드는 방법): 클래스 생성과 동시에 객체가 만들어진다.
    - 리터럴 방식 -> 제일 간단한 방법 -> var obj = {}; 객체를 만드는 블럭 // var objarr = []; 배열 (헷깔리지 말것!!!)
    - JSON 표기 : {} >> JSON : JavaScript Object Notation.
        - ex) var myObj = { "name":"John", "age":31, "city":"New York" };
        - TIP) JSON >> XML
> XML : 이중간의 데이터 호환 (한 때는 서점 : XML webserivice)

### 다른 이야기
### 객체지향언어 장점 : 설계도 (재사용성)
* 오브젝트 리터럴 방식 : 재사용을 지원하지 않는다.
* 설계도를 생성과 동시에 객체 생성(장점 : 편하고, 빠르다)
* 설계도를 미리 만들어 놓고 재사용하는 방식은 아니다
* 설계도당 하나의 객체만 생성 사용(only object)

~~~
var product = {};
var product = {제품명 :'사과', 년도:'2018', 원산지:'대구'};

var 인스턴스 = {
            프로퍼티:초기값,
            포로퍼티:초기값,
            ....
            메서드:function(){},
            메서드:function(){}....
            }
리터럴 방식 > 선언과 동시에 인스턴스 자동 생성
var 인스턴스 = ()
특징 : 생성자가 존재하지 않는다.
    프로퍼티와, 메서드만 정의 가능
단점 : 객체 하나 생성(재사용성 없다)
접근방법 : 인스턴스이름.자원  -> product2.제품명
~~~

~~~javascript
var product = {제품명 : '사과', 년도 :"2018", 원산지:"대구"};
document.write(product.제품명 + "<br>");
document.write(product.원산지 + "<br>");
document.write(product.년도 + "<br>");
document.write(product.toString() + "<br>");

//
var person = {name:"홍길동", 
                addr:'서울시 강남구 역삼동',
                eat:function(food){
                    document.write(this.name + "/" + this.addr + "/" + food + "냠냠");
                }
                };
document.write("<hr>");
person.eat("apple");
document.write("<br>"+person.name + "<br>");

//1. 속성제거
//var product = {제품명 :'사과', 년도:'2018', 원산지:'대구'};
delete(product.년도);
var output="";
/*  
    for(var index in Array){
        Array[index];
    }
*/
//아래 for문에서는 in -> 객체 -> 키값을 return
//{키:값, 키:값, 키:값}
for(var key in product){
    output += "key : " + key + " / " + product[key] + "<br>";
}
document.write(output);

//person 객체도 for문
for(var key in person){
    document.write("key : " + key +" / " +  person[key] + "<br>");
}

//json 객체의 활용
//외부 API (우편번호 , 서울시 화장실 정보 , 날씨정보)
//JSON 데이터 형태로 받아서 (필요한 정보만 추출 해서 사용)

var Member={}; // 풀어쓰면 var Member = new Object();
//빈 객체를 만들고 자원 추가 가능
Member.name = "hong"; //name 속성 과 값을 추가
//Member["name"] = "hong";
document.write(Member.name + "<br>");
Member.age = 100;
document.write(Member.age + "<br>");

//필요하다면 함수 추가
Member.print = function(){
    document.write(this.name + " / " + this.age + "<br>");
}

Member.print();

//JSON 객체 해석하기
//POINT 속성이 json 객체여도 된다...
var Grade={
            list : {
                        jinwon:10,
                        hong:20,
                        kim:30
                        },
            show : function(){
                        for(var key in this.list){
                            document.write(key + ":" + this.list[key] + "<br>");
                        }						
                    }
            };

Grade.show();
document.write("<hr>");
//POINT
var listobj = Grade.list; //listobj 리터럴 객체
for(var key in listobj){
    document.write(key + ", " + listobj[key] + "<br>");
}
~~~