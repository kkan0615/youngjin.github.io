---
layout: post
title:  "Vue js Tiptap 사용법"
image: ''
date:   2019-1-20 00:06:31
tags:
- Vue Js
description: ''
categories:
- Vue Js
---

## Tiptap 이란?
Tiptap 은 현재 Vue js 생태계에서 인기있는 ckeitor 중에 하나이다. tiptap은 다음과 같은 장점과 단점을 가지고 있따.

### 장점
1. 무료이다.
2. 사용 방법이 쉽다.

### 단점
1. 모든 것들을 개발자가 만들어야한다.

### 관련 사이트
메인 홈페이지 : <a href="https://tiptap.scrumpy.io/">소개사이트</a>
Github 페이지 : <a href="https://github.com/scrumpy/tiptap">Github</a>

## Tiptap 사용법
사용법은 github 와 홈페이지의 내용을 번역한 것입니다. 자세한 사항은 홈페이지에서 보실 수 있습니다.

### 설치
{% highlight bash %}
    npm i tiptap
{% endhighlight %}

### 예시 코드
<a href="https://tiptap.scrumpy.io/">여기</a>를 클릭한 후 SHOW CODE 버튼을 누르면 각각의 vue 파일로 이동한다.
Github 예제 코드

{% highlight javascript %}
    <template>
        <editor-content :editor="editor" />
    </template>

    <script>
        // Import the editor
        import { Editor, EditorContent } from 'tiptap'

        export default {
        components: {
            EditorContent,
        },
        data() {
            return {
            editor: null,
            }
        },
        mounted() {
            this.editor = new Editor({
            content: '<p>This is just a boring paragraph</p>',
            })
        },
        beforeDestroy() {
            this.editor.destroy()
        },
    }
    </script>
{% endhighlight %}

### Extensions 사용하기
    기본으로 제공해주는 것만으로는 우리가 원하는 것들 얻기 힘들다

### 설치
{% highlight bash %}
    npm i tiptap-extensions
{% endhighlight %}

### 사용법
{% highlight javascript %}
    <template>
        <div>
            <editor-menu-bar :editor="editor" v-slot="{ commands, isActive }">
                <button :class="{ 'is-active': isActive.bold() }" @click="commands.bold">
                Bold
                </button>
            </editor-menu-bar>
            <editor-content :editor="editor" />
        </div>
    </template>

    <script>
    import { Editor, EditorContent, EditorMenuBar } from 'tiptap'
    import {
        Blockquote,
        CodeBlock,
        HardBreak,
        Heading,
        OrderedList,
        BulletList,
        ListItem,
        TodoItem,
        TodoList,
        Bold,
        Code,
        Italic,
        Link,
        Strike,
        Underline,
        History,
    } from 'tiptap-extensions'

    export default {
    components: {
        EditorMenuBar,
        EditorContent,
    },
    data() {
        return {
            editor: null,
        }
    },
    mounted() {
            this.editor = new Editor({
                extensions: [
                    new Blockquote(),
                    new CodeBlock(),
                    new HardBreak(),
                    new Heading({ levels: [1, 2, 3] }),
                    new BulletList(),
                    new OrderedList(),
                    new ListItem(),
                    new TodoItem(),
                    new TodoList(),
                    new Bold(),
                    new Code(),
                    new Italic(),
                    new Link(),
                    new Strike(),
                    new Underline(),
                    new History(),
                ],
                content: `
                    <h1>Yay Headlines!</h1>
                    <p>All these <strong>cool tags</strong> are working now.</p>
                `,
            })
        },
    beforeDestroy() {
        this.editor.destroy()
    },
    }
    </script>
{% endhighlight %}

## 끝으로
굉장히 다양한 예시가 github에 올라와 있어서 사실 단점에 썼던 내용이 아닐 정도이긴하다. 그러기에 tiptap 을 사용하자 !

피드백과 질문은 댓글 혹은 이메일로 받습니다.