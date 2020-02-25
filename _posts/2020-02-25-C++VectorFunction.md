---
layout: post
title:  "[C++] 💁 C++ Vector 처리에 자주 사용되는 함수들 모음집><"
date:   2020-02-25 18:34:10 +0700
categories: [C++]
---

> Vector를 사용해서 문제를 풀면서 자주 사용하는 함수들 or 코드 라이브러리들을 모아놓는 곳!
>
> 문제를 풀면서 지속적으로 업데이트 될 수 있음!
>
> vector에 관한 자세한 정보는 [Vector cpluscplus 사이트](http://www.cplusplus.com/reference/vector/vector/)를 참조하자!

<br>

## 💁 Vector 생성하고 push_back 으로 값 넣기
---

~~~c++
#include<vector>
using namespace std;

int main(){

	// 테스트 케이스 개수 N 만큼의 배열 크기가 생성됨
	int N;
	cin>>N;
	vector<string> vec;
	for(int i = 0 ; i < N ; i++){
		string tmp;
		cin>>tmp;
		vec.push_back(tmp);
	}
} 
~~~

<br>

## 💁 vector로 만든 배열 sort() 함수로 정렬시키기
---

sort() 함수의 특징은 인자로 정렬할 배열의 구간 값을 써줘야 한다는 점이다.

이 때, 구간 값을 배열의 주소로 적어줘야 하는데 vector에서는 이것을 iterator 이라는 것을 통해 포인터처럼 사용해서 적어줄 수 있다!

즉, vec라는 배열의 처음부터 끝까지에 있는 원소를 정렬하고 싶을 때는

~~~c++
#include<vector>
#include<algorithm>
using namespace std;

int main(){
	vector<int> vec;

	// vec에 원소를 push_back 해줬다고 가정하자.
	sort(vec.begin(), vec.end());
}
~~~

이렇게 해주면 된다.

여기서 begin()과 end()는 각각 벡터의 처음과 끝을 가리키는 iterator 이다.

<br>
