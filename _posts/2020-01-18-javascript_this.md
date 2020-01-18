---
layout: post
title:  "[Javascript] this 사용법"
image: ''
date:   2020-01-18 00:06:31
tags:
- javascript
description: 'What is this keyword in javascript'
categories:
- javascript
---

## this
자바스크립트에서의 this는 타언어와 달리 상당히 어려운 개념이다. 이는 인터뷰에서도 많이 나오는 질문이라 외워두는 편이 편하다.
실행 컨텍스트는 함수를 호출 할때 생성되므로, this는 상황에 따라 다르게 결정된다.
1. 함수에서의 this
2. 메소드에서의 this
3. 생성자에서의 this
4. 객체에서의 this
5. 간접 실행에서의 this
6. 바인딩 함수에서의 this
7. 화살표 함수에서의 this

### this의 전역 객체
브라우저 에서는 window 객체, Node 환경에서는 global객체이다.

## 1번 - 함수에서의 this

### 함수 실행 방법
우선 함수 실행 방법부터 보자
{% highlight javascript %}
/* 함수 정의하기 */
function foo(name) {
    return 'Hello ' + name + ' !!!';
}

const test = foo('World');
console.log(test);
{% endhighlight %}

### 1번 - 함수에서의 this

{% highlight javascript %}
function name(param) {
    if(this === window) {
        console.log('I am window ' + param); // 이 줄이 출력 될 것입니다.
        this.message = 'Window Message'; // window라는 객체 안에 message를 만들고 저장할 것입니다.
    } else {
        console.log('I am not window ' + param);
    }
}

name('test'); // I am window test
console.log(window.message); // Window Message
{% endhighlight %}

### strict mode(엄격 모드) 에서의 this

{% highlight javascript %}
function name(param) {
    'use strict'; // 함수에서 엄격 모드 사용
    if(this === window) {
        console.log('I am window ' + param);
        this.message = 'Window Message';
    } else {
        console.log('I am not window ' + param); // 이 줄이 출력 될 것입니다.
    }
}

name('test'); // I am not window test
console.log(window.message); // error: window에 message가 저장되지 않았음!
{% endhighlight %}

### 내부 함수에서의 this

{% highlight javascript %}
function outer() {
    function inner() {
        console.log(this); // 전역 객체
    }
    inner();
    console.log(this); // 전역 객체
}

outer();
{% endhighlight %}
## 2번 - 메소드에서의 this
메소드에서의 this는 메소드 실행때 메소드를 소유하고 있는 객체입니다.
{% highlight javascript %}
const calculater = {
    num: 0,
    sum: function(a) {
        this.num += a; // 여기에서의 this는 calculater를 가르켜서 이 객체안에 있는 num을 사용 할 수 있는 것이다.
        return this.num;
    },
    check: function() {
        console.log(this === calculater); // true가 메소드를 소유하고 있는 객체를 의미하기에 출력 될 것입니다.
    },
}

console.log(calculater.sum(5)); // 5
console.log(calculater.sum(2)); // 7
calculater.check(); // true
{% endhighlight %}

함수와 메소드에 관한 내용은 -> <a link="#">더보기</a>를 클릭하여 보실 수 있습니다.

## 3번 - 생성자에서의 this
생성자는 new 키워를 붙여야 한다. ES6 문법 부터는 class를 사용 할 수 있다. 이번 기회에 둘다 알아보자.'
생성자 실행에서의 context(문맥)은 새롭게 만들어진 객체입니다. 주로 생성자로 객체의 초기값을 만들 때 사용합니다.
### 생성자에서의 this

{% highlight javascript %}
function person(_name, _age) {
    this.name = _name;
    this.age = _age;
}

/* Peson객체 메소드 추가 */
person.prototype.changeName = function(newName) {
    this.name = newName;
}

const p = new person('Youngjin', '22');
console.log(p.name); // Youngjin 이 출력
p.changeName('Requiem');
console.log(p.name); // Requiem 이 출력
{% endhighlight %}

### class 생성자에서의 this

{% highlight javascript %}
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    changeName(newName) {
        this.name = newName;
    }
}

const p = new Person('Youngjin', '22');
console.log(p.name); // Youngjin 이 출력
p.changeName('Requiem');
console.log(p.name); // Requiem 이 출력
{% endhighlight %}

#### 추가 설명
두 방법 모두 새롭게 만들어진 객체를 가르킨다. 단순 class냐 아니냐의 차이를 가지고 있다.

