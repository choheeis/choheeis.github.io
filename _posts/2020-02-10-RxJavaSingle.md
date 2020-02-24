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

Single 클래스는 Observable 클래스와  비슷하지만 Single 클래스 만의 특징을 가지고 있다! 

Observable는 item을 연속으로 무한히 발행할 수 있지만 Single 클래스는 단 하나의 item만 발행하거나 하나의 에러 notification만 발행한다는 점이 Single 클래스만의 특징이다.

> item을 발행하는 것을 data를 발행한다고도 부른다.

이런 Single 클래스의 특징 때문에 Single 클래스로 Observable을 생성하면 Observable로 부터 onNext, onError, onCompleted 함수가 호출되는 것이 아니라 __onSuccess__ 와 __onError__ 함수만 호출된다.

<br>

- __onSuccess 함수__

    Single 클래스로 생성된 Observable은 단 한 개의 아이템을 방출하고 이 때 Single은 onSuccess 함수를 호출한다. 
    
    따라서 방출된 단 한 개의 item만이 이 onSuccess 함수와 함께 사용된다.

- __onError__

    onError 함수가 호출되면 Single 클래스로 생성된 Observable이 item을 방출할 수 없게된 원인을 알 수 있다.

    <br>

Single Observable은 위 두 함수 중 단 하나의 함수만을 딱 한 번 호출하게 된다.

그리고 두 함수 중 하나를 호출한 후에는 Single이 종료되고 observer와의 연결도 끊어지게 된다!


## 🐭 Single 연산자(Operator)을 통한 Single 구성하기
---

Observable과 마찬가지로 Single도 여러 다양한 연산자를 통해 조작할 수 있다.

어떤 연산자들은 Observable과 Single 사이의 interface로 사용되기 때문에 Observable과 Single을 혼합할 수 있게 해준다.


<br>

## 🐭 RxJava 코드로 Single 만들어보기!
---

Single은 Observable을 생성할 때와 비슷한 방법으로 생성할 수 있다.

그 방법 중 just() 함수를 사용하는 방법이 있는데 다음과 같다!

~~~java
    public static void main(String[] args) {
        Single<String> source = Single.just("Hello Single!");
        source.subscribe(System.out::println);
    }
~~~

위 코드는 Single을 just() 함수를 이용하여 생성한 것이고 단 하나의 item 인 "Hello Single!" 이 onSuccess 함수인 println과 함께 사용된 모습이다!

이 코드의 실행 결과는

![01](https://user-images.githubusercontent.com/31889335/75146596-c735c480-573e-11ea-916b-0b50613a11db.PNG)


이와 같다.

