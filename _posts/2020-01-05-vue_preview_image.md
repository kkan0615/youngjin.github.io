---
layout: post
title:  "[Vue] Preview image 이미지 미리보기 구현 "
image: ''
date:   2020-01-05 00:06:31
tags:
- vue js
- javascript
description: 'Learn Vue js'
categories:
- javascript
- vue js
---

## 이미지 미리보기 구현하기
이미지를 넣으면 이미지를 미리보여주는 방식을 사용할 것이다. 주로 서버와 같이 이용되기도 하는데
이번에는 서버없이 사용하는 법을 보여줄 것이다.

## 코드

### template
{% highlight javascript %}
<input ref="imageInput" type="file" hidden @change="onChangeImages">
<v-btn type="button" @click="onClickImageUpload">이미지 업로드</v-btn>
<v-img
    v-if="imageUrl" :src="imageUrl"
></v-img>
{% endhighlight %}

#### 설명
v-btn은 vuetify로 button과 동일하니 모르셔도 상관없다.
v-img 역시 vuetify의 문법중 하나로 img와 동일하게 작동한다.

### script
{% highlight javascript %}
data() {
    return {
        imageUrl: null,
    }
},

// methods
onClickImageUpload() {
    this.$refs.imageInput.click();
},
onChangeImages(e) {
    console.log(e.target.files)
    const file = e.target.files[0]; // Get first index in files
    this.imageUrl = URL.createObjectURL(file); // Create File URL
}
{% endhighlight %}

#### 설명
imagUrl data에 URL 주소가 들어간다
onChangeImages(e)에는 e(event)를 무조건 파라미터로 받아주어야한다.
URL에 함수인 createObjectURL를 이용하여 파일의 객체 주소를 만들어주면된다.

### 전체 코드
{% highlight javascript %}
<template>
    <input ref="imageInput" type="file" hidden @change="onChangeImages">
    <v-btn type="button" @click="onClickImageUpload">이미지 업로드</v-btn>
    <v-img
        v-if="imageUrl" :src="imageUrl"
    ></v-img>
</template>

<script>
export default {
    data() {
        return {
            imageUrl: null,
        }
    },
    methods: {
        onClickImageUpload() {
            this.$refs.imageInput.click();
        },
        onChangeImages(e) {
            console.log(e.target.files)
            const file = e.target.files[0];
            this.imageUrl = URL.createObjectURL(file);
        }
    },
}
</script>

<style lang="scss" scoped>
</style>
{% endhighlight %}

## Reference
1. https://stackoverflow.com/questions/49106045/preview-an-image-before-it-is-uploaded-vuejs [Stack Overflow]

## 끝으로
댓글 혹은 이메일을 통해 피드백 받습니다. 혹시 예시가 더 필요하시면 제가 추가적으로 올리도록 하겠습니다.