또한 new 연산자를 사용해야 하며 사용 하지 않을 시 에러를 발생시킨다! -> <a link="#">더보기</a>

## 4번 - 객체에서의 this
사실 눈치가 빠른 사람은 이미 알겠지만 객체에서의 this는 객체를 가리킨다.

## 5번 - 간접 실행에서의 this
간접실행이란 call(), apply() 메소드를 호출하는 것을 의미한다.
간접 실행에서 첫 번째 매개변수로 this를 받습니다.
{% highlight javascript %}
var person = { name: 'Youngjin' };
function addLastName(lastName) {
    console.log(this); // this는 person 객체를 가리키며, 객체의 정보를 출력한다.
    return this.name + " " + lastName
}
// 두 메소드 모두 person(this가 될)을 첫 번째 매개 변수로 받았다.
console.log(addLastName.call(person, 'Kwak')); // Youngjin Kwak
// apply() 메소드는 call() 메소드와 달리 Array(배열)을 두 번째 매개변수로 받는다.
console.log(addLastName.apply(person, ['Kwak'])); // Youngjin Kwak
{% endhighlight %}

## 6번 - 바인딩 함수에서의 this
bind() 메소드와 함께 사용되며 이는 새로운 함수를 만드는 역할을 한다.
bind() 메소드의 this는 strict mode(엄격 모드)일 때와 아닐 때 차이를 보인다.

### 엄격 모드가 아닐 때

{% highlight javascript %}
function nonstrictSum(num) {
    return this + num;
};

const sumFive = nonstrictSum.bind(5);
console.log(sumFive(10)); // 15
console.log(sumFive(15)); // 20
{% endhighlight %}
### 엄격 모드 일때

{% highlight javascript %}
function strictSum(num) {
    'use strct'; // 엄격 모드 사용
    return this + num;
};

const sumFive = strictSum.bind(5);
console.log(sumFive(10)); // 15
console.log(sumFive(15)); // 20
{% endhighlight %}

### 각 모드에서 에러 발생시켜보기

{% highlight javascript %}
const calculater = {
    num: 0,
    sum: function(a) {
        this.num += a; // 여기에서의 this는 calculater를 가르켜서 이 객체안에 있는 num을 사용 할 수 있는 것이다.
        return this.num;
    },
    check: function() {
        //console.log(this === calculater); // true가 메소드를 소유하고 있는 객체를 의미하기에 출력 될 것입니다.
        console.log(this);

        return this === calculater;
    },
}

const test = calculater.check.bind(calculater) // true
console.log(test()); // calculater, 객체 true
const er = calculater.check;
console.log(er()); // global 객체, false. 만약 엄격 모드 였다면 1번의 규칙에 따라 error를 발생시켰을 것이다.
{% endhighlight %}
아까 2번에서 사용했던 코드를 조금 변경하여 재사용 해보았다. 결과는 옆에 주석으로 달아두었다.

## 7번 - 화살표 함수에서의 this
화살표 하무는 함수를 간단한 형태로 정의하거나 혹은 문백을 바인드 하기 위해 주로 사용된다.

### 일반적인 함수를 사용 했을 때

{% highlight javascript %}
let person = {
    name: "youngjin",
    getName: function () {
        console.log(this) // person 객체를 가리킨다
        setTimeout(function () {
          console.log(this) // window 객체를 가리킨다
          console.log(this.name); // undefined window객체안에 name이 없음으로!
      }, 1000);
  }
}

person.getName();
{% endhighlight %}

### 화살표 함수를 사용 했을 때

{% highlight javascript %}
let person = {
    name: "youngjin",
    getName: function () {
        console.log(this) // person 객체를 가리킨다
        setTimeout(() => {
          console.log(this) // person객체를 가리킨다
          console.log(this.name); // youngjin 을 출력한다.
      }, 1000);
  }
}

person.getName();
{% endhighlight %}

## Reference
1. https://github.com/FEDevelopers/tech.description/wiki/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%97%90%EC%84%9C-%EC%82%AC%EC%9A%A9%EB%90%98%EB%8A%94-this%EC%97%90-%EB%8C%80%ED%95%9C-%EC%84%A4%EB%AA%85-1
2. https://jsdev.kr/t/this-this/3285
3. https://velog.io/@pa324/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-this
4. https://velog.io/@ki_blank/JavaScript-%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98Arrow-function

## 끝으로
댓글 혹은 이메일을 통해 피드백 받습니다. 혹시 예시가 더 필요하시면 제가 추가적으로 올리도록 하겠습니다.
