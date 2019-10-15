---
layout: post
title:  "C++의 네임스페이스(namespace)"
image: ''
date:   2019-10-13 20:00:00
tags:
- C++
description: 'Describe Class of C++'
categories:
- C++ Class
---

## 들어가게 앞서
해외에서 공부를 하여서 한글로 번역하였습니다. 몇개의 단어는 한글을 몰라 영어와 같이 표기했습니다.
namespace는 c++에서 필수적으로 알아야할 개념입니다. 꼭 직접실행하면서 확인해보세요.

## namespace
변수, 함수, 구조체, 클래스 등을 서로 구분하기위해서 사용되는 내부 식별자(identifier)입니다.
### namespace 사용 이유
1. 서로 다른 라이브러리에서 발생할 수 있는 이름 충돌을 방지할 수 있습니다.
2. 코드를 모듈화할 수 있습니다.
3. 익명 namespace를 사용할 수 있습니다.(같은 이름의 namespace 사용이 허용됩니다. 이는 예제 4번에서 확인해주세요)
4. enum에서 발생할 수 있는 문제를 해결할 수 있습니다.
5. 관습에 의한 정보를 숨길 수 있습니다.


### 코드 예제
{% highlight c++ %}

// 예제 1
namespace test
{
    void hamsu(); // Original of function
    int beonsu; // Declare variables.
}
// 아래와 같은 방법으로 접근가능
test::hamsu();
test::beonsu = 5;

{% endhighlight %}

## using directive(지시자) && using delcaration(선언)
범위 지정 연산자를 사용 하지 않고도 사용 할 수 있도록 합니다. 선언 방법은 아래와 같습니다.
{% highlight c++ %}

// 예제 1 이어서
// test의 모든 properties에 범위 지정 연사자를 사용 하지 않겠다는 의미 입니다.
using namespace test;
hamsu();
beonsu = 5;

// 아래 방식은 beonsu 변수만 범위 지정 연사자를 사용 하지 않겠다는 의미입니다.
using namespace test::beonsu;
test::hamsu();
beonsu = 5;

{% endhighlight %}

주로 std에 다음과 같이 사용 합니다
{% highlight c++ %}
using namespace std;
{% endhighlight %}

std에 using 지시자를 넣는 것은 중요합니다. 왜냐하면 namespace를 사용 하지 않는 private들을 이용할 수 없게 되기 때문입니다.

## namespace 어디까지 접근이 가능한가
다음 예시를 잘 살펴보고 답을 유추해보세요.
{% highlight c++ %}

#include <iostream>
using namespace std;

namespace test
{
	int value = 5; // namespace variable
}

int value = 1; // Global variable

int main()
{
	int value = 8; // local variable
	cout << "value: " << value << endl; // 1번
	cout << "value: " << test::value << endl; // 2번
    using namespace test;
	cout << "value: " << value << endl; // 3번
	return 0;
}

{% endhighlight %}

### 1번
1번에서는 우리는 "value: 8"을 얻을 수 있다. global 보다 local이 우선이기 때문이다.

### 2번
우리는 test의 namespace에서 value를 얻겠다고 compiler 에게 알려 주었으니 "value: 5"가 나온다.

### 3번
우리는 using 지시자를 사용 했으나 local 변수가 우선임으로 "value: 8"이 나옵니다.

## nested namespace 사용하기

{% highlight c++ %}

#include <iostream>
using namespace std;
namespace test
{
	int data1 = 5;
	namespace test2
	{
		int data1 = 1;
	}
}

int main()
{
	cout << "top data: " << test::data1 << endl; // data 5
	cout << "child data: " << test::test2::data1 << endl; // data 1
	using namespace test;
	cout << "child data: " << test2::data1 << endl; // data: 1
	cout << "top data: " << data1 << endl; // data: 5
	//using namespace test::test2;
	//cout << "child data: " << data1 << endl; 불가능
	return 0;
}

{% endhighlight %}

### 불가능한 이유
data1의 변수이름이 충돌하기 때문입니다.

## 같은 이름을 가지는 namespace
다음 예제 코드입니다. 주석 output을 확인하실 수 있습니다
{% highlight c++ %}

#include <iostream>
using namespace std;

namespace test
{
	int data1 = 10;
}

namespace test
{
	int data2 = 5;
}

int main()
{
	cout << "data: " << test::data1 << endl; // 10
	cout << "data: " << test::data2 << endl; // 5
	using namespace test;
	cout << "data: " << data1 << endl; // 10
	cout << "data: " << data2 << endl; // 5
	return 0;
}

{% endhighlight %}
그러나 다음 과 같은 행위는 불가능합니다.
{% highlight c++ %}

#include <iostream>
using namespace std;

namespace test
{
	int data1 = 10;
}

namespace test
{
	int data1 = 5;
}

int main()
{
	cout << "data: " << test::data1 << endl;
	cout << "data: " << test::data1 << endl;
	using namespace test;
	cout << "data: " << data1 << endl;
	cout << "data: " << data1 << endl;
	return 0;
}

{% endhighlight %}

## 끝으로
댓글, 이메일 등을 통해 언제든지 질문과 피드백 받습니다. 감사합니다.