---
layout: post
title:  "[알고리즘] 💤 KMP 문자열 매칭 알고리즘"
date:   2020-01-21 18:34:10 +0700
categories: [Algorithm]
---

> [유튜브 동빈나](https://www.youtube.com/watch?v=yWWbLrV4PZ8&list=PLRx0vPvlEmdDHxCvAQS1_6XV4deOwfVrz&index=34) 를 통해 공부한 내용입니다.

<br>

## 💤 KMP 알고리즘이란?
---

> KMP = Knuth-Morris-Pratt(커누스-모리스-프랫)
>
> 커누스, 모리스, 프랫이라는 사람이 만든 알고리즘!

<br>

KMP 알고리즘은 __문자열 매칭 알고리즘__ 의 대표적인 알고리즘이다. 

> '처음 공부했을 때는 다소 난해하지만 반복적으로 공부하면 이해할 수 있을 것이다' 라고 유튜버 동빈나 님께서 말해주심...하하

[단순 비교 문자열 매칭 알고리즘](https://choheeis.github.io/algorithm/2020/01/21/C++SimpleStringMatching.html) 은 문자열이 매칭될 때까지 모든 경우를 다 비교하므로 최악의 경우 엄청난 시간이 소요될 수 있다. 

이와 다르게 __KMP 알고리즘은 모든 경우를 다 비교하지 않고도 문자열을 찾을 수 있는 알고리즘__ 이다!

<br>

## 💤 KMP 알고리즘의 동작 원리
---

- __KMP 알고리즘의 아이디어!__

	KMP 알고리즘은 모든 경우를 비교하지 않고 __점프__ 를 통해 부분 부분 비교하는 방식을 가진다.

	그럼 점프가 어떤 의미이고, 언제 점프하는 것일까?

	점프의 의미를 알기 위해서는 먼저 KMP 알고리즘에서 사용되는 __접두사__ 와 __접미사__ 라는 것을 알아야 한다.

	접두사는 어떠한 문자열에서 앞 쪽에 있는 문자열을 말하고, 접미사는 뒤 쪽에 있는 문자열을 말한다.

	예를 들어, __"abacaaba"__ 라는 문자열에서 접두사와 접미사는

	![01](https://user-images.githubusercontent.com/31889335/72785862-80503d00-3c6f-11ea-878d-1ed814cc66a7.PNG)

	가 될 수 있다.

	"abacaaba" 라는 문자열에서 가능한 접두사는 "a", "ab", "aba", "abac", "abaca", "abacaa", "abacaab", "abacaaba" 이다. 
	
	이와 반대로 가능한 접미사는 "a", "ba", "aba", "aaba", "caaba", "acaaba", "bacaaba", "abacaaba" 이다.
	
	위 그림에서 빨간 부분과 파란 부분은 접두사와 접미사가 같은 문자열인 경우이다!

	접두사와 접미사가 무엇인지를 잘 기억하고 다음 내용을 보자!

	<br>

- __KMP 알고리즘의 동작 과정__

	> --> [이 부분을 공부하며 추가로 참고한 사이트](https://bowbowbow.tistory.com/6)

	![15](https://user-images.githubusercontent.com/31889335/72807508-e900df00-3c9a-11ea-9d3c-cb3ef89be0ea.PNG)

	예를 들어 위와 같은 패턴의 문자열이 존재한다고 가정해보자.

	[단순 비교 문자열 매칭 알고리즘](https://choheeis.github.io/algorithm/2020/01/21/C++SimpleStringMatching.html) 을 사용한다면 패턴의 마지막 문자인 "E"에서 글과 매칭 실패이므로 

	![16](https://user-images.githubusercontent.com/31889335/72807719-5ad92880-3c9b-11ea-8e9f-bc4df4fbe5d8.PNG)

	이와 같이 패턴 전체를 오른쪽으로 한 칸 옮겨 다시 처음부터 비교해야 한다. 

	하지만 KMP 알고리즘을 사용하면 

	![17](https://user-images.githubusercontent.com/31889335/72808163-477a8d00-3c9c-11ea-98bd-d0fb11ae258f.PNG)

	이렇게 패턴을 단순 비교 문자열 매칭 알고리즘보다 훨씬 더 많이 이동시킨 후 'C'부터 다시 비교하면 된다! 
	
	![22](https://user-images.githubusercontent.com/31889335/72814305-2e77d900-3ca8-11ea-9da2-3d829bb4f8e2.PNG)


	즉, 패턴이 __"점프!"__ 하여 많이 이동된다는 것을 알 수 있다.

	<br>

	이렇게 점프할 수 있는 이유는 

	![18](https://user-images.githubusercontent.com/31889335/72808349-b657e600-3c9c-11ea-9443-28b55249fa2b.PNG)

	패턴안의 문자 중 매칭에 성공한 "ABCDAB" 까지에서 접두사와 접미사가 일치하는 부분인 "AB"가 존재하기 때문이다. 

	약간 애벌레가 기어가는 모습이랑 비슷한 것 같은데....!? (나중에 봤을 때 이해되기 쉽도록 그림으로 그려봤다,, 그림에 재능은 정말 없는걸로😅) 
	 
	![19](https://user-images.githubusercontent.com/31889335/72809599-4ac34800-3c9f-11ea-89b2-2b2e2ebe4ba6.PNG)
	
	![20](https://user-images.githubusercontent.com/31889335/72809596-4ac34800-3c9f-11ea-853a-ce3e5c48907a.PNG)
	
	점프시킨 "CD"는 맨 처음 비교할 때 이미 비교를 한 부분이기 때문에 다시 비교할 필요가 없다. 또, 위 그림에서 파란 부분인 "AB"도 맨 처음 비교할 때 비교한 부분이므로 다시 비교할 필요가 없다. 

	따라서 'C'부터 비교를 시작하면 되는 것이다!

	패턴 문자열을 이동시킬 때 주의할 점은 __문자를 비교하다가 다른 문자가 처음 등장했을 때 이동시키는 것__ 이며, 위 애벌레 비유 그림에서 "ABCDAB"가 애벌레의 몸이 되는 것처럼 다른 문자가 등장한 바로 이전 문자열이 애벌레가 되는 것이다! (애벌레가 되는 문자 뒤에 계속 이어지는 패턴 문자열은 애벌레를 이동시킨 후 그대로 뒤에 이어서 붙이면 된다!)

	즉, KMP 알고리즘도 처음에는 문자 하나하나 비교하는 것은 단순 비교 문자열 매칭 알고리즘과 같으나 서로 다른 문자가 나온 경우 패턴 문자열을 이동시키는 경우가 다른 것이다.

	이렇게 KMP 알고리즘을 사용하기 위해서는 패턴 문자열의 접두사, 접미사가 같은 부분이 있는지를 알고 있어야 하는데 이걸 어떻게 알아낼까? (이 과정이 매우 복잡스럽..!! 집중!) 

	<br>

	패턴 문자열의 접두사, 접미사가 일치하는 부분이 어디까지인지를 알아보기 위해 먼저 다음과 같은 기본 틀을 생각해보자.

	![02](https://user-images.githubusercontent.com/31889335/72786146-3451c800-3c70-11ea-9916-f71be27b386e.PNG)

	이제 이 틀에서 j와 i 밑에 있는 문자가 같은지 같지 않은지 비교하는 작업을 통해 접두사, 접미사가 일치하는 부분을 찾아나갈 것이다.

	<br>

	본격적으로 이 기본 틀을 가지고 위 문자열에서 접두사, 접미사가 "abc"로 일치한다는 사실을 알아내보자!!

	맨 먼저 j와 i 아래에 있는 문자인 'a' 와 'b'가 같은지 비교해본다. __'a'와 'b'는 다른 문자이기 때문에 'b' 아래의 결과 칸에 0(현재 j가 인덱스로 갖는 결과값)을 쓰고 i의 위치를 오른쪽으로 한 칸 옮긴다.__

	![03](https://user-images.githubusercontent.com/31889335/72787151-b3480000-3c72-11ea-9257-f63c3a19680f.PNG)

	그러면 이렇게 i의 위치가 변할 것이다!

	그 다음, 다시 j와 i 밑에 있는 문자인 'a'와 'a' 를 비교해본다. __'a'와 'a'는 같은 문자이기 때문에 'a' 아래의 결과 칸에 1(현재 j가 있는 칸의 인덱스 + 1)을 쓰고 i와 j의 위치를 모두! 오른쪽으로 한칸 옮긴다.__

	> 이 때, 결과값은 현재 j가 있는 칸의 인덱스에 1을 더한 것임을 알아야한다!!

	![04](https://user-images.githubusercontent.com/31889335/72787803-f48cdf80-3c73-11ea-8c58-9c17e68d5bb8.PNG)

	그러면 이렇게 j와 i의 위치가 변한다!

	> j와 i 밑의 문자가 다른 경우에는 i 의 위치만 옮기고, 같은 경우에는 i와 j의 위치를 모두 옮긴다!

	이제 __다시 j와 i 밑에 있는 'b'와 'c'를 비교해보니 'b'와 'c'는 같은 문자가 아니므로 j 만 한 칸 뒤로 이동시켜 다시 한번 j와 i 밑에 있는 문자를 비교한다.__

	즉, 

	![05](https://user-images.githubusercontent.com/31889335/72788081-81379d80-3c74-11ea-8d0d-a26d17c6cea5.PNG)

	이렇게 j를 뒤로 한칸 이동시킨 후, j와 i 밑의 문자인 'a'와 'c'를 다시 비교시킨다. 'a'와 'c'는 다른 문자이므로 'c' 아래 결과 칸에 0(j를 한 칸 뒤로 이동시킨 위치를 인덱스로 갖는 결과 값)을 넣고 i만 오른쪽으로 한 칸 이동시킨다.

	![06](https://user-images.githubusercontent.com/31889335/72788179-ba700d80-3c74-11ea-8e4b-cf2692ace549.PNG)

	그러면 이런 모습이 된다.

	다시 한번 j와 i 밑의 문자인 'a'와 'a'를 비교하면 두 문자가 같으므로 'a' 아래 결과 칸에 1(현재 j의 인덱스 + 1)을 넣고, j와 i 모두 1칸 증가시킨다. 

	![07](https://user-images.githubusercontent.com/31889335/72788326-0cb12e80-3c75-11ea-8606-ef3b35c7e002.PNG)

	위 그림에서 다시 j와 i 밑의 문자인 'b'와 'a'를 비교하면 같지 않기 때문에 j를 한 칸 뒤로 이동시킨 후 다시 j 와 i 밑의 문자를 비교한다.

	![08](https://user-images.githubusercontent.com/31889335/72788433-46823500-3c75-11ea-88f6-c398bd9410c6.PNG)

	이 상태에서 j와 i 밑의 문자인 'a'와 'a'는 같은 문자이므로 'a' 아래 결과 칸에 1(현재 j의 인덱스 + 1)을 적고 j와 i를 모두 오른쪽으로 한 칸 이동시킨다.

	![09](https://user-images.githubusercontent.com/31889335/72788767-ce683f00-3c75-11ea-82b9-8583c3589857.PNG)

	이와 같은 모습이 될 것이다.

	이 상태에서 다시 한번 j와 i 밑의 문자인 'b'와 'b'를 비교해보니 같은 문자이므로 'b' 아래 결과 칸에 2를 적고 (현재 j는 인덱스 1에 있으므로!) j와 i를 모두 오른쪽으로 이동시킨다.

	![10](https://user-images.githubusercontent.com/31889335/72788991-29019b00-3c76-11ea-8680-7d5ae27d1a29.PNG)

	그러면 이와 같은 모습이 될 것이다. 

	마지막으로 j와 i 아래에 있는 문자인 'a'와 'a'를 비교하면 같은 문자이므로 'a'의 결과 칸에 3 (현재 j가 인덱스 2에 있으므로!) 을 적고 이 과정이 완료되게 된다!

	> 휴,,,, 휴~~~ i랑 j 참 쉽지 않다.

	<br>

	여기서 모두 구해진 __결과값__ 은 접두사, 접미사가 일치하는 부분의 최대 길이를 의미한다. 

	즉, 

	![11](https://user-images.githubusercontent.com/31889335/72789391-0c199780-3c77-11ea-8951-dddd4e017302.PNG)

	![12](https://user-images.githubusercontent.com/31889335/72805752-ef8d5780-3c96-11ea-9cb1-fb2176d4d874.PNG)

	![13](https://user-images.githubusercontent.com/31889335/72805809-164b8e00-3c97-11ea-9303-5aad8a82774d.PNG)

	![14](https://user-images.githubusercontent.com/31889335/72805863-367b4d00-3c97-11ea-846d-ff707dbdda65.PNG)

	라는 것을 의미한다.

	그럼 이렇게 구한 결과값을 가지고 위에서 알아본 KMP 알고리즘 동작과정에 어떻게 사용해야 할까?

	아래 그림을 보면서 결과값을 어떻게 이용하는 것인지 생각해보자! 

	![24](https://user-images.githubusercontent.com/31889335/72962485-ce904800-3df7-11ea-8748-6c45c622f93e.PNG)

	위 그림에서 맨 처음 비교를 진행하면 빨간 부분의 a 에서 매칭이 처음 실패하게 된다. 

	그럼 이 때 점프를 해야하는데 점프를 하기 위해서는 a 바로 이전 문자열인 "abaca"에서 접두사, 접미사가 같은 부분이 있는지를 알아보아야 한다.

	같은 접두사, 접미사가 있는지 알아보기 위해 결과값을 보면 빨간 동그라미에 의해 1개 문자가 같은 접두사, 접미사가 "abaca"에 존재한다는 것을 알 수 있다. 

	따라서 패턴을 현재 매칭 실패 위치인 빨간색 a에서 1칸 전 위치로 이동시키면 된다는 것을 알 수 있게 된다!

	<br>


## 💤 KMP 알고리즘의 시간 복잡도
---

위 애벌레 예시를 보면

![19](https://user-images.githubusercontent.com/31889335/72809599-4ac34800-3c9f-11ea-89b2-2b2e2ebe4ba6.PNG)

이 그림에서 'E' 문자가 매칭되지 않아 

![20](https://user-images.githubusercontent.com/31889335/72809596-4ac34800-3c9f-11ea-853a-ce3e5c48907a.PNG)

이 그림처럼 패턴 문자열을 점프시키고 파란 부분인 "AB" 바로 뒤 부터 다시 비교하기 시작한다.

이 때, 점프를 한 __"CD"__ 는 이미 점프 전에 매칭 비교를 한 번 했으므로 넘어가는 것인데 이 과정을 글 전체로 확장시켜서 생각해보면 결국 문자 비교를 글 전체 문자수(N)만큼만 하게되는 것과 같다!

즉, 단순 비교 문자열 매칭 알고리즘의 시간복잡도는 O(N^2)이였지만 KMP 알고리즘의 시간복잡도는 __O(N)__ 이 되는 것이다!!!

![시간복잡도](https://user-images.githubusercontent.com/31889335/72819417-90d4d780-3cb0-11ea-9aea-b9ba8ef6c629.PNG) 

를 보면 O(N^2)보다 O(N)이 빠르다는 것을 알 수 있다.

<br>

## 💤 KMP 알고리즘 코드 작성
---

KMP 알고리즘 코드를 작성하기 위해서는 먼저 위에서 알아본 패턴 문자열의 접두사, 접미사가 일치하는 길이를 나타내는 결과값을 구하는 코드가 있어야 한다.

아래 코드는 패턴 문자열의 접두사, 접미사가 일치하는 길이를 나타내는 결과값을 구하는 코드이다.

~~~c++
#include<iostream>
#include<vector>
#include<string>
using namespace std;

/* 패턴의 접두사, 접미사 일치 결과에 의한 결과값 배열 만드는 함수 */
vector<int> makeResultArray(string pattern){
	int patternSize = pattern.size(); // 패턴 문자열의 길이 
	
	// 배열의 크기가 패턴 문자열의 길이와 같고, 배열의 모든 원소가 0으로 초기화된 vector 생성 = 결과값을 넣을 배열
	vector<int> resultArray(patternSize, 0);  
	
	int j = 0; 
	for(int i = 1 ; i < patternSize ; i++){
		
		// i 아래 문자와 j 아래 문자가 일치하는 경우 
		if(pattern[i] == pattern[j]){ 
			resultArray[i] = j + 1; // 현재 j의 인덱스에 1을 더함  
			j = j + 1; // j 를 i와 함께 증가시켜줌  
		}
		
		// i 아래 문자와 j 아래 문자가 일치하지 않을 경우 실행되는 부분
		// 두 문자가 다른 경우 j 를 뒤로 이동시키고 다시 한번 j와 i 아래에 있는 문자를 비교
		while(j > 0 && pattern[i] != pattern[j]){
			j = j - 1;
			if(pattern[i] == pattern[j]){
				resultArray[i] = j + 1;
				j = j + 1;
				break;
			}
		}
	}
	return resultArray; 
}

int main(){
	string pattern = "abacaaba";
	vector<int> resultArray = makeResultArray(pattern);
	for(int i = 0 ; i < resultArray.size() ; i++){
		cout<<resultArray[i]<<' ';
	}
}
~~~

이 코드를 실행시키면 

![21](https://user-images.githubusercontent.com/31889335/72812923-c32d0780-3ca5-11ea-85f1-e3100c4295d5.PNG)

이렇게 패턴 문자열 "abacaaba"에 대한 결과값 배열이 구해지게 된다.

<br>

이제 본격적으로 위 코드를 활용하여 KMP 알고리즘을 작성해보자!

~~~c++
#include<iostream>
#include<vector>
#include<string>
using namespace std;

/* 패턴의 접두사, 접미사 일치 결과에 의한 결과값 배열 만드는 함수 */
vector<int> makeResultArray(string pattern){
	int patternSize = pattern.size(); // 패턴 문자열의 길이 
	
	// 배열의 크기가 패턴 문자열의 길이와 같고, 배열의 모든 원소가 0으로 초기화된 vector 생성 
	vector<int> resultArray(patternSize, 0);  
	
	int j = 0; 
	for(int i = 1 ; i < patternSize ; i++){
		
		// i 아래 문자와 j 아래 문자가 일치하지 않을 경우 실행되는 부분
		// 두 문자가 다른 경우 j 를 뒤로 이동시키고 다시 한번 j와 i 아래에 있는 문자를 비교
		while(j > 0 && pattern[i] != pattern[j]){
			j = resultArray[j-1];	
		}
		
		// i 아래 문자와 j 아래 문자가 일치하는 경우 
		if(pattern[i] == pattern[j]){ 
			resultArray[i] = ++j; // 현재 j가 가리키는 원소의 인덱스에 1을 더하고 j를 i와 함께 증가시켜줌 
		}
	
	}
	return resultArray; 
}

/* KMP 알고리즘 */
// --> parent 문자열은 인덱스 i를 1씩 증가시키면서 pattern 문자열의 인덱스 j에 해당하는 문자와 비교한다.
// --> pattern 문자열은 인덱스 j를 1씩 증가시키면서 parent 문자열의 인덱스 i에 해당하는 문자와 비교한다.
void KMP(string parent, string pattern){
	// 패턴의 접두사, 접미사 일치 결과에 의한 결과값 배열을 makeResultArray함수로 바로 만듦  
	vector<int> resultArray = makeResultArray(pattern);
	
	int parentSize = parent.size();
	int patternSize = pattern.size(); 
 
	int j = 0;
	for(int i = 0 ; i < parentSize ; i++){
		
		// parent[i]에 해당하는 문자와 pattern[j]에 해당하는 문자가 같지 않을 경우 
		while(j > 0 && parent[i] != pattern[j]){
			j = resultArray[j-1]; // 패턴 문자열 점프
		}
		
		// parent문자열과 pattern문자열을 한 문자씩 비교할 때 두 문자가 같을 경우 
		if(parent[i] == pattern[j]){
			
			// j가 pattern 문자열의 끝까지 증가된 경우
			// 즉, pattern 문자열 전체가 parent에서 찾아진 경우!  
			if(j == patternSize - 1){
				cout<<"parent 문자열의 인덱스 ["<<i - patternSize + 1<<"]에서 찾았습니다."<<endl;
				j = resultArray[j]; // parent 문자열에 존재하는 또 다른 pattern을 찾아 나가기 위함 
			}else{
				j++;
			}
		}
		

	}
}

int main(){ 
	string parent = "ababacabacaabacaaba";
	string pattern = "abacaaba";
	KMP(parent, pattern);
}
~~~

위 코드의 실행 결과는 다음과 같다.

![23](https://user-images.githubusercontent.com/31889335/72906346-6e58c200-3d75-11ea-89ca-8dc54240d11d.PNG)

<br>

> 이렇게 KMP 알고리즘 스터디 끝!!!! 이해하고 정리해서 포스팅하는데까지 거의 5시간 넘게 걸린 것 같다,, 휴! 😖

<br>