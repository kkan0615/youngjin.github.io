---
layout: post
title:  "[Node Js] Url 가져오기"
image: ''
date:   2019-12-22 00:06:31
tags:
- node js
- javascript
- network
description: 'Learn url with node jss'
categories:
- node js
- javascript
- network
---

## URL 이란

## URL 형식
http://www.example.com:8080/calendar?year=2019&month=12

### 설명
http://: 프로토콜입니다 간혹 https:// 형식도 보실텐데 이는 secure 보완이 추가된 형태라고 이해하시면 됩니다.
www.example.com: url 주소 , 웹 서버 입니다.
8080: port 넘버입니다.
calendar: 경로입니다. 이 뒤에 index.html 등 이 달린 경우도 있을 텐데 이는 파일 명입니다.
?뒤에: 쿼리입니다. 추가 적인 정보를 나타냅니다. & 를 이용하여 여러개의 query를 얻을 수 있습니다.

## parameter 파라미터
파라미터는 값이 계속해서 변경될 수 있습니다. 다음과 같은 주소를 봅시다.
http://localhost:3000/products/3
위 주소에서의 3이 파라미터입니다. 저 숫자에 따라 데이터베이스에서 가져오는 product가 다르게 가져옵니다. 주로 게시글 같은데에
사용함으로 대부분의 사이트에서 예제를 찾아 볼 수 있습니다.

### 예시 1번
예시 1번에서는 url 모듈을 이용하여 url 주소를 분석해보겠습니다. 결과값은 옆에 주석으로 달아두었습니다.
{% highlight javascript %}
const url = require('url');
const address = 'http://www.example.com:8080/calendar.html?year=2019&month=december';
const parsedData= url.parse(address, true);

console.log(parsedData.host); //return 'www.example.com:8080'
console.log(parsedData.pathname); //return '/calendar.html'
console.log(parsedData.search); //return '?year=2019&month=december'

const queryData = parsedData.query; //return { year: 2019, month: 'december' }
console.log(queryData.month); //return 'december'
{% endhighlight %}

### 예시 2번
2번은 req를 사용하여 query와 parameter를 가져오겠습니다.
{% highlight javascript %}
/* 들어오게 되는 값 : localhost:8080/products/3?q=option */
rotuer.get('/products/:id', (req, res, next) => {
    const { id } = req.params; // const id = req.params.id;
    const { q } = req.query; // const q = req.query.q;
    /* some codes in here ... */
});
{% endhighlight %}

아주 간단합니다. req.params를 사용시 parameter를 가져오고 req.query를 사용시 query 값을 가져옵니다.

## Reference
1. https://www.w3schools.com/nodejs/nodejs_url.asp [w3school]

## 끝으로
댓글 혹은 이메일을 통해 피드백 받습니다. 혹시 예시가 더 필요하시면 제가 추가적으로 올리도록 하겠습니다.
