---
layout: post
title:  "[RxJava] 🐶 Rx의 핵심 Observable!!"
date:   2020-02-03 18:34:10 +0700
categories: [RxJava]
---

> ["RxJava 프로그래밍"](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=116852658)이라는 책과 [ReactiveX documentation](http://reactivex.io/documentation/observable.html)을 참고하여 공부한 내용입니다.😃
>
> ![09](https://user-images.githubusercontent.com/31889335/74652601-ba0e5800-51c9-11ea-9a67-7f2b9232295d.PNG) --> [ReactiveX documentation](http://reactivex.io/documentation/observable.html) 이 문서에서 Observable 부분을 읽고 해석하고 이해한 내용!

<br>

## 🐶 Observable 이 등장하게 된 배경
---

[ReactiveX Observable 문서](http://reactivex.io/documentation/observable.html) 를 읽어보니 다음과 같은 말로 설명을 시작하고 있었다!


"우리는 보통 프로그래밍을 할 때, 작성한 명령어들이 우리가 작성한 순서대로 차례대로 실행되어 점진적으로 완성되는 방식에 익숙할 것이다."

즉, 절차식 프로그래밍에 익숙하다는 말이였다.

하지만 이와 다르게 ReactiveX 에서는 

__"여러 명령어들은 병렬적으로 실행된다. 즉, 어떠한 순서에 따라 실행되는 것이 아니다. 그리고 병렬적으로 실행된 명령어들의 결과는 observer에 의해 임의적으로 나중에 수집된다!"__ 

라고 설명 되어있었다.

뭔가 ReactiveX 에서는 명령어들을 일단 동시에 병렬적으로 실행한다는 것까지는 이해가 되었지만 실행된 명령어들의 결과는 observer에 의해 임의적으로 나중에 수집된다는 말은 잘 이해되지 않았다.

그래서 위 말을 이해하기 위해 더 자세히 문서를 읽어보았더니 아래와 같은 설명이 나와있었다! 

"ReactiveX 방식으로 프로그래밍을 하려면 먼저 데이터를 수집하고 조작하는 어떠한 __체계( = mechanism)__ 를 정의해야 한다. 이 체계를 __Observable__ 이라고 하고 observer가 이 Observable 이라는 데이터 수집 및 조작 체계를 구독하는 것이다."

"observer가 Observable을 구독하는 시점에 Observable은 observer와 함께 작동한다. 이 observer는 Observable이 방출하는 것에 반응하고, Observable이 방출하는 것을 잡기 위해 Observable을 감시하고 있는 것이다.

![12](https://user-images.githubusercontent.com/31889335/75011969-4b304800-54c4-11ea-8f41-a95d0af373d7.PNG)

<br>

즉, 위의 말을 이해해보면

우리가 ReactiveX 프로그래밍의 특징인 명령어를 병렬적으로 동시에 실행시키는 작업을 하기 위해서는 Observable과 observer 이라는 것이 필요하다는 것이고,

이 때, observer는 Observable을 구독함으로써 감시하고 있게 된다는 것이다.

이렇게 observer가 Observable을 구독하는 순간부터 Observable은 observer와 함께 작동하게 되고, observer는 Observable이 방출하는 어떤 것에 반응하거나 그것을 잡는 역할을 한다!

라고 이해해볼 수 있다.

여기서 Observable은 자신을 구독하고 있는 observer에게 observer의 함수(observer's method 라고 설명되어 있었다)들을 호출함으로써 item을 방출하거나 notification을 보낸다. 

이 말이 무슨 의미인지는 조금 나중에 이해할 수 있을 것이다! 라고 되어있었다.

이렇게 Observable과 observer가 존재하는 model을 Reactor Pattern(반응자 패턴) 이라고 부르기도 한다!

<br>

그렇다면 이러한  __ReactiveX의 장점__ 은 무엇일까?

__"ReactiveX의 장점은 어떠한 작업할 것들이 있을 때 이 작업할 것들이 서로에게 의존적이지 않다는 점이 ReactiveX의 장점이다!"__

작업할 것들이 서로에게 의존적이지 않다는 뜻은 무슨 의미일까?

작업할 것들이 서로에게 의존적이지 않다는 말은 작업할 것들을 한번에 동시에 실행시킬 수 있기 때문에 나온 말이다.

우리가 만약 작업할 것들을 동시에 시작하지 않는다면 어떻게 시작할 수 있을까?

작업할 것들을 하나씩 하나씩 차례대로 실행시킬 것이다.

즉, 한 작업을 먼저 실행시키고 이 작업이 끝나면 다음 작업을 실행시켜 모든 작업이 완료할 때까지 차례대로 실행시킨다는 의미이다. ( = 절차식으로 실행시킨다)

이 경우에는 각 작업이 서로에게 의존적이게 된다. 왜냐하면 바로 이전에 실행된 작업이 끝나야 다음 작업이 실행될 수 있는 관계이기 때문에 서로 의존하고 있는 것과 같다.


하지만 이와 다르게 ReactiveX 는 작업할 것들을 동시에 모두 실행시키기 때문에 어떠한 작업이 끝날때까지 기다릴 필요가 없어서 서로에게 의존적이지 않다.

따라서 ReactiveX 방식으로 작업을 하면 처리해야할 모든 작업이 완료될 때까지 걸리는 시간이 훨씬 줄어들게 된다.

즉, 동시에 실행시킨 작업들 중 가장 오래 걸리는 작업 만큼의 시간만이 걸리게 될 것이다. 

![11](https://user-images.githubusercontent.com/31889335/74656274-26408a00-51d1-11ea-8a47-bc310dc33095.PNG)

![10](https://user-images.githubusercontent.com/31889335/74656276-2771b700-51d1-11ea-972a-90ce1d647576.PNG)

따라서 ReactiveX 방식으로 프로그래밍을 하면 여러 작업을 동시에 실행시킬 수 있기 때문에 수행 시간을 줄일 수 있다는 장점이 있다!! ( = 비동기적으로 함수를 호출할 수 있다.)

<br>

## 🐶 observer(구독자) 생성하기
---

절차식 프로그래밍 방식에서 함수를 호출할 때는

1. 메소드를 호출한다.
2. 메소드의 return 값을 어떠한 변수에 저장한다.
3. 변수에 저장되어 있는 값을 어떠한 일을 하는데 사용한다.

와 같은 순서로 함수 호출이 실행된다.

하지만 ReactiveX와 같은 비동기적 모델에서 함수를 호출하는 flow는 다음과 같다.

1. return 값을 가지고 있는 어떤 함수를 정의한다. (여기서 이 함수는 observer의 일부분이다!)
2. 비동기적 호출 자체를 정의한다. (여기서 비동기적 호출 자체가 Observable이 된다.)
3. observer가 Observable을 구독하게 한다. (구독하게 하는 것 자체가 observer를 Observable에 붙이는 것과 같다. observer가 구독하는 순간 Observable의 활동이 시작된다.)
4. 3번까지 하고 나서 전혀 다른 작업을 하고 있으면 1번에서 정의한 함수의 return값이 방출될 때마다 observer의 함수는 return된 value를 가지고 작동하기 시작할 것이다. (여기서 1번에서 정의한 함수의 return 값을 Observable이 방출하는 item이라고 한다.)

<br>

위에서 알아본 4가지 단계를 조금 더 자세히 알아보자.

1단계는 구독자의 onNext handler를 정의하는 단계이다. 정의만 하고 아직 호출하지는 않는다.

2단계는 Observable을 정의하는 단계이다. 정의만 하고 아직 호출하는 것은 아니다.

3단계는 observer이 Observable을 구독하도록 하는 단계이고 구독함으로써 Observable이 비로소 호출되는 단계이다.

4단계는 그냥 이제 이 체계에 신경끄고 다른 일을 하면 되는 단계이다. 1,2,3번에서 정의한 것들의 상호작용이 알아서 될 것이다.

이렇게 4단계로 수행되는 비동기적 함수 호출을 수도 코드로 알아보면

~~~pseudocode
// 1. onNext 핸들러 정의
def myOnNext = { it -> do something useful with it };

// 2. Observable 정의
def myObservable = someObservable(itsParameters);

// 3. observer이 Observable 구독 시작
myObservable.subscribe(myOnNext);
~~~

위 수도 코드에서 3번을 보면 observer이 Observable을 구독하는 방법으로 __subscribe()__ 라는 함수를 호출한 모습을 볼 수 있다.

이 [subscribe()](http://reactivex.io/documentation/operators/subscribe.html) 함수는 observer가 Observable과 연결되는 방법으로 사용되는 함수이다. 

여기까지 이해가 되었다면! 이제 onNext가 뭔지 알아보자!

<br>

## 🐶 onNext, onCompleted, onError 
---

observer는 __onNext, onError, onCompleted__ 라는 3가지 함수들을 가지고 있다. 이 3가지 함수를 observer의 함수라고 부른다.(= observer's method)

<br>

1. __onNext 함수__

    Observable은 자신이 정상적으로 item을 방출할 때마다 observer의 onNext 함수를 호출한다.

    이 onNext 함수는 인자로 Observable로 부터 방출된 item을 갖게 된다.

    <br>

2. __onError 함수__

    Observable이 item을 방출하는 것을 실패했거나 어떠한 다른 에러가 발생했을 때는 onError 함수를 호출한다. 

    이 onError 함수가 호출되고 나서는 더 이상 onNext 함수나 onCompleted 함수가 호출되지 않을 것이다.

    onError 함수는 인자로 에러의 원인을 나타내는 것을 갖게 된다.

    <br>

3. __onCompleted 함수__

    Observable이 어떠한 오류도 만나지 않은 상태에서 onNext 함수 호출을 마지막으로 한 후에는 반드시 이 onCompleted 함수를 호출한다.

    즉, Observable이 방출할 item이 더 이상 존재하지 않아 onNext 함수를 방출할 일이 없어졌을 때 onCompleted 함수를 방출하는 것이다.

    <br>

이제 ReactiveX에서는 관습적으로 __Observable이 onNext 함수를 호출하는 것 자체를 item의 방출이라고 부르고,__ 

onNext를 제외한 __onCompleted 함수와 onError 함수를 호출하는 것을 notification을 호출한다고 부른다.__

<br>

따라서 observer는 자신의 함수로 onNext, onCompleted, onError 를 가지고 있고, 비동기적 함수 호출의 더 정확한 코드는 다음과 같다.

~~~
// 1. onNext 함수 정의
def myOnNext     = { item -> /* do something useful with item */ };

// 2. onError 함수 정의
def myError      = { throwable -> /* react sensibly to a failed call */ };

// 3. onCompleted 함수 정의
def myComplete   = { /* clean up after the final response */ };

// 4. Observable 정의( = 체계 정의)
def myObservable = someMethod(itsParameters);

// 5. observer가 Observable 구독하도록 하기
myObservable.subscribe(myOnNext, myError, myComplete);

// 6. 다른 할일 하기
~~~

<br>

## 🐶 구독 해지하기
---

위에서 알아본 ReactiveX는 여러 언어로 구현되어 있다. (RxJava, RxKotlin, RxJS 등등)

이 중 ReactiveX가 실행되도록 구현된 어떤 언어에서는 조금 특별한 observer interface인 __Subscriber__ 이 존재한다. (여기서 observer interface는 onNext, onCompleted, onError 같은 observer의 함수들을 말한다.)

이 Subscriber 이라는 interface에는 구독을 해지하는 기능인 unsubscribe 함수가 구현되어 있다.

observer가 구독하고 있는 Observable를 더 이상 구독하고 싶지 않을 경우 unsubscribe 함수를 호출하면 된다.

<br>

## 🐶 ReactiveX 에서 Naming Convention에 관련된 주의 사항
---

각기 다른 언어로 구현된 ReactiveX는 구현된 언어에 따라 각기 다른 naming 특징이 있다. 

왜냐하면 ReactiveX의 naming에 대한 표준이 정해져 있지 않기 때문에 ReactiveX를 구현한 각 언어마다 존재하는 해당 언어의 Convention을 따르면 된다.

더 나아가서는 ReactiveX를 구현한 각각의 언어마다 같은 이름이지만 서로 다른 함축적 의미를 가지고 있는 경우가 있을 수 있고, 어떤 언어에서는 자연스러운 이름이지만 특정 언어에서는 이상해 보일 수도 있다.

__onNext나 onError__ 같은 namming 패턴을 예로 들어보자. ( = on은 소문자, Next의 맨 첫 알파벳만 대문자인 패턴) 

어떠한 언어에서는 이러한 네이밍 규칙을 함수를 나타낼 때 사용한다. 즉, 자바에서의 onClick 함수를 생각해보면 이 onClick 함수는 클릭을 했을 때 실행되는 이벤트 핸들러이다. 하지만 ReactiveX에서 onNext는 이벤트 핸들러가 아니다. onNext는 그냥 onNext 함수의 핸들러인 것이다.  

이런 것처럼 ReactiveX 만의 naming convention은 없으므로 구현된 각 언어의 naming convetion을 따르는 것이 좋다.

<br>


## 🐶 "Hot" Observable과 "Cold" Observable
---

그렇다면 Observable은 언제 item을 방출하기 시작하는 걸까? 

이것을 알아보기 위해서 __Hot Observable__ 과 __Cold Observable__ 이 있다는 것을 알아보자.

Hot Observable은 Observable이 생성되자마자 item을 방출하기 시작하는 Observable을 부르는 말이다.

어떤 observer가 Hot Observable을 구독한다고 가정하고, 이 observer는 Hot Observable이 생성된 시점에 구독한 것이 아니라 생성되고 나서 시간이 조금 흐른 뒤에 구독을 시작했다고 생각해보자.

이런 경우일 때, 이 observer는 자신이 구독한 시점부터 Observable이 방출하는 item을 관찰할 수 있고, 구독하기 전에 방출된 item은 관찰할 수 없게 된다.

왜냐하면 Hot Observable 은 생성된 순간부터 item 방출이 시작되므로 observer가 구독하기 이전에도 item이 방출되어졌기 때문이다!

<br>

반면에 Cold Observable은 observer가 자신을 구독하기 시작할 때까지 item을 방출하지 않고 기다리는 Observable이다.

따라서 Cold Observable을 구독하는 observer들은 자신이 구독하는 시점부터 Observable의 item 방출이 시작되므로 모든 item 방출을 볼 수 있다는 점이 보장된다!

ReactiveX를 지원하는 어떤 언어에서는 Connectable Observable 이라는 것도 있다.

이 Observable은 observer가 구독하는 시점에 관계없이 Connect 함수가 호출되기 전까지는 item 방출을 하지 않는다. 

정리해보면 Hot Observable은 구독자가 구독하는 시점에 상관없이 생성된 순간부터 item을 발행하는 Observable 이고, Cold Observable은 구독자가 구독을 시작해야만 item을 발행하는 Observable 이다.

<br>

## 🐶 Observable 연산자(Operator)를 통해 Observable 구성하기!
---

지금까지 알아본 Observable과 observer라는 개념은 ReactiveX의 시작일뿐이다.

이 두 개념은 표준적으로 알려져있는 observer 패턴을 약간 확장시킨것뿐이고, 이 두 개념으로 callback 부분을 다룬다기 보다는 일련의 event 처리를 하는데 더 적합하다.

## 그리고 ReactiveX( = Reative Extension)의 진정한 힘은 __연산자(Operator)__ 에서 나온다!

ReactiveX 의 연산자는 Observable이 방출하는 item들을 조작할 수 있게 해주는 존재이다.

또, ReactiveX의 연산자는 선언적인 방법으로 비동기적 처리를 구성할 수 있게 해주고, ReactiveX 연산자로 구성한 비동기적 처리는 비동기 시스템의 callback 중첩 관련 문제점이 없는 처리가 된다.

ReactiveX에는 다음과 같은 Observable 관련 연산자들이 다양하게 존재한다!

![02](https://user-images.githubusercontent.com/31889335/75147509-d3bb1c80-5740-11ea-8d03-14d3c4b0b7ec.PNG)

따라서 각 연산자들의 설명을 보고 싶다면 [Observable Document](http://reactivex.io/documentation/observable.html)의 하단을 참고하자!!

<br>

## 🐶 RxJava로 Observable 생성해보기!
---

> ReactiveX 실습을 위해 사용한 언어는 RxJava입니다~

위에서 알아본 Observable 관련 연산자 표를 보면 맨 위에 Creating Observable 과 관련된 연산자들이 나열되어 있다!

즉, Observable을 생성할 때 사용되는 연산자들인 것이다.

[Observable 생성 관련 연산자](http://reactivex.io/documentation/operators.html#creating) 에 들어가보면 

![03](https://user-images.githubusercontent.com/31889335/75147905-bf2b5400-5741-11ea-9a8b-0d790887be78.PNG)

이와 같이 새로운 Observable을 생성하는데 사용되는 여러가지 연산자들이 있는 것을 볼 수 있다.

이 여러 연산자들 중 just() 함수를 이용해서 새로운 Observable을 생성하는 코드를 작성해보자.

~~~java
public static void main(String[] args) {
    Observable.just(1, 2, 3, 4, 5, 6).subscribe(System.out::println);
}
~~~

이 코드는 Observable을 생성한 후 observer가 구독한 모습까지의 코드이며 실행 결과는 

![04](https://user-images.githubusercontent.com/31889335/75148268-96f02500-5742-11ea-906e-d35e16452fd5.PNG)


이와 같다!

코드를 한번 차근차근 봐보자.

일단 Observable 클래스 안에 있는 just()라는 creating Observable 연산자를 사용하여 Observable을 생성한 후,

subscribe() 함수를 이용하여 생성한 Observable을 어떤 observer가 구독하기 시작한 모습이다!

just() 연산자에 대해서는 [여기](http://reactivex.io/documentation/operators/just.html)를 보면 알 수 있을 것이다!

또 subscribe() 연산자에 대해서는 [여기](http://reactivex.io/documentation/operators/subscribe.html)를 보면 알 수 있다!

subscribe() 연산자의 인자로 들어간 println 이라는 함수는 onNext 함수로 사용되는 것이다.

즉, observer는 just() 연산자로 생성된 Observable을 관찰하고 있고, Observable은 item을 방출하면서 자신의 observer의 함수인 onNext를 호출하는 것이다.

onNext 함수가 호출된 결과로 println 함수가 호출되어 실행 결과가 위 그림과 같이 나오는 것이다!

<br>

> 여기까지 Observable과 observer에 대한 개념 스터디 끝~!! 😲



