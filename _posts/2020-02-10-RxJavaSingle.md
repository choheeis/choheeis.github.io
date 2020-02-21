---
layout: post
title:  "[RxJava] 🐭 Single 클래스"
date:   2020-02-10 18:34:10 +0700
categories: [RxJava]
---

> ["RxJava 프로그래밍"](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=116852658)이라는 책과 [ReactiveX Single 클래스 Docs](http://reactivex.io/documentation/single.html)을 참고하여 공부한 내용입니다.😃

## 🐭 Single 클래스란?
---

RxJava에는 Single 이라는 클래스가 존재하는데 이 클래스는 앞 포스팅에서 알아본 [Observable 클래스](https://choheeis.github.io/rxjava/2020/02/03/RxJavaObservable.html)의 특수한 케이스로 개발되고 있는 클래스이다. 

그래서 Single 클래스는 Observable 클래스와 비슷하지만 Single 클래스 만의 특징을 가지고 있다. 

Observable 클래스는 데이터를 연속으로 무한히 발행할 수 있지만 Single 클래스는 단 하나의 데이터만 발행하거나 에러 notification만 발행한다는 점이 Single 클래스만의 특징이다.

> 그래서 Single 클래스는 결과가 유일한 서버 API를 호출할 때 유용하게 사용할 수 있다!

이런 Single 클래스의 특징 때문에 Single 클래스로 Observable을 생성한 구독자(observer)는 Observable이 전달하는 3개의 알림(onNext, onError, onCompleted)를 구독할 필요가 없고, __onSuccess__ 와 __onError__ 라는 2개의 알림만 구독하면 된다!

> 지금부터 이와 같은 알림은 method로 이해하면 편하다!

<br>

- __onSuccess__

    Single 클래스로 생성된 Observable은 단 한 개의 아이템을 방출하는데 이 단 한 개의 아이템이 onSuccess 알림(method)인 것이다. 

- __onError__

    이 알림(method)이 전달되면 Single 클래스로 생성된 Observable이 더 이상 어떠한 아이템을 방출할 수 없도록 할 수 있다. 

Single 클래스는 위와 같은 알림(메소드)들 중 단 한 개만을 호출할 수 있고, 딱 한 번만 호출할 수 있다.

그리고 Single 클래스가 두 메소드 중 하나를 호출하면 Single이 종료되고 이와 동시에 Single로 생성된 Observable을 구독하고 있던 구독자도 종료된다.

<br>

이제 Single 클래스의 개념을 filp() 이라는 함수를 호출한 상황을 가정하여 마블 다이어그램으로 이해해보자.

