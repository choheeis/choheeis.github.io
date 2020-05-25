---
layout: post
title:  "[자료구조] 👆 우선 순위 큐"
date:   2020-05-05 18:34:10 +0700
categories: [자료구조]
---

> [라이 블로그 - Priority Queue](http://blog.naver.com/kks227/220791188929) 를 보고 공부한 내용입니다.

<br>

## 👆 우선 순위 큐(Priority Queue)란?
---

우선 순위 큐는 '큐(queue)' 라는 말이 포함되어 있듯이 데이터를 넣을 수도 있고, 뺄 수도 있는 구조이다.

하지만!

__일반적인 큐와는 하는일도 다르고 내부 구조도 완전히 다르다!!__

우선 순위 큐에도 대표적인 기능으로 __push__, __top__, __pop__ 이라는 기능들이 있지만 top과 pop에 의해서 빠져나오는 원소는 일반적인 큐와는 다른 원소가 빠져나온다.

일반적인 큐에서 pop을 하였을 경우 가장 먼저 들어간 데이터가 나오는데 우선 순위 큐에서는 가장 먼저 들어간 데이터가 나오는 것이 아니라 __우선순위가 높은 데이터가 나오게 된다!__

예를 들어 우선 순위 큐에 {1, 4, 3, 7} 을 순서대로 넣었다면 처음 pop 연산에 의해 나오는 데이터는 1이 아니라 7이 된다.

7이 빠진 {1, 4, 3}에서 다시 한번 pop 연산을 하면 4가 나오게 되는 것이다!

즉, 우선 순위 큐 내부에서는 __정렬__ 이 일어난다는 의미이며 우선 순위 큐 내부에서 사용되는 정렬 알고리즘은 __힙 정렬(Heap sort)__ 이다.

> 힙 정렬에 대한 포스팅 --> [여기](https://choheeis.github.io/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/2020/02/04/heapSort.html)

<br>

## 👆 우선 순위 큐(Priority Queue) 내부 원리
---

우선 순위 큐는 기본적으로 pop 연산을 할 때 우선 순위가 높은 것 부터 나오게 된다.

그렇기 때문에 __최대 힙__ 을 사용한 힙 정렬이 내부에서 일어나고 있는 것이다.

먼저 우선 순위 큐에 __push__ 연산을 할 경우 내부에서 일어나는 일에 대해 알아보자.

![01](https://user-images.githubusercontent.com/31889335/81049658-9968bb00-8ef9-11ea-97d3-7aec282b37ad.PNG)

우선 순위 큐 내부에 위와 같은 데이터들이 최대 힙 구조로 들어가 있다고 가정하고, 9라는 데이터가 추가로 push 되었다고 해보자.

![02](https://user-images.githubusercontent.com/31889335/81049817-e187dd80-8ef9-11ea-8c24-806d0b3e2b4b.PNG)

그렇다면 이렇게 일단 힙의 완전 이진 트리 형태를 유지하도록 첫 번째 빈자리로 들어가게 된다.

그 다음, 최대 힙 구조를 만족하도록 9는 다시 한번 배치가 될 것이다.

![03](https://user-images.githubusercontent.com/31889335/81049940-257ae280-8efa-11ea-8268-fcbd13c861b1.PNG)

이렇게 9는 자신의 부모 노드 3과 자리를 바꿀 것이고, 이어서

![04](https://user-images.githubusercontent.com/31889335/81050011-43484780-8efa-11ea-8aaf-3ff61dc78150.PNG)

이와 같이 새로운 부모 노드인 6과도 자리를 바꿀 것이다.

여기까지가 9를 push 했을 때 우선 순위 큐의 내부에서 일어나는 일의 전부이다!

push 연산 한 번 할 때 자리를 바꾸는 연산은 최대 트리의 높이만큼 수행될 수 있으므로 push 연산의 시간 복잡도는 O(logN) 이다. 

> N은 트리의 전체 노드 수.

<br>

그렇다면 이번에는 __pop__ 연산이 수행될 경우 우선 순위 내부에서 일어나는 일에 대해 알아보자.

우선 순위 큐에서 pop 연산을 하면 우선 순위가 높은 원소가 빠져나오게 되므로 위 트리에서는 root 노드였던 17이 빠져나오게 될 것이다.

![05](https://user-images.githubusercontent.com/31889335/81050730-7808ce80-8efb-11ea-8c66-ea4cb8eb187c.PNG)

그렇다면 17이 빠져나간 root 자리가 비어있게 되는데 어떻게 되는 것일까??

![06](https://user-images.githubusercontent.com/31889335/81051731-3a0caa00-8efd-11ea-8090-969d1a01a8d3.PNG)

이렇게 최대 힙 구조의 가장 마지막 원소를 root 자리에 넣는다!

![07](https://user-images.githubusercontent.com/31889335/81050818-a1c1f580-8efb-11ea-91d6-86a7cf2c2d0e.PNG)

즉, 위와 같은 그림이 되겠지만..! 최대 힙 구조를 만들지 못하고 있으므로 다시 한 번 최대 힙 구조를 만들어야 한다.

먼저 부모 노드로 선정된 3보다 자식 노드인 13이 더 크므로 13이 부모 노드로 올라가게 된다.

![08](https://user-images.githubusercontent.com/31889335/81051841-717b5680-8efd-11ea-8806-8043a5979822.PNG)

계속적으로 최대 힙을 만들기 위해 자리를 바꿔주다 보면

![09](https://user-images.githubusercontent.com/31889335/81052038-c028f080-8efd-11ea-82ca-1fa682ee3d59.PNG)

![10](https://user-images.githubusercontent.com/31889335/81052044-c1f2b400-8efd-11ea-8d4a-60618cae0ea0.PNG)

이렇게 3은 자신이 있어야 할 자리로 배치되고 root 노드는 13이 차지하게 된다.

따라서 맞는 자리를 찾으면 pop 연산은 끝이 난다! 

이렇게 pop 연산이 행해졌을 때 우선 순위 큐 내부에서 일어나는 swap 행위는 최대 트리 높이만큼 일어나게 되므로

pop 연산의 시간 복잡도는 __O(logN)__ 이 된다!

<br>

## 👆 우선순위 큐 C++ STL로 쉽게 사용하기
---

[c++ queue 레퍼런스](http://www.cplusplus.com/reference/queue/) 에 들어가보면

![11](https://user-images.githubusercontent.com/31889335/81053332-de8feb80-8eff-11ea-8e30-bff6ba31d53c.PNG)

위와 같이 \<queue> 라는 헤더파일에 총 두 개의 클래스가 정의되어 있는 것을 볼 수 있다.

그 중 우선 순위 큐를 위한 __[priority_queue](http://www.cplusplus.com/reference/queue/priority_queue/)__ 라는 클래스가 있다!

이 클래스를 읽어보니 최대 힙을 사용한 우선순위 큐를 구현해놓고 여러 멤버 함수로 큐 내부의 원소에 접근할 수 있도록 만들어진 클래스이다.

![12](https://user-images.githubusercontent.com/31889335/81054677-2e6fb200-8f02-11ea-9b11-bf2588b2290f.PNG)

위와 같은 멤버 함수들이 있으니 적합할 때 사용하면 된다!

<br>

