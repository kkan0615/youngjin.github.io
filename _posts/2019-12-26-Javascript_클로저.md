---
layout: post
title:  "[Javascript] 클로저가 뭐에요?"
image: ''
date:   2019-12-26 00:06:31
tags:
- javascript
description: 'Learn Javscript closure'
categories:
- javascript
---

## 클로저(closuer)
클로저는 내부함수가 외부함수 맥락(context) 혹은 변수에 접근할 수 있는 것을 말합니다.

클로저는 자바스크립트 할 때 자주 만나는 에러이며 상위급 개념입니다.
## 클로저의 4가지 방식

### 1번
내부 함수에서 외부함수의 변수 가져오기

{% highlight javscript %}
function outter() {
    var test = 'I am test statement';
    function inner() {
        console.log(test); // I am test statement 가 출력된다.
    }
        inner();
    }
outter();
{% endhighlight %}

test라는 이름의 변수는 inner function 밖에 선언 되었으나 정상적으로 출력되는 것을 알 수 있다.

### 2번
클로저는 외부함수가 리턴된 이후에도 변수에 접근 할 수 있다.

{% highlight javscript %}
function outter(title){
    let TotalTitle = 'Total Title: ';
    function inner(subTitle){
        return TotalTitle + title + " " + subTitle;
    }
    return inner;
}
const test = outter('test Title');
//console.log(test("sub test")); // Total Title: test Title sub test 가 출력된다.
test("sub test") // Total Title: test Title sub test
{% endhighlight %}

### 3번
내부함수를 이용하여 외붐함수의 변수를 변경 할 수 있다.
function parameterReturns() {
    var test = 'test'; // 외부함수 변수
    // 모든 내부함수는 외부변수에 접근할 수 있습니다.
    return {
        getTest: () => {
            // 이 내부함수는 test를 return 합니다.
            // test의 값이 변하면 변한 값을 return 하게 됩니다.
            return test;
        },
        setTest: (newTest) => {
            // 이 내부함수는 외부함수의 값을 언제든지 변경할 것입니다.
            test = newTest;
        }
    }
}
var pr = parameterReturns(); // 이 시점에, parameterReturns의 외부함가 return 됩니다.
var pr2 = parameterReturns(); // setTest로 pr의 외부함수 변수를 변경해도 바뀌는지 확인하기 위한 용도
console.log(pr.getTest()); // test 가 출력된다
console.log(pr2.getTest()); // test 가 출력된다
pr.setTest('newTestWord'); // newTestWord로 외부 변수의 값을 변경한다
console.log(pr.getTest()); // newTestWord 가 출력된다.
console.log(pr2.getTest()); // test 가 출력된다
{% endhighlight %}

### 문제와 함께 생각해보기
{% highlight javscript %}
var arr = []
for(var i = 0; i < 5; i++){
    arr[i] = function(){
        return i;
    }
}
for(var index in arr) {
    console.log(arr[index]()); // 5 5 5 5 5 가 출력된다.
}
{% endhighlight %}

아마도 여러분들은 0 1 2 3 4 가 출력 될것이라 기대하지만
5 5 5 5 5 가 출력될 것이다.

이를 해결하기위해서는 아래와같이 코드를 짜야한다.
{% highlight javascript %}
var arr = []
for(var i = 0; i < 5; i++){
    arr[i] = function(id) {
        return function(){
            return id; // 만약 그냥 i로 했으면 5로만 출력된다
        }
    }(i);
}
for(var index in arr) {
    console.log(arr[index]()); // 0 1 2 3 4 가 출력된다.
}
{% endhighlight %}

#### [추가] var대신 let을 사용 할 경우
{% highlight javascript %}
let arr = []
for(let i = 0; i < 5; i++){
    arr[i] = function(id) {
        return function(){
            return id + i;
        }
    }(i);
}
for(let index in arr) {
    console.log(arr[index]()); // 0 2 4 6 8
}

// 아래도 가능합니다.
let arr = []
for(let i = 0; i < 5; i++){
    arr[i] = function() {
        return function(){
            return i;
        }
    }(i); //(i) 가없다면 function만 출력된다.
}
for(let index in arr) {
    console.log(arr[index]()); // 0 1 2 3 4
}
{% endhighlight }

## Reference
1. https://opentutorials.org/course/743/6544 [생활 코딩]
2. http://chanlee.github.io/2013/12/10/understand-javascript-closure/ [chanlee blog]
3. http://javascriptissexy.com/understand-javascript-closures-with-ease/
4. https://medium.com/@khwsc1/%EB%B2%88%EC%97%AD-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%8A%A4%EC%BD%94%ED%94%84%EC%99%80-%ED%81%B4%EB%A1%9C%EC%A0%80-javascript-scope-and-closures-8d402c976d19 [Hyeokwoo Alex Kwon blog]

## 끝으로
댓글 혹은 이메일을 통해 피드백 받습니다. 혹시 예시가 더 필요하시면 제가 추가적으로 올리도록 하겠습니다.
