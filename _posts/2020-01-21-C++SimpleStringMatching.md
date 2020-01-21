---
layout: post
title:  "[알고리즘] 😛 단순 문자열 매칭 알고리즘"
date:   2020-01-21 18:34:10 +0700
categories: [Algorithm]
---

> [유튜브 동빈나](https://www.youtube.com/watch?v=WAzjfl7Pt_4&list=PLRx0vPvlEmdDHxCvAQS1_6XV4deOwfVrz&index=33) 영상으로 공부한 내용입니다.

<br>

## 😛 문자열 매칭?
---

문자열 매칭이란 특정한 글이 있을 때 그 글 안에서 하나의 특정 문자열을 찾는 것을 말한다. 

예를 들어, 인터넷 브라우저에서 Ctrl + F 를 누르고 찾고자 하는 단어를 검색하는 것도 문자열 매칭이라고 할 수 있다.

문자열 매칭을 해주는 알고리즘에는 대표적으로 KMP 알고리즘이 있다. 

하지만 복잡한 KMP 알고리즘을 다루기 전에 __단순 비교 문자열 매칭 알고리즘__ 에 대해 알아보자!

<br>

## 😛 단순 비교 문자열 매칭 알고리즘 동작 원리
---

- __단순 비교 문자열 매칭 알고리즘의 아이디어!__

	단순 비교 문자열 매칭 알고리즘은 '단순 비교' 라는 말 그대로 글의 처음부터 끝까지 한 문자씩 훑어보면서 찾고자 하는 문자열이 있는지 비교해보는 것이다.

	<br>

- __단순 비교 문자열 매칭 알고리즘의 동작 과정__

	단순 비교 문자열 매칭 알고리즘의 아이디어대로 시각화시켜보면 다음과 같다!

	![01](https://user-images.githubusercontent.com/31889335/72779742-517e9a80-3c60-11ea-8ce7-27c45b55b02d.PNG)

	![02](https://user-images.githubusercontent.com/31889335/72779740-50e60400-3c60-11ea-9b47-7e0f08e65672.PNG)

<br>

## 😛 단순 비교 문자열 매칭 알고리즘의 시간복잡도
---

위 과정을 보면 비교하는 작업이 __글의 총 길이(N) X 찾고자 하는 문자열의 길이(M)__ 만큼 반복되는 것을 알 수 있다. 

따라서, 이 알고리즘의 시간복잡도를 빅오 표현법으로 나타내면 __O(N*M)__ 이다! 쉽게 말하면 __O(N^2)__ 과 같은 것이다. 

시간 복잡도에 따르면 효율적이지는 않지만 직관적이고 코드가 간단하다는 장점이 있다. (길이가 10,000,000인 글에서 길이가 1,000인 문자열을 찾으려면 연산 양이 100억이다...)

> 시간 복잡도 그래프
>
> ![시간복잡도](https://user-images.githubusercontent.com/31889335/72781509-c5bb3d00-3c64-11ea-85dc-c640149029cf.PNG)

<br>

## 😛 단순 비교 문자열 매칭 알고리즘 코드
---

~~~c++
#include<iostream>
#include<string>
using namespace std;

/* 단순 비교 문자열 매칭 알고리즘 */
// --> parent : 글
// --> pattern : 찾고자 하는 문자열 	
int findString(string parent, string pattern){
	int parentSize = parent.size(); //글의 길이 
	int patternSize = pattern.size(); // 찾고자 하는 문자열의 길이 
	
	for(int i = 0 ; i <= parentSize - patternSize ; i++){
		bool finded = true; // 매칭 성공 여부를 저장하는 변수
		
		for(int j = 0 ; j < patternSize ; j++){
			// 문자열이 하나라도 매칭되지 않을 경우 
			if(parent[i + j] != pattern[j]){
				finded = false;
				break;
			}
		}
		
		// 문자열이 모두 매칭인 경우 
		if(finded == true){
			return i; // 글에서 문자열이 찾아진 위치를 인덱스로 return 
		}
	}
	return -1; // 찾고자 하는 문자열이 없을 경우 -1을 return 
}

int main(){
	string parent = "Hello World";
	string pattern = "llo W";
	
	int findedIndex = findString(parent, pattern);
	
	if(findedIndex == -1){
		cout<<"글이 찾고자 하는 문자열을 포함하고 있지 않습니다.";
	}else{
		cout<<"인덱스"<<findedIndex<<"에서 같은 문자열을 찾았습니다.";
	} 	
}
~~~

<br>