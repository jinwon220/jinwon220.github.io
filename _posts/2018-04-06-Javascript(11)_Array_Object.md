---
title: Javascript(11)_Array_Object
author: JinWon
layout: post
category: JavaScript
---

# JavaScript_Array_Object

1. 배열 -> [] -> var arr = [10,20,30];
2. JSON -> {} -> var obj = {}

~~~javascript
//배열과 객체 혼용
var students = [];
console.log(students);

//배열에 객체 추가
//javascript Array (push(), pop() >> stack > LIFO)
students.push({이름 : "홍길동", 국어:80,  영어:60 });
students.push({이름 : "아무개", 국어:100, 영어:50 });
students.push({이름 : "이순신", 국어:50,  영어:100});

//기존 객체에 함수 추가
for(var index in students){
    //var obj = students[index]; //obj 는 json 객체
    students[index].getSum = function(){return this.국어+this.영어};
    students[index].getAvg = function(){ return this.getSum()/2};
    
}
console.log(students)

//문제
//students 배열에 담겨있는 학생의 이름, 총점, 평균을 출력하세요
for(var index in students){
    document.write("이름 : " + students[index].이름 + ", 총점 : " + students[index].getSum()
                    + ", 평균 : " + students[index].getAvg() + "<br>");
}
~~~