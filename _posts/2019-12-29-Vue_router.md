---
layout: post
title:  "[Vue] Vue js Router 사용법"
image: ''
date:   2019-12-31 00:06:31
tags:
- javascript
- vue js
- vue-router
description: 'Learn Vue js Router'
categories:
- javascript
- vue js
- vue-router
---

## Vue js router
vue 컴포넌트를 이용하여 싱글페이지 앱을 만듭니다. 라우트에 컴포넌트를 맵핑 한 후 어떤 url 주소에 랜더링 할지 알려주면 됩니다.

혹시 nuxt js를 이용하시면 <a href="#">여기</a>를 클릭해주세요!

## Vue Router 설치
{% highlight bash %}
npm i vue-router --save
{% endhighlight %}

## 예시 1번
{% highlight javascript %}
/**
* Router directory index.js
* Full path: src/router/index.js
*/
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import test from '@/components/test/test'
import test2 from '@/components/test/test2'
import test3 from '@/components/test/test3'

Vue.use(Router)

export default new Router({
  mode: 'history', // Url 주소에서 #과 같은 hash를 제거해주는데 사용. hash를 사용하고싶으면 'hash'로 변경
  routes: [
    {
      path: '/', // url path
      name: 'HelloWorld', // namesapce
      component: HelloWorld // 컴포넌트
    },
    {
      path: '/test',
      name: 'test',
      component: test,
        // children 사용시 /test/test2 로 접근가능하며 상위 데이터를 사용 할 수 있다.
        // <router-view></router-view> 를 사용해야 children 컴포넌트가 보인다. 자세한 건 밑에 참조
          children: [
            {
              path: 'test2',
              name: 'test2',
              component: test2
            }
          ]
      },
    {
      path: '/test/test3',
      name: 'test3',
      component: test3
    },
  ]
})

/**
* HelloWorld.vue
* scr/compnents/HelloWorld.vue
*/
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    <ul>
      <li>
        <a
          href="https://vuejs.org"
          target="_blank"
        >
          Core Docs
        </a>
      </li>
      <li>
        <a
          href="https://forum.vuejs.org"
          target="_blank"
        >
          Forum
        </a>
      </li>
      <li>
        <a
          href="https://chat.vuejs.org"
          target="_blank"
        >
          Community Chat
        </a>
      </li>
      <li>
        <a
          href="https://twitter.com/vuejs"
          target="_blank"
        >
          Twitter
        </a>
      </li>
      <br>
      <li>
        <a
          href="http://vuejs-templates.github.io/webpack/"
          target="_blank"
        >
          Docs for This Template
        </a>
      </li>
    </ul>
    <h2>Ecosystem</h2>
    <ul>
      <li>
        <a
          href="http://router.vuejs.org/"
          target="_blank"
        >
          vue-router
        </a>
      </li>
      <li>
        <a
          href="http://vuex.vuejs.org/"
          target="_blank"
        >
          vuex
        </a>
      </li>
      <li>
        <a
          href="http://vue-loader.vuejs.org/"
          target="_blank"
        >
          vue-loader
        </a>
      </li>
      <li>
        <a
          href="https://github.com/vuejs/awesome-vue"
          target="_blank"
        >
          awesome-vue
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>


/**
* test.vue
* src/componets/test/test.vue
*/
<template>
    <div class="test">
        <h1>{{ message }}</h1>
        <router-view></router-view> // 아까 위에서 언급한 부분
        <router-link to="/test/test3">test3</router-link> // <a href="">와 같은 역할이다. 다른 방법도 있으니 밑에 확인!
    </div>
</template>
<script>
export default {
    name: 'test',
    data() {
        return {
            message: 'Test page '
        }
    },
}
</script>

/**
* test2.vue
* src/componets/test/test2.vue
*/
<template>
    <div class="test2">
        count:
        <h1>{{ count }}</h1>
    </div class="test2">
</template>
<script>
export default {
    name: 'test2',
    data() {
        return {
            count : 0,
        }
    },
}
</script>

/**
* test3.vue
* src/componets/test/test3.vue
*/
<template>
    <div class="test3">
        <h1>{{ msg }}</h1>
    </div>
</template>
<script>
export default {
    name: 'test3',
    data() {
        return {
            msg: 'test3 입니다!',
        }
    },
}
</script>
{% endhighlight %}

## 뷰 인스턴스 생성
여기서 끝이 아니다.
scr 디렉토리에 App.vue와 main.js를 생성하자
{% highlight javascript %}
/**
* main.js
* src/main.js
*/
import Vue from 'vue'
import App from './App'
import router from './router'

Vue.config.productionTip = false

new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})

/**
* App.vue
* scr/App.vue
*/
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view/> // layout으로 사용되는 최상위 이기 때문에 무조건 필요하다. 컴포넌트들은 이를 통과한다.
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
{% endhighlight %}

### 파일 분리하기
{% highlight javascript %}
/**
* src/router/test/index.js
*/
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import test from '@/components/test/test'
import test2 from '@/components/test/test2'
import test3 from '@/components/test/test3'

Vue.use(Router)
// 모듈로 만들기
export default [
    {
        path: '/',
        name: 'HelloWorld',
        component: HelloWorld
    },
    {
        path: '/test',
        name: 'test',
        component: test,
            children: [
                {
                path: 'test2',
                name: 'test2',
                component: test2
                }
            ]
    },
    {
        path: '/test/test3',
        name: 'test3',
        component: test3
    },
]
/**
* src/router/index.js
*/
import Vue from 'vue'
import Router from 'vue-router'
import test from '../router/test';

Vue.use(Router)

export default new Router({
  mode: 'history',
  routes: [
    ...test, // ...을 무조건 붙여야한다.
  ]
})
{% endhighlight %}

## Router를 통해 페이지 이동하기
페이지를 이동하는 방법은 다양하다. 위의 예제들을 통해 적어보겠습니다.
{% highlight javascript %}
// 1번 absolute path를 사용
// 가장 간단한 방법이다.
<router-link to="/test/test3">test3</router-link>

// 2번 name을 사용
// to가 아닌 :to를 사용해야한다.
<router-link :to="{ name: 'test3' }">test3</router-link>

// 3번 router.push를 이용한 이동
// 이를 script란에 넣어준다.
this.$router.push({ name: 'test3' })
{% endhighlight %}

### 추가 설명
1번과 2번은 선언전 방식이라고 불린다. 주로 template에 사용된다.
3번은 프로그래밍 방식이라 불린다. 주로 methods에 사용된다.

## Reference
1. https://router.vuejs.org/kr/guide/#html [Vue Guide KR]
2. https://router.vuejs.org/guide/essentials/navigation.html [Vue Guide EN]
3. https://www.npmjs.com/package/vue-router [npm vue-router]
4. https://febdy.tistory.com/73 [집 tistory]
5. https://velog.io/@skyepodium/Vue-Router-fijr95dn4j [skyepodium velog]

## 끝으로
댓글 혹은 이메일을 통해 피드백 받습니다. 혹시 예시가 더 필요하시면 제가 추가적으로 올리도록 하겠습니다.
