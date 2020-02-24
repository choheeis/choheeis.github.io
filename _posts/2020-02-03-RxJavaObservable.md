---
layout: post
title:  "[RxJava] 🐶 RxJava의 핵심 Observable!!"
date:   2020-02-03 18:34:10 +0700
categories: [RxJava]
---

> ["RxJava 프로그래밍"](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=116852658)이라는 책과 [ReactiveX documentation](http://reactivex.io/documentation/observable.html)을 참고하여 공부한 내용입니다.😃
>
> ![09](https://user-images.githubusercontent.com/31889335/74652601-ba0e5800-51c9-11ea-9a67-7f2b9232295d.PNG) --> [ReactiveX documentation](http://reactivex.io/documentation/observable.html) 이 문서에서 Observable 부분을 읽고 해석하고 이해한 내용!

<br>

## 🐶 Observable 이 등장하게 된 배경
---

[ReactiveX Observable 문서](http://reactivex.io/documentation/observable.html) 를 읽어보니 다음과 같은 말로 설명을 시작했다!


"우리는 보통 프로그래밍을 할 때, 작성한 명령어들이 우리가 작성한 순서대로 차례대로 실행되어 점진적으로 완성되는 방식에 익숙할 것이다."

즉, 절차식 프로그래밍에 익숙하다는 말이였다.

하지만 이와 다르게 ReactiveX 에서는 

__"여러 명령어들은 병렬적으로 실행된다. 그리고 병렬적으로 실행된 명령어들의 결과는 observer에 의해 임의적으로 나중에 수집된다!"__ 

라고 설명이 되어있었다.

뭔가 ReactiveX 에서는 명령어들을 일단 동시에 병렬적으로 실행한다는 것까지는 이해가 되었지만 실행된 명령어들의 결과는 observer에 의해 임의적으로 나중에 수집된다는 말은 잘 이해되지 않았다.

그래서 위 말을 이해하기 위해 더 자세히 문서를 읽어보았더니 아래와 같은 설명이 나와있었다! 

"ReactiveX 방식으로 프로그래밍을 하려면 먼저 데이터를 수집하고 조작하는 어떠한 __체계( = mechanism)__ 를 정의해야 한다. 이 체계를 __Observable__ 이라고 하고 observer가 이 Observable 이라는 데이터 수집 및 조작 체계를 구독하는 것이다."

"observer가 Observable을 구독하는 시점에 Observable은 observer와 함께 작동한다. 이 observer는 Observable이 방출하는 것에 반응하고, Observable이 방출하는 것을 잡기 위해 Observable을 감시하고 있는 것이다.

![12](https://user-images.githubusercontent.com/31889335/75011969-4b304800-54c4-11ea-8f41-a95d0af373d7.PNG)

<br>

즉, 위의 말을 이해해보면

우리가 ReactiveX 프로그래밍의 특징인 명령어를 병렬적으로 동시에 실행시키는 작업을 하기 위해서는 Observable과 observer 이라는 것이 필요하다는 것이고,

이 때, observer는 Observable을 구독함으로써 감시하고 있게 된다. 이렇게 observer가 Observable을 구독하는 순간부터 Observable은 observer와 함께 작동하게 되고, observer는 Observable이 방출하는 어떤 것에 반응하거나 그것을 잡는 역할을 한다!

라고 이해해볼 수 있다.

여기서 Observable은 자신을 구독하고 있는 observer에게 observer의 함수들을 호출함으로써 item을 방출하거나 notification을 보낸다. 

이 말이 무슨 의미인지는 조금 나중에 이해할 수 있을 것이다!

이렇게 Observable과 observer가 존재하는 model을 Reactor Pattern(반응자 패턴) 이라고 부르기도 한다!

<br>

그렇다면 이러한 방식인 __ReactiveX의 장점__ 은 무엇일까?

__"ReactiveX의 장점은 어떠한 작업할 것들이 있을 때 이 작업할 것들이 서로에게 의존적이지 않다는 점이 ReactiveX의 장점이다!"__

작업할 것들이 서로에게 의존적이지 않다는 뜻은 무슨 의미일까?

작업할 것들이 서로에게 의존적이지 않다는 말은 작업할 것들을 한번에 동시에 실행시킬 수 있기 때문에 나온 말이다.

우리가 만약 작업할 것들을 동시에 시작하지 않는다면 어떻게 시작할 수 있을까?

작업할 것들을 하나씩 하나씩 차례대로 실행시킬 수 있다. 즉, 한 작업을 먼저 실행시키고 이 작업이 끝나면 다음 작업을 실행시켜 모든 작업이 완료할 때까지 차례대로 실행시킨다는 의미이다. ( = 절차식으로 실행시킨다)

이 경우에는 각 작업이 서로 서로 의존적이게 된다. 왜냐하면 바로 이전에 실행된 작업이 끝나야 다음 작업이 실행될 수 있는 관계이기 때문에 서로 의존적이게 된다.


하지만 이와 다르게 ReactiveX 는 작업할 것들을 동시에 모두 실행시키기 때문에 어떠한 작업이 끝날때까지 기다릴 필요가 없어서 서로에게 의존적이지 않게 된다.

따라서 ReactiveX 방식으로 작업을 하면 처리해야할 모든 작업이 완료될 때까지 걸리는 시간은 작업들 중 가장 오래 걸리는 작업 만큼의 시간이 걸리게 될 것이다. 

![11](https://user-images.githubusercontent.com/31889335/74656274-26408a00-51d1-11ea-8a47-bc310dc33095.PNG)

![10](https://user-images.githubusercontent.com/31889335/74656276-2771b700-51d1-11ea-972a-90ce1d647576.PNG)

ReactiveX 방식으로 프로그래밍을 하면 여러 작업을 동시에 실행시킬 수 있기 때문에 수행 시간을 줄일 수 있다는 장점이 있다!! ( = 비동기적으로 함수를 호출할 수 있다.)

<br>

## 🐶 observer(구독자) 생성하기
---

절차식 프로그래밍 방식에서 함수를 호출할 때는

1. 메소드를 호출한다.
2. 메소드의 return 값을 어떠한 변수에 저장한다.
3. 변수에 저장되어 있는 값이나 변수를 어떠한 일을 하는데 사용한다.

와 같은 순서로 함수 호출이 실행된다.

하지만 ReactiveX와 같은 비동기적 모델에서 함수를 호출하는 flow는 다음과 같다.

1. return 값을 가지고 있는 어떤 함수를 정의한다. (여기서 이 함수는 observer의 일부분이다!)
2. 비동기적 호출 자체를 정의한다. (여기서 비동기적 호출 자체가 Observable이 된다.)
3. observer가 Observable을 구독하게 한다. (구독하게 하는 것 자체가 observer를 Observable에 붙이는 것과 같다. observer가 구독하는 순간 Observable의 활동이 시작된다.)
4. 3번까지 하고 나서 전혀 다른 작업을 하고 있으면 1번에서 정의한 함수의 return값이 방출될 때마다 observer의 함수는 return된 value를 가지고 작동하기 시작할 것이다. (여기서 1번에서 정의한 함수의 return 값을 Observable이 방출하는 item이라고 한다.)

위에서 알아본 4가지 단계를 조금 더 자세히 알아보면

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

observer는 __onNext, onError, onCompleted__ 라는 3가지 함수들을 가지고 있도록 되어있다. 이 3가지 함수를 observer의 함수라고 부른다.

> 위에서 Observable이 item을 방출하거나 notification을 보낼 때 observer의 함수를 호출한다고 했었는데 그게 이것이다!

<br>

1. __onNext 함수__

    Observable은 자신이 정상적으로 item을 방출할 때마다 이 onNext 함수를 호출한다.

    이 onNext 함수는 인자로 Observable로 부터 방출된 item을 가진다.

    <br>

2. __onError 함수__

    Observable이 item을 방출하는 것을 실패했거나 어떤 다른 에러가 발생했을 때는 이 onError 함수를 호출한다. 

    이 onError 함수가 호출되고 나서는 더 이상 onNext 함수나 onCompleted 함수가 호출되지 않을 것이다.

    onError 함수는 인자로 에러의 원인을 나타내는 것을 가지고 있다.

    <br>

3. __onCompleted 함수__

    Observable이 어떠한 오류도 만나지 않은 상태에서 onNext 함수의 호출을 마지막으로 한 후에는 이 onCompleted 함수를 호출한다. 

    즉, Observable이 방출할 item이 더 이상 존재하지 않아 onNext 함수를 방출할 일이 없어졌을 때 onCompleted 함수를 방출한다는 것이다. 

    <br>

이제 ReactiveX에서는 관습적으로 __Observable이 onNext 함수를 호출하는 것 자체를 item의 방출이라고 부르고,__ 

onNext를 제외한 __onCompleted 함수와 onError 함수를 호출하는 것을 notification을 호출한다고 부른다.__

그렇기 때문에 위에서 Observable에 대해서 설명할 때 "Observable은 item을 방출하거나 notification을 보낸다" 라고 설명했던 것이였나보다!

<br>

따라서 observer는 자신의 함수로 onNext, onCompleted, onError 를 가지고 있으므로 비동기적 함수 호출의 더 정확한 코드는 다음과 같다.

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

이 중 ReactiveX가 구현된 어떤 언어에서는 조금 특별한 observer interface인 __Subscriber__ 이 존재한다. (여기서 observer interface는 onNext, onCompleted, onError 같은 observer의 함수들을 말한다.)

이 Subscriber 이라는 interface에는 구독을 해지하는 기능인 unsubscribe 함수가 구현되어 있다.

observer이 더 이상 구독하고 있는 Observable를 구독하고 싶지 않을 경우 unsubscribe 함수를 호출하면 된다.

그러면 Observable에게는 새로운 item을 발행하는 것을 멈출지 말지에 대한 선택권이 주어지게 된다. 

unsubscribe에 대한 결과는 Observable에 적용된 operator들의 chain을 타고 흘러갈 것이다. 그리고 그 chain 안에서 더 이상 item 방출을 멈추기 위한 각각의 link를 생성할 것이다. 

하지만 이러한 과정이 즉시 일어난다고 보장할 수는 없고, unsubscribe를 통해 Observable에 붙어있는 observer가 아무도 없음에도 Observable이 item을 계속 방출하거나 방출하려고 시도할 가능성이 있다. 

> 이 부분은 나중에 관련 코드가 나오면 다시한번 작성하자!

<br>

## 🐶 ReactiveX 에서 Naming Convention에 관련된 주의 사항
---

각기 다른 언어로 구현된 ReactiveX는 구현된 언어에 따라 각기 다른 naming 특징이 있다. 

왜냐하면 ReactiveX의 naming에 대한 표준은 없기 때문에 ReactiveX를 구현한 각 언어마다 존재하는 해당 언어의 Convention을 따르면 된다.

더 나아가서는 ReactiveX를 구현한 각각의 언어마다 같은 이름이지만 서로 다른 함축적 의미를 가지고 있는 경우가 있을 수 있고, 어떤 언어에서는 자연스러운 이름이지만 특정 언어에서는 이상해 보일 수도 있다.

__onNext나 onError__ 같은 namming 패턴을 예로 들어보자. ( = on은 소문자, Next의 맨 첫 알파벳만 대문자인 패턴) 

어떠한 언어에서는 이러한 네이밍 규칙을 함수를 나타낼 때 사용한다. 즉, 자바에서의 onClick 함수를 생각해보면 이 onClick 함수는 이벤트 핸들러이다. 하지만 ReactiveX에서 onNext는 이벤트 핸들러가 아니다. onNext는 그냥 onNext 함수의 핸들러인 것이다.  

이런 것처럼 ReactiveX 만의 naming convention은 없으므로 구현된 각 언어의 naming convetion을 따르는 것이 좋다.

<br>


## 🐶 "Hot" Observable과 "Cold" Observable
---


