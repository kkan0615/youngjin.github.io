---
layout: post
title:  "[Vue Js] v-show와 v-if 차이점 with 예시"
image: ''
date:   2019-12-21 00:06:31
tags:
- Vue Js
- javascript
description: 'Learn v-show v-if!'
categories:
- Vue Js
- javascript
---

## 조건부 렌더링 if 와 show
둘의 공통점은 조건부를 렌더링할 때 사용한다는 것입니다.
그러나

조건 || 설명 || 차이점 || 사용처
v-if || 조건 블록이 true 가 될 때까지 렌더더링 되지 않는다. || 토글 비용이 높다. || 런타임시 조건이 변경이 안될 때
v-show || css 기반 토글이만으로 초기 조건 관계 없이 엘리먼트가 항상 렌더링 || 초기 렌더링 비용이 높다 || 자주 바뀐다면 사용

## v-if 의 종류

종류 || 방법 || 설명
v-if || v-if="condition" || 조건이 충족되면 작동합니다.
v-else-if || v-else-if="condition" ||if 다음에 사용되면 추가 조건이 들어옵니다
v-else || v-else || 모든 조건이 충족되지 않을 때 작동합니다.

## 예시 1번
{% highlight javascript %}
<template>
    <div class="content">
        <p v-show="description">open을 입력하시면 문이 열립니다.열립니다!</p>
        <input v-model="message" type="text">
        <p>Door condition: </p>
        <p v-if="message === 'open'">open</p>
        <p v-else-if="message === 'secret'">You found secret!</p>
        <p v-else>close</p>
        <input type="button" value="설명 열닫기" @click="descriptionClose()">
    </div> <!-- content end -->
</template>

<script>
export default {
    data() {
        return {
            message: '',
            description: true,
        }
    },
    methods: {
        descriptionClose() {
            if(this.description)
                this.description = false;
            else
                this.description = true;
        }
    },
}
</script>

<style lang="scss" scoped>

</style>
{% endhighlight %}

### 설명
간단하다 message에 입력 값에 따라 if문이 작동된다.
v-show는 버튼을 클릭 할 때마다 사라졌다 생겼다한다.

#### 예시 1번 문제점
v-show에 경우는 굉장히 괜찮은 예제이지만 v-if의 경우 값이 자주 변경되여 좋은 예제는 아니다. 물론 else를 사용하기에 괜찮을 수도 있다.

v-if의 경우는 예시 2번이 적절하다

## 예시 2번 - 로그인 사용자 확인
{% highlight javascript %}
<template>
            <div class="dropdown" v-if="me">
                <button @click="onLogOut" v-if="me" >로그아웃</button>
            </div>
            <div class="dropdown" v-else>
                <button class="dropbtn">
                    로그인하기
                </button>
                <div class="dropdown-content">
                    <nuxt-link to="/login">로그인</nuxt-link>
                    <nuxt-link to="/signUp">회원가입</nuxt-link>
                </div>
            </div>
</template>

<script>
export default {
    data() {
        return {
        };
    },
    computed: {
        me() {
            return this.$store.state.users.user;
        }
    },
}
</script>
<style lang="scss" scoped>
</style>
{% endhighlight %}

### 설명
이 예제는 지금 개인 프로젝트로써 준비중인 사이트이다.
v-if를 사용하여 로그인한 사용자의 경우는 로그아웃을 보여주고 비로그인 사용자는 로그인과 회원가입을 보여준다.
로그인 유저와 아닌사람은 페이지 렌더링 이후 변경이 자주 되지 않는다. 이럴 경우 v-if가 적절하다.

## v-if 사용시 주의할점
1. v-else-if 와 v-else는 v-if가 없을시 에러를 발생시킨다.
2. v-for과 v-if 같이 사용시 v-for이 더 높은 우선순위를 가진다.

## Reference
1. https://kr.vuejs.org/v2/guide/conditional.html

## 끝으로
댓글 혹은 이메일을 통해 피드백 받습니다. 혹시 예시가 더 필요하시면 제가 추가적으로 올리도록 하겠습니다.
