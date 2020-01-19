---
layout: post
title:  "[Javascript] formData 생성하기"
image: ''
date:   2020-01-08 00:06:31
tags:
- javascript
description: 'Learn formdata with javascript js!'
categories:
- javascript
---

## form data
주로 JSON 파일을 이용하여 서버와 통신하지만, 상황에 따라 Formdata가 필요한 경우가 있다.

## 예제 코드
axios와 함께 데이터 전송해보기
{% highlight javascript %}
const formData = new FormData();
/** append image into form */
formData.append('image', this.imageFile);
this.$axios.post('url', formData, {
    withCredentials: true,
}).then((res) => {
    console.log(res);
}).catch((error) => {
    console.error(error);
});
{% endhighlight %}

### 설명
FormData()를 이용하여 새로운 인터페이슬 생성한다.
append 메소드를 이용하여 image라는 키로 새로운 value을 만든다.

## 메소드들
메소드 || 설명
append() ||  FormData 객체안에 이미 키가 존재하면 그 키에 새로운 값을 추가하고, 키가 없으면 추가합니다.
delete() || FormData 객체로부터 키/밸류 쌍을 삭제합니다.
entries() || 이 객체에 담긴 모든 키/밸류 쌍을 순회할 수 있는 iterator를 반환합니다.
get() || FormData 객체 내의 값들 중 주어진 키와 연관된 첫번째 값을 반환합니다.
getAll() || FormData 객체 내의 값들 중 주어진 키와 연관된 모든 값이 담긴 배열을 반환합니다.
has() || FormData 객체에 특정 키가 포함되어 있는지 여부를 나타내는 boolean 을 반환합니다.
keys() || 이 객체에 담긴 모든 키/벨류 쌍들의 모든 키들을 순회 할 수 있는 iterator를 반환합니다.
set() || FormData 객체 내에 있는 기존 키에 새 값을 설정하거나, 존재하지 않을 경우 키/밸류 쌍을 추가합니다.
values() || 이 객체에 포함된 모든 밸류를 통과하는 iterator를 반환합니다

## Reference
1. https://developer.mozilla.org/ko/docs/Web/API/FormData [MDN Web docs]

## 끝으로
댓글 혹은 이메일을 통해 피드백 받습니다. 혹시 예시가 더 필요하시면 제가 추가적으로 올리도록 하겠습니다.
