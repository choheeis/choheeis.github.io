---
layout: post
title:  "[RxJava] 🐼 Subject 클래스"
date:   2020-02-24 18:34:10 +0700
categories: [RxJava]
---

> ["RxJava 프로그래밍"](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=116852658)이라는 책과 [ReactiveX Subject 클래스 Docs](http://reactivex.io/documentation/subject.html)을 참고하여 공부한 내용입니다.😃

<br>

# 🐼 Subject 클래스란?

이전에 [Observable](https://choheeis.github.io/rxjava/2020/02/03/RxJavaObservable.html) 와 [Single](https://choheeis.github.io/rxjava/2020/02/10/RxJavaSingle.html) 에 대해 공부를 했다면!

__Subject 클래스__ 라는 것도 알아보자.

![01](https://user-images.githubusercontent.com/31889335/75418312-858f5e80-5976-11ea-91f8-9501a3f44111.PNG)

Rx 공식 문서를 보면 Subject 클래스를 다루는 배너가 있다. 파파고를 준비하고 한번 해석해보자ㅎㅎ

<br>

Subject는 Observable 으로도 행동할 수 있고, observer 로도 행동할 수 있는 존재이다.

즉, Observable처럼 item을 발행할 수도 있고, observer처럼 발행된 item을 바로 처리할 수도 있는 것이다.

여기서 Subject는 Observable을 구독하는 존재이기도 하기 때문에 Observable이 item 방출을 시작하도록 결정할 수 있다는 의미이다. (observer가 Observable을 subscribe 하기 시작하면 Observable의 item 방출이 시작되므로! 물론 Hot Observable을 말하는 것이다.)

또 이 말은 Cold Observable을 Hot Observable로 바꿀 수 있다는 의미이다.

즉, 구독자의 구독 시점에 상관없이 생성된 순간부터 item을 발행하는 Cold Observable을 구독자가 구독한 시점에 item을 방출하는 Hod Observable로 바꾸어준다는 것이다.

이 의미는 아래의 예시들을 보면 자연스럽게 이해할 수 있을 것이다!

Subject는 총 4개의 Subject로 나뉠 수 있다.

이 4개의 Subject는 특정한 경우에 따라서 다르게 만들어진 Subject이다.

이 4개의 Subject가 ReactiveX를 구현한 모든 언어에서 사용되는 것은 아닐 수도 있고, 이름도 조금씩 바뀌어서 사용되는 경우도 있다.

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
> 
> 위 그림의 빨간색 화살표 부분은 해당 Observable이 complete(완료)된 시점이라는 뜻이다!

즉, AsyncSubject에 의해 두 구독자에게 방출되는 item은 같은 시간에 방출되는 것이다.

만약 AsyncSubject가 어떤 에러로 인해 종료되지 않는 상황이라면 AsyncSubject는 구독자들에게 어떠한 item도 방출하지 않을 것이다.

왜냐하면 AsyncSubject는 반드시 Observable이 완료된 후에 item을 방출하기 때문이다.

![04](https://user-images.githubusercontent.com/31889335/75420295-17996600-597b-11ea-8cab-acd6f149df1a.PNG)

위 그림을 보면 AsyncSubject는 완료되지 않았으므로 구독자들에게 방출되는 item이 없는 모습임을 알 수 있다.

그렇다면 AsyncSubject를 나타내는 RxJava 코드를 봐보자!

~~~java
public static void main(String[] args) {
        // AsyncSubject를 create() 함수를 사용하여 생성한 모습 (Observable을 생성한 것과 같다.)
        AsyncSubject<String> subject = AsyncSubject.create();
        subject.subscribe(data -> System.out.println("Subscriber #1 => " + data));

         // onNext함수는 item을 발행하는 함수이다.
         // AsyncSubject를 생성할 때 데이터 타입을 String이라고 했으므로 onNext가 발행하는 item의 타입도 String이여야 한다.
        subject.onNext("1");
        subject.onNext("2");
        subject.subscribe(data -> System.out.println("Subscriber #2 => " + data));
        subject.onNext("5");
        subject.onComplete();
    }    
~~~

위 코드의 실행결과는 

![10](https://user-images.githubusercontent.com/31889335/75651778-c223c880-5c9c-11ea-85ac-0280822eee33.PNG)

이와 같다. 

즉, AsyncSubject가 complete된 시점에 두 observer에게 모두 마지막 item인 5가 방출된 모습이라는 것을 알 수 있다.

위 코드에서는 AsyncSubject가 Observable으로서 사용된 것이다.

그렇다면 observer로서 사용된 예시도 봐보자.

~~~java
    public static void main(String[] args) {
        Float[] temperature = {10.1f, 13.4f, 12.5f};

        // 온도 데이터를 담는 Observable 생성 (생성만 하고 아직 구독하지 않은 상태)
        Observable<Float> source = Observable.fromArray(temperature);

        // AsyncSubject 생성 (아직 Observable로 쓰일지 observer로 쓰일지 모름)
        AsyncSubject<Float> subject = AsyncSubject.create();
        subject.subscribe(data -> System.out.println("Subscriber #1 => " + data));

        // subject가 온도 데이터를 담은 Observable을 구독하는 구독자로 쓰임
        source.subscribe(subject);
    }
~~~

위 코드의 실행결과는

![11](https://user-images.githubusercontent.com/31889335/75653705-76275280-5ca1-11ea-8822-4aca6f0243ca.PNG)

이와 같다. Observable이 발행한 마지막 데이터인 12.5가 출력된 모습이다.



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

하지만 위 마블 다이어그램처럼 만약 Observable이 끝나지 않는다면 observer는 item을 얻을 수 없음을 알 수 있다.

그렇다면 RxJava 코드로 위 마블 다이어그램과 같은 BehaviorSubject를 구현해보자.

~~~java
    // 기본값이 1인 BehaviorSubject 생성
    BehaviorSubject<String> subject = BehaviorSubject.createDefault("1");

    // 이 시점에 구독한 observer는 기본값 1을 얻게됨
    subject.subscribe(data -> System.out.println("Subscribe #1 => " + data));        subject.onNext("2");
    subject.onNext("3");

    // 이 시점에 구독한 observer는 3을 얻게됨
    subject.subscribe(data -> System.out.println("Subscribe #2 => " + data));        subject.onNext("5");
    subject.onComplete();
~~~

이 코드의 실행결과는 

![12](https://user-images.githubusercontent.com/31889335/75655029-580f2180-5ca4-11ea-823d-25b61fdb6481.PNG)

이와 같다!

<br>

# 🐼 PublishSubject

또 다른 Subject 중 하나인 __PublishSubject__ 에 대해서 알아보자.

PublishSubject는 observer가 자신을 구독한 시점 이후로 방출되는 item만을 observer에게 방출해준다.

이것 또한 아래의 마블 다이어그램을 보고 이해해보자.

![07](https://user-images.githubusercontent.com/31889335/75650330-e41b4c00-5c98-11ea-9837-26a0bedc0782.PNG)

이 그림에서 두 명의 observer는 각각 PublishSubject에 의해 받는 item이 다르다.

첫 번째 observer는 자신이 구독한 시점 이후에 PublishSubject가 방출하는 item만을 받고 두 번째 observer는 같은 방식으로 자신이 구독한 시점 이후에 PublishSubject가 방출하는 item만을 받는다.

여기서 주의할 점은 PublishSubject는 자신이 생성된 즉시 아이템을 방출하기 시작하기 때문에 observer는 자신이 구독하기 이전 시점에 PublishSubject에서 방출된 item들은 얻지 못한다는 점이다.

만약 PublishSubject가 방출하는 모든 item을 얻고 싶다면 Observable을 생성할 때 create() 함수로 생성하거나 ReplaySubject를 사용해야 한다.

create() 함수로 생성하면 PublishSubject를 조작적으로 cold Observable로 바꿀 수 있기 때문이다.

만약 PublishSubject의 Observable이 에러와 함께 종료된다면 observer에게 더 이상 item을 줄 수 없게 된다.

![08](https://user-images.githubusercontent.com/31889335/75650588-a965e380-5c99-11ea-8f1d-e1df04bf5028.PNG)

<br>

# 🐼 ReplaySubject

4개의 Subject중 마지막 Subject인 ReplaySubject에 대해서 알아보자.

ReplaySubject는 Observable이 방출한 모든 아이템을 자신을 구독한 observer에게 방출한다.

이 때, observer가 자신을 구독한 시점이 언제이든지 상관없이 모든 item을 방출한다.

![09](https://user-images.githubusercontent.com/31889335/75650659-ec27bb80-5c99-11ea-8fb4-e068dd13a828.PNG)


이 마블다이어그램을 보면 두 번째 구독자가 구독하자 마자 앞에서 이미 방출했던 빨간색, 연두색 동그라미들을 한번에 차례로 방출해주는 모습을 볼 수 있다.

만약 ReplaySubject를 사용한다면 다중 스레드로부터 onNext 함수를 호출하지 않도록 주의해야한다. 왜냐하면 이런 호출이 순차적인 호출이 아닌 동시적으로 호출될 수 있기 때문이다. 

동시적으로 호출되면 Observable 계약에 위배되는 것이기 때문이다.

<br>