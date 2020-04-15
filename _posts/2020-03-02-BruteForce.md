---
layout: post
title:  "[알고리즘] 🤖 Brute Force란!?"
date:   2020-03-02 18:34:10 +0700
categories: [알고리즘]
---

<br>

> [라이 블로그 - 완전 탐색](http://blog.naver.com/kks227/220769870195) 을 참고한 내용입니다.

<br>

## 🤖 Brute Force란??
---

위키백과에 따르면 Brute Force는 암호학에서 사용되었던 용어로, __무차별 대입 공격(Brute Force Attack)__ 이라고 하며 __특정한 암호를 풀기 위해 가능한 모든 값을 대입하는 것__ 을 말한다고 한다!

이러한 Brute Force의 의미가 알고리즘에서도 사용되게 되었다..!

알고리즘에서의 Brute Force는 빠르고 정확한 컴퓨터의 장점을 이용하여 __모든 경우의 수를 직접 "탐색"__ 해보는 것을 의미한다.

다른 말로는 __완전 탐색__ 이라고도 한다.

Brute Force는 복잡한 생각 없이 모든 경우를 다 살펴보는 것이므로 가장 단순하게 문제를 푸는 방법이면서 탐색 문제에서 틀릴 경우가 없다.

하지만 시간적 소비가 크다는 단점이 있다.

Brute Force 방법으로 문제를 풀 때 가장 많이 사용되는 것은 __반복문__ 과 __재귀 함수__ 이다.

반복문은 다중 반복문을 이용해서 간단하게 구현할 수 있지만 가변적인 길이를 다룰 땐 구현이 복잡해진다는 단점이 있다.

재귀함수는 다중 반복문에 비해 직관적이지는 않지만 Brute Force 방법으로 풀 때 가장 보편적으로 이용되는 방법이다.

탐색 문제를 풀어야할 때 기본적으로 완전 탐색을 하면 시간 안에 풀리는지 생각해보는 습관도 좋은 습관이다. 

어려워 보이는 문제도 문제의 입력 크기가 작아서 완전 탐색으로 풀리는 문제들이 의외로 있기 때문이다!

<br>

## 🤖 Brute Force 로 풀어보면 좋은 문제들!
---

[백준 6603번 - 로또](https://www.acmicpc.net/problem/6603) 문제를 반복문을 사용하여 풀어보자!

~~~c++
#include<iostream>
#include<vector>
using namespace std;

int main(){
	int k = -1;
	while(k != 0){
		cin>>k;
		vector<int> vec;
		for(int i = 0 ; i < k ; i++){
			int tmp;
			cin>>tmp;
			vec.push_back(tmp);
		}
		
		for(int i = 0 ; i < k ; i++){
			for(int j = i + 1 ; j < k ; j++){
				for(int h = j + 1 ; h < k ; h++){
					for(int g = h + 1 ; g < k ; g++){
						for(int d = g + 1 ; d < k ; d++){
							for(int o = d + 1 ; o < k ; o++){
								cout<<vec[i]<<' '<<vec[j]<<' '<<vec[h]<<' '<<vec[g]<<' '<<vec[d]<<' '<<vec[o]<<'\n';
							}
						}
					}
				}
			}
		}
		cout<<'\n';
	}
}
~~~

또는 백준의 N과 M 문제집을 풀어보는 것도 좋다.

<br>


