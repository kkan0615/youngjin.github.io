---
layout: post
title:  "C++의 포인터(Pointer) vs 레퍼런스(Reference)"
image: ''
date:   2019-10-17 20:00:00
tags:
- C++
description: 'Describe difference 포인터(Pointer) between 레퍼런스(Reference)'
categories:
- C++ Pointer and refernece
---

## 들어가기에 앞서
캐나다에서 공부 중이라 한국어로 번역이 조금 어색 할 수 있습니다. 밑은 단어 사전입니다.
배열 (Array), 객체(Object), 포인터(Pointer)

## 배열, 포인터, 레퍼런스 기본 의미
### 배열(Array)
하나 혹은 그 이상의 연속적으로 메모리에 저장된 객체 나 같은 type의 집합체이다.
It is a collection of one or more objects of same type that are stored contiguously in memory

1. 배열의 요소는 0부터 시작하며 unsgined unique 정수이다.
2. 배열는 []로 연다.

{% highlight c++ %}

double x[10];
int data[100][200];

{% endhighlight %}

### 포인터(Pointer)
포인터는 다른 객체가 저장되어있는 메모리안의 주소인 객체이다.

1. Null Pointer가 허용 어떠한 명백한 주소 위치를 가르치지 않는다. nullptr 키워드 사용.
2. Dereference를 통해 포인터의 값을 얻을 수 있다.
3. cout << p 는 주소가 나오고 cout << *p 는 값이 나온다.

{% highlight c++ %}

char c;
char* cp = nullptr; // cp is pointer to char
char* cp2 = &c; // cp2 is pointer to char
T* x = &x; // T 포인터를 가지는 객체의 주소

{% endhighlight %}

### 레퍼런스(Reference)

1. 이미 존재하는 객체에 별명(alias) 이다.
2. 레퍼런스 초기화를 레퍼런스 binding 이다.
3. T&, T&& 이라는 두가지 타입의 레퍼런스가 존재한다.
4. 무조건 선언과 동시에 초기화를 해주어야한다.
5. cout << &rep 는 주소가 나오고 cout << rep 는 값이 나온다. (int &rep = a, a = 30일 때).

{% highlight c++ %}
int x;
int& y = x; // y is reference to int
{% endhighlight %}

## 포인터와 레퍼런스의 차이.

1. 레퍼런스는 무조건 어떤 것이든 참조해야하지만, 포인터는 NULL을 가질 수 있다(nullptr)
2. 레퍼런스는 새로운 값을 가질 수 없으나, 포인터는 바뀔 수 있다.
3. 레퍼런스는 포인터보다 깔끔한 syntax를 가지고있다.
4. 포인터의 사용은 필요한 메모리를 관리한다는 것을 암시한다. 메모리 관리는 잘못하면 상당한 버그를 발생시킨다.
5. 포인터는 주로 null 혹은 바뀔 수있는 경우에 사용한다.

## 몇가지 퀴즈

1. ++pChar 혹은 ++pArray 가 맞는 표현인지 생각해보세요.
{% highlight c++ %}
char pArray[] = { 'a','b','c','d','e'}; char* pChar = pArray;
{% endhighlight %}
++pChar은 가능하지만, ++pArray는 불가능합니다. pChar은 현재 a를 가리키고있고, ++ 사용하면 b를 가리키고있다.

2. ++*p의 값, --q의 값, **r은 무엇일지 생각해보세요.
{% highlight c++ %}
int a = 5, b = 7; int *p = &b; int &q = a; int** r = &p;
{% endhighlight %}
답: ++*p: 8 , --q: 5, **r: int

## 끝으로
댓글, 이메일 등을 통해 언제든지 질문과 피드백 받습니다. 감사합니다.