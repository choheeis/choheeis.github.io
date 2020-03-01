---
layout: post
title:  "[RxJava] 🐼 Subject 클래스"
date:   2020-02-24 18:34:10 +0700
categories: [RxJava]
---

> ["RxJava 프로그래밍"](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=116852658)이라는 책과 [ReactiveX Subject 클래스 Docs](http://reactivex.io/documentation/subject.html)을 참고하여 공부한 내용입니다.😃

<br>

# 🐼 Subject 클래스란?

[Observable](https://choheeis.github.io/rxjava/2020/02/03/RxJavaObservable.html) 와 [Single](https://choheeis.github.io/rxjava/2020/02/10/RxJavaSingle.html) 에 대해 공부를 했다면!

__Subject 클래스__ 라는 것도 알아보자.

![01](https://user-images.githubusercontent.com/31889335/75418312-858f5e80-5976-11ea-91f8-9501a3f44111.PNG)

Rx 공식 문서를 보면 Subject 클래스를 다루는 배너가 있다. 파파고를 준비하고 한번 해석해보자ㅎㅎ

<br>

Subject는 Observable 으로도 행동할 수 있고, observer 로도 행동할 수 있는 존재이다.

> 동시에?? 아니면 그냥 둘 중 하나로 될 수 있다는 건가??

Subject는 observer이기 때문에 하나 이상의 Observable을 구독할 수 있고 또, Subject는 Observable이기 때문에 새로운 item을 방출하거나 ~~~

> 여기 ~~~ 부분 이해가 안된다... 스터디때 물어보기

Subject는 Observable을 구독하는 존재이기 때문에 Observable이 item 방출을 시작하도록 결정할 수 있다는 의미이다. (observer가 Observable을 subscribe 하기 시작하면 Observable의 item 방출이 시작되므로! 물론 Cold Observable을 말하는 것이다.)

이것은 Cold Observable을 Hot Observable로 바꿀 수 있다는 의미이다.

> 여기도 띠이이이용

Subject는 총 4개의 Subject로 나뉠 수 있다.

이 4개의 Subject는 특정한 경우에 따라 다르게 만들어진 Subject이다.

이 4개의 Subject가 ReactiveX를 구현한 모든 언어에서 사용되는 것은 아니고 이름도 조금씩 바뀌어서 사용되는 경우도 있다.

<br>

# 🐼 AsyncSubject

4개의 Subject 중 하나는 __AsyncSubject__ 이다. 

AsyncSubject는 Observable이 방출한 가장 마지막 item을 방출해준다. 이 때 AsyncSubject가 이 마지막 item을 방출해주는 시점은 Observable이 다 끝난 후이다.

밑의 마블 다이어그램을 보고 이해해보자!

![02](https://user-images.githubusercontent.com/31889335/75419651-a7d6ab80-5979-11ea-8cb1-f3e18cbdf897.PNG)

위 그림의 네모 박스 밑의 화살표 2개는 각각 observer를 나타내고 subscribe() 라고 되어있는 파란색 점선 화살표는 observer가 AsyncSubject를 구독한 시점이다.

즉, 여기서 AsyncSubject는 Observable로서 작동하고 있는 것이다!

이 마블 다이어그램을 보면 첫 번째 observer는 AsyncSubject를 맨 처음부터 구독했고, 두 번째 observer는 AsyncSubject를 파란색 동그라미 직전에 구독한 모습이다.

이 때, 두 구독자에게 방출되는 item은 AsyncSubject의 마지막 item 인 파란 동그라미뿐이며 방출되는 시점은 AsyncSubject가 끝난 시점이다.

> 마블 다이어그램에서 
>
> ![03](https://user-images.githubusercontent.com/31889335/75420174-c38e8180-597a-11ea-9b87-bbf5289e3ab6.PNG)
> 이 부분은 해당 Observable이 complete(완료)된 시점이라는 뜻이다!

즉, 두 구독자에게 방출되는 item 시점이 같게 된다!

만약 AsyncSubject가 어떤 에러로 인해 종료되지 않는 상황이라면 AsyncSubject는 구독자들에게 어떠한 item도 방출하지 않을 것이다.

왜냐하면 AsyncSubject는 반드시 Observable이 완료된 후에 item을 방출하기 때문이다.

![04](https://user-images.githubusercontent.com/31889335/75420295-17996600-597b-11ea-8cab-acd6f149df1a.PNG)

위 그림을 보면 AsyncSubject는 완료되지 않았으므로 구독자들에게 방출되는 item이 없는 모습임을 알 수 있다.

<br>

# 🐼 BehaviorSubject

4개의 Subject 중 또 하나는 __BehaviorSubject__ 이다.

observer가 BehaviorSubject를 구독하면 구독한 시점에 BehaviorSubject는 Observable이 가장 최근에 방출했던 item을 observer에게 방출해준다.

또는 가장 최근에 방출한 item이 없다면 기본값을 방출해준다.

이것도 아래 마블다이어그램을 보고 이해해보자.

![05](https://user-images.githubusercontent.com/31889335/75421322-56c8b680-597d-11ea-8ca0-243aad609fa7.PNG)

위 그림을 보면 두 observer는 각각 BehaviorSubject를 구독한 시점이 다르게 배치되어 있다.

첫 번째 구독자는 BehaviorSubject를 빨간 동그라미 이전에 구독했으므로 빨간 동그라미 이전에 방출된 가장 최근값이 없는 상황이다.

이 때는 BehaviorSubject의 기본값으로 갖고 있는 핑크색 동그라미를 방출하게 된다.

두 번째 구독자는 파란색 동그라미 이전에 구독하므로 그 시점에서 가장 최근에 방출된 item인 연두색 동그라미를 얻게 된다.

두 구독자 모두 BehaviorSubject에 의해 방출된 item을 얻은 후에도 계속 Observable이 방출하는 다른 item들도 얻는 모습을 볼 수 있다.

즉, BehaviorSubject는 가장 최근에 방출했던 item이나 기본 item을 먼저 방출해주고, 나머지 item을 계속 이어서 방출해준다는 걸 알 수 있다.

![06](https://user-images.githubusercontent.com/31889335/75421566-e4a4a180-597d-11ea-8f4a-4a3d95cc8c8f.PNG)

하지만 위 마블 다이어그램을 보면 만약 Observable이 끝나지 않는다면 observer는 item을 얻을 수 없음을 알 수 있다.

<br>

# 🐼 PublishSubject

또 다른 Subject 중 하나인 __PublishSubject__ 에 대해서 알아보자.

> 아직 포스팅하지 말기!!


