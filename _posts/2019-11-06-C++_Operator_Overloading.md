---
layout: post
title:  "C++ 연산자 오버로딩 Operator Overloading"
image: ''
date:   2016-03-12 00:06:31
tags:
- c++
description: ''
categories:
- c++ Operator Overloading
---

## 들어가기에 앞서
C++는 연산자들을 클래스에 맞게 바꿀 수 있다. 예제를 보고 생각해보자.

## 예제
{% highlight c++ %}
#include <iostream>
using namespace std;
//to .h
class Test {
private:
	int a;
	int b;
public:
	Test(int _a, int _b) { a = _a; b = _b; };
	// add with cin >>
	friend std::istream& operator>>(std::istream&, Test&);
	// output with cout <<
	friend std::ostream& operator<<(std::ostream&, Test&);
	friend Test operator+(Test const& t1, Test const& t2);
};
//to .cpp
std::istream& operator>>(std::istream& is, Test& test)
{
	cout << "Is passed" << endl;
	is >> test.a;
	is >> test.b;
	return is;
}

std::ostream& operator<<(std::ostream& os, Test& test)
{
	os << "a in test: "<< test.a << "\n";
	os << "b in test: " << test.b << "\n";
	return os;
}

Test operator+(Test const& t1, Test const& t2)
{
	return Test(t1.a + t2.a, t1.b + t2.b);
}
//to main
int main()
{
	Test t1(1, 1), t2(2, 2);
	Test t3 = t1 + t2;
	cout << t1 << endl; // 1, 1
	cout << t2 << endl; // 2, 2
	cout << t3 << endl; // 3, 3
	cin >> t1; // 5, 5을 입력 할 것입니다.
	cout << t1 << endl; // 5, 5
	return 0;
}
{% endhighlight %}

### 설명
안에 코드를 확인 어려운것은 없습니다. 단순히 operator 연산자를 friend 키워드와 함께 받아서 사용하는 것입니다. 실제로 사용시에는 header파일과 cpp파일을 분리한 후 사용 해주세요.

 <a href="http://courses.cms.caltech.edu/cs11/material/cpp/donnie/cpp-ops.html" target="_blank">여기</a>를 클릭하여 추가 예시들을 확인하 실수 있습니다.

## 가능한 연산자(operator)
1. = (assign opeartor 할당 연산자 )
2. + - * /, % (arithmetic operator 이진 산술 연산자 )
3. += -= *= /=, %= (복한 할당 연산자)
4. ==, != || &&, >, < (비교 연산자, comparsion opertor)
5. new, new[], delete, delete[]
6. <<, >>, <<=, >>=
7. (), []
사용 방법을 위와 동일합니다.

### 예시
{% highlight c++ %}
Test& Test::operator=(cont Test&);
{% endhighlight %}

## 불가능한 연산자(operator)
다음과 같은 연산자는 오버로딩이 불가능 합니다.
1. . (dot)
2. ::
3. ?:
4. sizeof

## 끝으로
어렵지는 않지만 OOP에서 중요하고 자주 만나게 되는 개념입니다.
댓글, 이메일 등을 통하여 피드백과 질문 받습니다. 감사합니다.