---
layout: post
title:  "[Python] 백준 11399"
image: ''
date:   2019-12-24 00:06:31
tags:
- python
- baekjoon
description: 'baekjoon'
categories:
- python
- baekjoon
---

## 백준 11399
문제 <a href ="https://www.acmicpc.net/problem/11399">링크</a>

## 접근 방식
가장 작은 순서대로 더하면 최소값으로 더해지기 때문에 배열을 만들고 sort 함수로 작은 수 부터 나열하고 더하면된다!


## 코드 1
시간복잡도: n^2 + n
{% highlight python %}
# 결과값
result = 0

# 인원수
people = input()

# 각 시간
time = list((map(int, input().split())))

# 작은 숫자 순으로 정렬
time.sort()

# 더하기 ! 시간복잡도는 n^2
for i in range(int(people)):
    for j in range(i + 1):
        result += time[j]

# print 결과 값
print(result)
{% endhighlight %}

## 끝으로
댓글 혹은 이메일을 통해 피드백 받습니다.
