---
layout: post
title:  "[C++] 👰 C++ 입출력 처리에 알아두면 좋은 것들><"
date:   2020-02-25 18:34:10 +0700
categories: [C++]
---

## 👰 입력, 출력 함수 속도 높이기
---

~~~c++
#include<iostream>
using namespace std;

int main(){
	ios::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);
}
~~~

입력할 데이터나 출력할 데이터가 많을 때에는 c++의 느린 입출력 함수 때문에 시간초과가 날 수 있다.

그럴 때는 위의 두 코드를 추가로 입력해서 입출력 함수의 속도를 높이자!

또 c++에서 개행할 때 사용되는 endl도 속도가 느린편이므로 endl 대신

~~~c++
#include<iostream>
using namespace std;

int main(){
	cout<<'\n';
}
~~~

와 같이 개행문자인 \n 을 사용하도록 하자!

<br>