---
layout: post
title:  "[알고리즘] 👩‍👩‍👦‍👦 Union Find 알고리즘"
date:   2020-05-04 18:34:10 +0700
categories: [알고리즘]
---

> [동빈나 Union-Find 영상](https://www.youtube.com/watch?v=AMByrd53PHM&list=PLRx0vPvlEmdDHxCvAQS1_6XV4deOwfVrz&index=18) 을 보고 공부한 내용입니다.

<br>

## 👩‍👩‍👦‍👦 Union-Find 란???
---

Union-Find(유니온 파인드) 알고리즘은 대표적인 __그래프 알고리즘__ 중 하나이다.

__합집합 찾기__ 라는 의미를 가진 알고리즘이며 그래프의 여러 개의 노드가 존재할 때 두 개의 노드를 선택해서 이 __두 노드가 서로 같은 그래프에 속하는지 판별__ 하는 알고리즘이다!

예를 들어 아래 그림과 같이 각 노드가 서로 연결되어 있지 않고 따로 따로 떨어져 있다고 해보자.

![01](https://user-images.githubusercontent.com/31889335/80918942-71b30f00-8da2-11ea-94e4-95cc16a47414.PNG)

이와 같은 모습을 컴퓨터 상에서 어떻게 나타낼 수 있을까?

![02](https://user-images.githubusercontent.com/31889335/80919039-cb1b3e00-8da2-11ea-9269-e1563365500a.PNG)

위와 같이 각 노드의 부모 노드가 자신으로 표현되는 배열로 나타낼 수 있을 것이다.

그렇다면 만약, 

![03](https://user-images.githubusercontent.com/31889335/80919066-f3a33800-8da2-11ea-932f-d38b49845561.PNG)

위 그림과 같이 노드 1과 노드 2가 연결되어 있는 상황을 컴퓨터 상에서 나타내려면 어떻게 나타낼 수 있을까?

![04](https://user-images.githubusercontent.com/31889335/80919118-4da3fd80-8da3-11ea-9b00-8303f15b92bd.PNG)

바로 위와 같이 노드 1과 노드 2 중 번호가 더 작은 1을 두 노드의 부모 노드로 설정하여 배열로 나타낼 수 있다.

일반적으로 이렇게 서로 다른 노드의 부모를 같게 만들어 줄 때는 더 작은 값 쪽으로 합치고, 이와 같은 작업을 __Union(합침)__ 이라고 부른다.

<br>

그렇다면 만약,

![05](https://user-images.githubusercontent.com/31889335/80919195-b9866600-8da3-11ea-8223-4c721df7445b.PNG)

이렇게 노드 1과 2와 3이 연결되어 있는 경우에는 컴퓨터 상에서 어떻게 표현할 수 있을까?

![06](https://user-images.githubusercontent.com/31889335/80919247-f2bed600-8da3-11ea-9134-e6f6aa68a9aa.PNG)

위와 같이 표현할 수 있다. 노드 1과 노드 2는 연결되어 있으므로 더 작은 값인 1을 부모 노드로 갖게 된다.

이어서 노드 2와 노드 3은 연결되어 있으므로 더 작은 값인 2를 부모 노드로 갖게 된다.

하지만 전체적으로 보았을 때는 노드 1, 2, 3이 모두 한 부모를 가지고 있는 모양이기 때문에 위와 같은 배열은 모순이 있게 된다.

이 때, 이러한 모순을 해결해주기 위해 __재귀 함수__ 가 사용된다.

어떤 부분에서 재귀 함수가 사용되는지 알아보자.

3의 부모 노드는 2가 아니라 1이라는 것을 어떻게 확인할 수 있을까?

3의 부모 노드로 선정된 노드 2는 어떤 노드를 부모로 가지고 있는지 확인해보면 된다.

노드 2는 노드 1을 부모로 가지고 있다.

따라서 또 한 번 노드 1은 어떤 노드를 부모로 가지고 있는지 확인하는 것이다.

노드 1은 자기 자신을 부모로 가지고 있다.

따라서 노드 3의 부모도 노드 1로 표현이 가능하다는 것을 알 수 있게 된다.

즉, 부모 노드를 진짜 부모 노드로 다시 한번 고쳐주기 위해 반복적으로 확인해주는 작업에서 재귀 호출이 사용되게 되는 것이다!

따라서 노드 1, 2, 3이 연결되어 있는 그래프는

![07](https://user-images.githubusercontent.com/31889335/80919394-b0e25f80-8da4-11ea-8965-7b369f345973.PNG)

이와 같이 표현되어야 한다.

여기까지가 제대로된 __Union(합침)__ 과정이다.

<br>

그렇다면 Union-Find에서 __Find__ 는 뭘까?

Find 는 어떤 두 개의 노드의 부모 노드를 확인하여 같은 그래프에 속해있는지 확인하는 것을 Find 라고 한다.

즉, 위 그래프에서 노드 4가 노드 3과 같은 그래프에 연결되어 있는 노드인지 확인하는 것을 Find 라고 하는 것이다.

<br>

## 👩‍👩‍👦‍👦 Union-Find 알고리즘을 코드로 구현해보자
---

필요한 함수는 총 3개이다.

1. getParent 함수

2. unionParent 함수

3. findParent 함수

각 함수의 기능을 아래 코드를 통해 확인해보자.

<br>

~~~c++
#include<iostream>
using namespace std;

// 진짜 부모 노드를 찾는 함수 
// 인자의 parent라는 배열은 그래프를 배열로 나타낼 때의 그 배열.
// 인자의 x 는 노드 번호.  
int getParent(int parent[], int x){
	if(parent[x] == x){
		// x의 부모 노드가 자기 자신인 경우 부모 노드(자기 자신) 반환 
		// 재귀 종료 조건 
		return x; 
	}else{
		// 재귀 호출로 부모 노드의 부모 노드를 재귀적으로 찾음.  
		return parent[x] = getParent(parent, parent[x]);
	}
}

// 부모 노드가 다른 두 노드를 연결해주는 함수 (그래프 상에서 두 노드를 연결해주는 선을 그어주는 함수)
int unionParent(int parent[], int a, int b){
	a = getParent(parent, a);
	b = getParent(parent, b);
	
	// 더 작은 노드로 부모 노드를 설정 	
	if(a < b){
		return parent[b] = a;
	}else{
		return parent[a] = b;
	}
} 

// 같은 부모를 가지는지 확인 (Find)
int findParent(int parent[], int a, int b){
	a = getParent(parent, a);
	b = getParent(parent, b);
	if(a == b){
		// 반환 값이 1이면 노드 a 와 노드 b가 같은 그래프라는 뜻. 
		return 1;
	}else{
		// 반환 값이 0이면 노드 a와 노드 b가 다른 그래프라는 뜻. 
		return 0;
	}
} 

int main(){
	// 총 10개의 노드를 표현하고자 함. 
	int parent[11]; 
	for(int i = 1 ; i <= 10 ; i++){
		// 먼저 모든 노드가 따로 떨어져 있게 만듬. 
		parent[i] = i; 
	}
	
	// 몇 개의 노드를 합쳐보자. 
	unionParent(parent, 1, 2); 
	unionParent(parent, 2, 3); 
	unionParent(parent, 3, 4); 
	unionParent(parent, 5, 6); 
	unionParent(parent, 6, 7); 
	unionParent(parent, 7, 8); 
	
	cout<<"1과 5는 연결되어 있나? : "<<findParent(parent, 1, 5);
}
~~~

위와 같은 코드의 결과는 


![08](https://user-images.githubusercontent.com/31889335/80920585-bb542780-8dab-11ea-9a20-c47667b7bb55.PNG)

이와 같을 것이다. 만약 위 코드에 

unionParent(parent, 1, 5)를 추가한 후 다시 컴파일 하면

![09](https://user-images.githubusercontent.com/31889335/80920610-dde64080-8dab-11ea-838d-d60c04cb0117.PNG)

이렇게 나올 것이다!

<br>