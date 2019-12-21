---
layout: post
title:  "[Vue js] Method Computed 그리고 Watch 설명과 사용법"
image: ''
date:   2019-12-21 00:06:31
tags:
- Vue Js
- javascript
description: 'Learn Meothod Computed Watch'
categories:
- Vue Js
- javascript
---

## Method, Computed, Watch

### 공통점과 차이점

#### 공통점
셋의 공통점은 데이터를 가공할 때 주로 사용된다는 것이다.
공통점은 많이들 알고 있고 이러한 공통점때문에 많은 사람을이 이 셋을 헷갈려한다.

#### 차이점
데이터를 가공하는 방식이 다르다. 주로 비교할 때는
method vs computed, computed vs watch로 비교한다

## Method vs Computed
### 공통점
vue js에서 함수를 정의하는데 사용 되며, 데이터가 데이터가 변동됨에 따라 안에 있는 함수를 재호출한다

### 차이점 테이블
방법 || 설명 || 사용분야
method || 랜더링을 다시 할 때마다 항상 함수를 실행 || 데이터 변경이 덜 할때 혹은 한번 정도 발생시 사용
computed || 이전의 계산된 값을 캐시에 저장해 두었다가 함수 호출시 다시 쓰게 됩니다. || 데이터 변경이 많을 시 사용

### 예시 1

{% highlight css %}
<template>
    <div class="content">
        <p>message: {{ message }}</p>
        <p>methodCount: {{ methodCount() }} </p> <!-- 함수 형태라는 거 잊지마세요 ! -->
        <p>computedCount: {{ computedCount }} </p> <!-- () 가 필요 없다는거 잊지마세요 !-->
        <input type="button" value="NewmethodsIncrease" @click="methodCounts++">
        <input type="button" value="NewmethodsDecrease" @click="methodCounts--"> <br>
        <input type="button" value="NewmethodsIncrease" @click="computedCounts++">
        <input type="button" value="NewmethodsDecrease" @click="computedCounts--"> <br>
        <input type="button" value="message" @click="changeMessageFromMethod()">
        <input type="button" value="another" @click="changeAnthodMessageFromMethod()">
    </div> <!-- content end -->
</template>

<script>
export default {
    data() {
        return {
            message: '',
            methodCounts: 0,
            computedCounts: 0,
        }
    },
    methods: {
        methodCount() {
            return this.methodCounts;
        },
        changeMessageFromMethod() {
            this.message = "Changed from method";
        },
        changeAnthodMessageFromMethod() {
            this.message = "Changed from new method";
        }
    },
    computed: {
        computedCount() {
            return this.computedCounts;
        },
    },
}
</script>

<style lang="scss" scoped>

</style>
{% endhighlight %}

#### 예시 1 설명
button을 클릭할 때마다 method 혹은 computed으 값이 바뀝니다.
methodcounts를 변경시에는 method를 다시 실행합니다.
그러나 computedcounts를 변경시에는 캐시에서 값을 가져와서 변경합니다.

#### 추가 설명
message관련한 것들은 method에만 들어 있는데 이유는 computed에서는 메소드처럼 사용 할 수 없습니다.

간단히 말해서 값이 자주 변경되야한다면 computed를 사용하는 것이 좋습니다.

### 예시 2번 - 변현된 코드 from 공식 사이트
{% highlight javascript %}
<template>
    <div class="content">
        <p>message: {{ message }}</p>
        <p>method: {{ reverseMethod() }}</p>
        <p>computed: {{ reverseComputed }}</p>
        <input type="button" value="Reset" @click="changeAnthodMessageFromMethod()"> <br>
        <input v-model="message" type="text">
    </div> <!-- content end -->
</template>

<script>
export default {
    data() {
        return {
            message: '',
            whatAreYouDoing: '',
        }
    },
    methods: {
        reverseMethod() {
            return this.message.split('').reverse().join('');
        },
        messageRest() {
            this.message = "Message has been cleared";
        },
    },
    computed: {
        reverseComputed() {
            return this.message.split('').reverse().join('');
        },
    },
}
</script>

<style lang="scss" scoped>

</style>
{% endhighlight %}

### 설명
설명의 예시 1번과 비슷하기 때문에 패스

## Computed vs Watch

### 공통점
값을 이용해서 계산한다.

### 차이점
방법 || 설명
computed || 특정 값을 계산한다.
watch || 특정 값을 변경해서 다른 작업을 한다.

사실 협업에서 watch는 주로 사용 하지 않는다. 물론 사용하는 곳이 있지만 고려해야 할 것들이 많다고한다.

단순히 말해서 watch는 값을 변경하고 함수를 실행한다고 보면된다.

### watch 예시
{% highlight javascript %}
<div id="watch-example">
    <p><input v-model="watchOut"></p>
    <p>{{ message }}</p>
</div>

watch: {
        watchOut: function (newQuestion) {
            this.message = '나는 watch에 있어요'
            this.getMessage()
        }
  },
{% endhighlight %}
예시만 봐도 무엇을 의미하는지 알 수 있을 것이다.

## Reference
1. https://kr.vuejs.org/v2/guide/computed.html
2. https://kr.vuejs.org/v2/guide/events.html
3. https://kamang-it.tistory.com/entry/Vue23computed-%EA%B7%B8%EB%A6%AC%EA%B3%A0-methods%EC%99%80%EC%9D%98-%EC%B0%A8%EC%9D%B4featwatch


## 끝으로
댓글 혹은 이메일을 통해 피드백 받습니다. 혹시 예시가 더 필요하시면 제가 추가적으로 올리도록 하겠습니다.