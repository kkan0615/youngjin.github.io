---
layout: post
title:  "[Vue] Vue로 Typescript 사용하기"
image: ''
date:   2020-01-01 00:06:31
tags:
- typescript
- vue js
description: 'Learn Vue js with typescript'
categories:
- typescript
- vue js
---

## Typescript
이 글은 typescript의 문법을 어느 정도 숙지한 상태에서 진행이 됩니다.
그러나 자바스크립트와 유사한 부분이 많기 때문에 보시면서 공부하셔도 됩니다.

타입스크립트 배우기 <a href="https://www.typescriptlang.org/docs/home.html">여기</a>를 클릭해주세요!

## Vue-cli로 시작하기
2가지 방법이 있습니다.
{% highlight bash %}
vue create [project-name] # 1번 프로젝트르 시작하기
vue add typescript # 2번 이미 프로젝트가 있다면
{% endhighlight %}

### 추가설명
1번방법으로 시작하시면
<img src="https://d33wubrfki0l68.cloudfront.net/7c73ee1cfe78f0cf3ddabcef98340c223bd56a8b/87f2d/images/vuejs/using-typescript-with-vue/vue-cli-3.png">

## 설치 후 디렉토리
설치 후 여러 파일과 폴더를 볼 수 있습니다. 설명들어갑니다.
대체적으로 vue js와 비슷한 부분은 뺏습니다.

파일명와 위치 || 설명
tsconfig.json || typescript와 관련된 설정파일입니다.
src/maints || main.js를 typescript와 한것입니다.
src/router/index.ts || router의 index.js와 역할이 동일합니다.

단순히 말해서 tsconfig.json파일이 생겼으며,
js -> ts로 바뀐 것들이 대부분입니다.

## vue에서 ts문법 사용해보기.
공식 사이트에 의하면 두 가지 방법이 존재합니다.

### 1번
{% highlight javascript %}
<template>
  <div>
    {{ result }}
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  data() {
    return {
      result: '',
    }
  },
  methods: {
    sayHi(someone: string) {
      this.result = 'hello ' + someone;
    }
  },
  created() {
    this.sayHi('world');
  },
})
</script>
{% endhighlight %}

### 2번
class component를 사용해서 만드는 법입니다.
{% highlight javascript %}
<!-- https://github.com/vuejs/vue-class-component -->
<template>
  <div>
    {{ result }} <br>
    {{ computedResult }}
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
// Prop는 사용하지 않았으나 사용하고싶을시 코멘트확인해주세요
import { Component, Prop } from 'vue-property-decorator';

@Component({
    /*
    1번방법
    props: {
        propMessage: String
    }
    */
    name: 'test2',
})
export default class App extends Vue {
    // 2번 방법 @Prop() private msg!: string;
    // data입니다.
    result = 'name will be in here';

    // method 입니다.
    sayHi(someone: string) {
        this.result = 'Hello ' + someone;
    }

    // Liftcycle중 하나인 created입니다.
    created() {
        this.sayHi('World 2');
    }

    // computed 입니다.
    get computedResult () {
        return 'computed ' + this.result;
    }
}
</script>
{% endhighlight %}

## Reference
1. https://alligator.io/vuejs/using-typescript-with-vue/ [Dave Berning]
2. https://github.com/vuejs/vue-class-component [Github offical page]
3. https://kr.vuejs.org/v2/guide/typescript.html [Vue js offical page]

## 끝으로
댓글 혹은 이메일을 통해 피드백 받습니다. 혹시 예시가 더 필요하시면 제가 추가적으로 올리도록 하겠습니다.
