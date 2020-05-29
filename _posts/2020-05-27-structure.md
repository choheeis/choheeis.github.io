---
layout: post
title:  "[C++] 🧠 우선순위 큐의 기준 바꾸기"
date:   2020-05-27 18:34:10 +0700
categories: [C++]
---

<br>

## 🧠 우선순위 큐에 구조체를 사용해서 우선순위 기준을 바꿀 수 있다?
---

[priority_queue reference](http://www.cplusplus.com/reference/queue/priority_queue/) 를 보면 

![13](https://user-images.githubusercontent.com/31889335/83017930-73998680-a05f-11ea-9f2b-226b063bfea1.PNG)

이렇게 priority_queue의 정의에 대해서 알 수 있다.

즉, 우선순위 큐를 실제로 정의할 때는 

~~~c++
#include<queue>
using namespace std;

priority_queue<int, vector<int>, less<int>> pq;

int main(){
	
}
~~~

이런 식으로 정의한다는 것이다.

가운데 vector는 우선순위 큐가 실제로는 vector를 사용해서 저장되기 때문에 사용되는 것이다.

마지막 Compare 클래스 부분은 우선순위 큐의 우선순위 기준을 정해주는 클래스이다.

이 설명으로 대충 감을 잡은 후, 실제 템플릿 인자들에 대한 아래 설명을 읽어보자.

![14](https://user-images.githubusercontent.com/31889335/83018264-f0c4fb80-a05f-11ea-9ff7-96d04f885559.PNG)

일단 템플릿 인자의 맨 첫 번째 T 는 priority_queue의 원소들의 자료형이라고 되어 있다.

즉, priority_queue 안에 int 형 외에도 다른 자료형의 데이터들을 넣을 수 있는 것이다.

템플릿의 두 번째 인자인 Container에 대해서 읽어보자.

priority_queue의 원소들이 저장되는 컨테이너라고 되어있다. c++ 에서 컨테이너 역할을 하는 것들은 

![15](https://user-images.githubusercontent.com/31889335/83019073-4bab2280-a061-11ea-9df8-e0e788924868.PNG)

이런 것들이 있는데 그 중 위 코드에서는 vector를 사용한 것이다.

템플릿의 마지막 원소인 Compare 에 대해서 읽어보자.

Compare은 binary 값인 true와 false를 통해 두 원소를 비교하는 기준을 정해줄 수 있다.

기본적으로는 less\<T> 가 Compare 자리에 들어가고 의미는 두 원소 a, b가 있을 때 정렬하는 기준이 a < b 라는 것이다. 따라서 연산자 < 기준이 기본으로 들어가 있으므로 다른 기준을 하고 싶다면 연산자 <를 오버로딩하면 된다.

<br>

따라서 우리는 T 를 정의해줄 수 있는데 여기서 T를 구조체라고 한다.

T를 만들어서 우선순위 큐의 기준을 바꿔보자!

~~~c++
#include<iostream>
#include<queue>
#include<utility>
#include<vector>
using namespace std;

struct Element{
	int first;
	int second;
};

bool operator<(Element a, Element b){
	return a.first > b.first;
}

int main(){
	priority_queue<Element, vector<Element>> pq;

	struct Element e;
	e.first = 1;
	e.second = 2;
	pq.push(e);
    
	e.first = 3;
	e.second= 4;
	pq.push(e);
	
	while(pq.empty() == false){
		cout<<pq.top().first<<'\n';
		pq.pop();	
	}
	
}
~~~

