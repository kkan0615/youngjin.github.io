---
layout: post
title:  "[Javascript] Arrow Function Expression 화살표함수 사용법"
image: ''
date:   2019-12-27 00:06:31
tags:
- javascript
description: 'Learn v-show v-if!'
categories:
- javascript
---

## 화살표 함수
화살표 함수 표현(arrow function expression)은 ES6 문법으로써 function 표현에 비해 구문이 짧고 자신의 this, arguments, super 또는 new.target을 바인딩 하지 않습니다. 화살표 함수는 항상 익명입니다.
화살표 함수 표현은 mothod 함수가 아닌 곳에서 적합하기에 생성자로 사용 될 수 없습니다.

## 기본 예시
{% highlight javascript %}
// 1. 매개변수 없이 사용
const test = () => console.log('message');
test(); // message 출력

// 2. 매개변수를 하나만 사용
const test = a => a;
console.log(test('message')); // message 출력

// 3. 매개변수를 하나 이상 사용
const test = (a, b)=> a + b;
console.log(test('message', ' add')); // message add 출력

// 4. 매개변수를 하나 이상 사용하고 { } block을 이용하여 만들경우
const test = (a, b)=> { return a + b }; // return을 무조건 사용해야합니다.
console.log(test('message', ' add'));

// 5. block 사용시 여러줄을 사용가능
const test = (a, b)=> {
    const space = ' space ';
    return a + space + b // return을 사용 하는 것을 볼 수 있다.
};
console.log(test('message', 'add')); // message space add 출력

// 6. 콜백 함수와 함께 사용
const fruits = [
    'apple',
    'banana',
    'peach',
    'water melon',
]

console.log(fruits.map(fruit => fruit.length)); // Array(4) [5, 6, 5, 11] 출력

// 7. 콜백 함수와 함께 사용 예제 2번
const fruits = [
    'apple',
    'banana',
    'peach',
    'water melon',
]
const bigFruits = fruits.filter(fruit => {
    return fruit.length > 5; // string가 5넘는 것만 출력
});
console.log(bigFruits); // Array(2) ["banana", "water melon"] 출력

// 8. 배열 return
const test = () => {
    return [
        'apple',
        'banana',
        'peach',
        'water melon',
    ]
};

console.log(test()); // Array(4) ["apple", "banana", "peach", "water melon"] 출력
{% endhighlight %}

## this와 함께 사용
화살표 함수는 외부 이 함수를 포함하고있는 scope에서 this를 가져온다.

{% highlight javascript %}
function Person(family) {
    this.family = family; // this를 활용하여 데이터 넣기
} // 생성자는 화살표 함수 사용 불가

Person.prototype.nameArray = function (arr) {
    return arr.map(x => `${ this.family }  ${ x }`); // { } 을 사용 하지않았음으로 return 사용하지 않음
}; // 메소드 함수는 화살표 함수 사용 불가

const person = new Person('Kwak');
console.log(person.nameArray(['youngjin', 'junsub'])); // Array(2) ["Kwak  youngjin", "Kwak  junsub"] 출력
{% endhighlight %}

## Reference
1. https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/%EC%95%A0%EB%A1%9C%EC%9A%B0_%ED%8E%91%EC%85%98 [MDN web docs]
2. https://poiemaweb.com/es6-arrow-function [poiemweb]
3. https://velog.io/@ki_blank/JavaScript-%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98Arrow-function [ki_blank velog]
4. https://jeong-pro.tistory.com/110 [정아마추어 블로그]

## 끝으로
댓글 혹은 이메일을 통해 피드백 받습니다. 혹시 예시가 더 필요하시면 제가 추가적으로 올리도록 하겠습니다.
