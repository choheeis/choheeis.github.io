---
layout: post
title:  "[RxJava] 🐶 Observable 클래스"
date:   2020-02-03 18:34:10 +0700
categories: [RxJava]
---

> ["RxJava 프로그래밍"](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=116852658)이라는 책과 [ReactiveX documentation](http://reactivex.io/documentation/observable.html)을 참고하여 공부한 내용입니다.😃

## 🐶 Observable 이라는게 RxJava에서는 매우 중요하대! 대체 뭘까!
---

[Reactive Programming에 관한 포스팅](https://choheeis.github.io/rxjava/2020/01/22/RxJavaBasic.html) 에서 알아보았듯이 Reactive Programming은 프로그래머가 작성한 함수들을 어떠한 순서대로 실행하는 방식이 아니라 __여러 함수들이 병렬적으로 일단 실행되고 함수 실행의 결과는 "observers(관찰자)"에 의해 결정된 순서대로 나중에 획득되는 방식이다.__

조금 더 이해되기 쉽도록 알아보자!

Reactive Programming 방식대로 프로그램을 작성할 때는 무작정 함수 호출을 나열하도록 작성하는 것이 아니라 __"Observable(관찰 가능한)"__ 방식으로 작성한다. 

Observable 방식으로 작성한다는 것이 뭘까?

Observable 방식으로 작성하기 위해서는 먼저 observer와 Observable에 대한 이해가 필요하다.

Observable 이라는 영어 단어의 의미는 Observed 라는 영어 단어의 의미와 비교해서 생각하면 쉽게 이해될 것이다.

Observed는 관찰을 통해서 얻은 결과라는 의미를 가지고 있고, Observable은 현재는 관찰되지 않았지만 앞으로 관찰할 가능성 자체를 의미한다. 

그렇기 때문에 Observable은 __관찰자가 관찰할 대상__ 을 의미하게 되고 관찰할 대상은 데이터 흐름이라고 생각하면 된다!😆

위 말들이 조금 어려운 것 같아서 다시 정리해서 말하자면 Observable 방식으로 프로그래밍을 한다는 것은

__"어떤 한명의 관찰자(observer)"__ 가 __"하나의 관찰 대상(Observable)"__ 을 관찰(구독)하고 있는 방식이다. 

> 즉, 관찰자는 구독자가 되는 것이다!

그리고, 상태 변화가 일어나면 그 관찰 대상은 자신을 구독하고 있는 관찰자에게 __item을 방출__ 하거나 __알림을 보낸다!!!__ 이런 방출된 item을 받은 관찰자는 이에 반응하는 것이다.



이러한 방식을 Observer 패턴이라고 한다.

사용자가 버튼을 누르면 프로그래머가 버튼에 미리 등록해 둔 onClick() 메서드를 호출해 원하는 처리를 하게끔 해놓은 것이 Observer 패턴이 적용된 대표적인 사례이다!

어떤 관찰자가 버튼을 관찰하고 있고, 사용자가 버튼을 클릭함으로써 버튼의 상태변화가 바뀌었으므로 관찰자에게 onClick() 이라는 메소드가 방출된 것이기 때문이다.

> 음 어느정도 감은 잡히네..!

<br>

## 🐶 Observable 방식으로 접근하는 것이 좋은 이유?
---

만약 우리가 서로 관련이 없는 여러 작업(task)을 실행해야 한다고 가정해보자.

이 때, 이 작업들을 실행하는 방식으로 Observable 방식을 도입한다면 이 작업들을 하나 하나 차례대로 실행시켜 어떤 한 작업이 끝날 때까지 기다렸다가 다른 작업을 실행하는 과정을 반복할 필요가 없어진다!! 

왜냐하면 __Observable 방식을 도입하면 일단 실행시켜야할 여러 작업물을 동시에 모두 실행시켜 놓을 수 있기 때문이다.__

만약 Observable 방식을 도입하지 않아서 어떤 한 작업이 끝날 때까지 기다렸다가 다른 작업을 실행하는 과정을 반복해야 한다면 모든 작업이 끝날때까지는 상당히 많은 시간이 소요될 것이다.

<br>

## 🐶 Observable(관찰대상)이 자신을 관찰하는 구독자에게 전달하는 알림들!
---

Observable(관찰대상)이 구독자에게 전달하는 알림은 총 3가지가 있다.

- __1. onNext__

    이 알림(메소드)은 Observable이 item을 방출할 때마다 전달(호출)된다. 

    __이 메소드의 매개변수로 방출되는 item이 들어있기 때문__ 에 item이 방출될 때마다 호출되는 것이다!

    <br>

- __2. onError__

    이 알림은 발생될 예정이였던 데이터가 발생되는데 실패한 경우나 어떠한 다른 에러가 발생했을 때 전달된다.

    이 메소드의 매개변수에는 어떤 원인에 의해 에러가 발생했는지에 대한 내용이 들어있다.

    이 메소드가 호출되면 더 이상의 onNext나 onCompleted 메소드가 호출되지 않는다. 따라서 Observable이 종료되는 것이다!

    <br>

- __3. onCompleted__

    이 알림은 발생되는 모든 onNext 알림이 다 전달된 후에 마지막으로 전달되는 알림이다. 

    즉, 모든 데이터의 상태 변화가 완료되었다는 의미이다! 

    <br>

## 🐶 Observable 클래스
---

위에서 알아본 것처럼 Observable은 관찰자에게 여러 메소드를 호출함으로써 알림을 전달한다.

Observable이 처리하는 여러 함수들은 RxJava의 __Observable 클래스__ 에 정의되어 있다.

Observable 클래스에는 Observable 자체를 생성하는 함수, 중간 결과를 처리하는 함수, 예외 처리 함수 등등 여러 함수가 존재한다!

그렇다면 이제 RxJava의 Observable 클래스에 정의되어 있는 함수를 사용해보면서 위에서 알게된 것들을 코드로 알아보자!

<br>

## 🐶 Hello RxJava! 출력해보기
---

> --> [Hello RxJava! 출력해본 프로젝트](https://github.com/choheeis/RxJava_Study/tree/master/HelloRxJava)

<br>

[RxJava 시작하기!](https://choheeis.github.io/rxjava/2020/01/22/RxJavaStart.html) 라는 포스팅을 참고하여 실습 프로젝트를 만들고 HelloRxJava라는 클래스 파일을 만들어보자!

만든 HelloRxJava라는 클래스 파일에 다음과 같은 코드를 작성하고 실행시키면

~~~java
import io.reactivex.rxjava3.core.Observable;

public class HelloRxJava {

    public void emit(){
        Observable.just("Hello", "RxJava3!!").subscribe(System.out::println);
    }

    public static void main(String[] args) {
        HelloRxJava helloRxJava = new HelloRxJava();
        helloRxJava.emit();
    }
}
~~~

![01](https://user-images.githubusercontent.com/31889335/73636947-6fa9b900-46aa-11ea-9d43-7a8e0e0e9bfa.PNG)

이렇게 출력된다!

이 코드가 어떻게 출력을 하도록 하는지 알아보자!

일단 RxJava에서 지원하는 Observable 클래스가 import 되어 있는 것을 볼 수 있을 것이다.

그 다음 main() 함수를 보면 HelloRxJava 클래스의 객체를 생성하고 그 객체 안의 멤버 함수인 emit() 함수를 호출하고 있는 것도 볼 수 있다.

이렇게 emit() 함수를 호출했더니 Hello RxJava3!!가 출력되었다면 emit() 함수 안에 출력에 관련된 코드가 있을 것이니 emit() 함수를 자세히 살펴보자!

<br>

emit() 함수만 따로 자세히 보면

~~~java
public void emit(){
        Observable.just("Hello", "RxJava3!!").subscribe(System.out::println);
    }
~~~

emit() 함수 안에서 Observable 클래스를 생성한 모습을 볼 수 있다. 

Observable를 생성할 때는 직접 객체 인스턴스를 만들지 않고 __Observable 안의 멤버 함수를 호출함으로써 Observable이 생성되게끔 작성되어 있는 것을 볼 수 있다.__ 

따라서 Observable 클래스에 정의되어 있는 just() 함수를 호출함으로써 Observable이 생성된 것이다.

just() 함수 외에도 Observable을 생성하는데 사용되는 다른 여러 메소드들이 있는데 그 메소드들에 대해서는 [Creating Observables Docs 문서](http://reactivex.io/documentation/operators.html#creating) 를 참고하면 된다.

> just() 함수와 다른 여러 함수에 대한 자세한 내용은 밑에서 다시 설명해놓았다!

또, [ReactiveX Observable 공식 Docs](http://reactivex.io/documentation/observable.html) 를 보면

![02](https://user-images.githubusercontent.com/31889335/73637527-e2676400-46ab-11ea-93b9-5b72a3ef1f3c.PNG)

Observable을 생성하는 것 외에도 Observable에 관련된 여러 메소드들이 존재한다는 것을 알 수 있다!

<br>

## 🐶 Observable 클래스 안의 여러 멤버 함수들 (feat. 마블 다이어그램)
---

RxJava의 Docs를 보면 어떠한 메소드에 대해 설명할 때 

![03](https://user-images.githubusercontent.com/31889335/73638133-2d35ab80-46ad-11ea-936c-2bf0fb315752.PNG) 

이런 그림이나

![04](https://user-images.githubusercontent.com/31889335/73638132-2c9d1500-46ad-11ea-8d03-1af44b7fffaa.PNG)

이러한 그림으로 설명하는 것을 볼 수 있다.

이런 그림을 __마블 다이어그램__ 이라고 한다.

마블 다이어그램은 RxJava를 이해하는데 필요한 핵심 도구이고, 함수나 리액티브 연산자들을 이해하는데 큰 도움을 주는 다이어그램이다. 

위 두 번째 그림의 마블 다이어그램의 의미를 알아보자.

From 이라고 써있는 박스 위의 여러색깔 동그라미들은 Observable에서 시간순으로 데이터가 발행되는 것을 표현한 것이다. 즉, 빨간색 동그라미로 표시된 데이터부터 발행되기 시작하고 보라색 동그라미로 표시된 데이터까지 발행된 후 Observable이 종료된다.

이 데이터들이 발행될 때마다 onNext 알림이 발생하고, 보라색 동그라미 데이터까지 다 발행되면 onCompleted 알림이 발생하는 것이다.

그리고 From 이라고 써있는 박스는 From() 이라는 함수를 나타낸 것이다. 빨간색 동그라미로 표시된 데이터부터 From()함수의 입력으로 들어오고 그에 대한 출력 데이터들이 From 박스 아래에 차레대로 표시된다. 

이제 위에서 본 emit() 함수 안의 RxJava 코드의 함수들을 이렇게 마블 다이어그램으로 이해해보자!

~~~java
public void emit(){
        Observable.just("Hello", "RxJava3!!").subscribe(System.out::println);
    }
~~~

<br>

- __1. just()__

    > --> [just() 함수 Docs](http://reactivex.io/documentation/operators/just.html) 참고!

    ![05](https://user-images.githubusercontent.com/31889335/73908827-38c3e500-48ee-11ea-974c-c2848784c0e6.PNG)

    just() 함수의 마블 다이어그램은 위와 같다.

    just 함수는 인자로 넣은 데이터를 차례대로 발행하기 위해 Observable을 생성한다. 또, 데이터가 발행될 때마다 onNext 알림이 자동으로 전달된다!

    just 함수에 인자로 들어간 데이터는 데이터 내용이 변경되지 않고 그대로 발행되기 때문에 'just(그냥)' 인 것이다.

    인자로 한 개의 값을 넣을 수도 있고, 여러 개의 값(최대 10개)를 넣을 수도 있다. 단! 인자에 들어가는 데이터의 타입은 모두 같아야한다!

    이 때, 실제 데이터의 발행은 subscribe() 함수를 호출해야 시작한다.

    위 emit() 함수 안의 just()함수를 보면 문자열 2개를 인자로 넣어 "Hello" 와 "RxJava3!!" 를 차례대로 발행하려고 하는 것과 같다. 

    만약, emit() 함수가 다음과 같다면

    ~~~java
    public void emit(){
        Observable.just(1, 2, 3, 4, 5, 6).subscribe(System.out::println);
    }
    ~~~

    ![06](https://user-images.githubusercontent.com/31889335/73909446-05825580-48f0-11ea-83c3-93f92aa59170.PNG)

    출력 결과는 이렇게 될 것이다.

    이 때의 마블 다이어그램은 

    ![07](https://user-images.githubusercontent.com/31889335/73909727-c1dc1b80-48f0-11ea-8d96-25258e35c0d9.PNG)

    이렇게 되는 것이다.

    각 동그라미들은 1부터 6까지의 숫자 데이터를 의미한다.

    그렇다면 이제 just() 함수 뒤에 호출되는 subscribe() 함수에 대해 알아보자.

    <br>

- __2. subscribe()__   

    > --> [subscribe() 함수에 대한 Docs](http://reactivex.io/documentation/operators/subscribe.html) 참고

    RxJava에서 subscibe() 라는 함수를 사용하면 내가 동작시키기 원하는 것을 사전에 정의해둔 다음 실제 그것이 실행되는 시점을 조절할 수 있다.

    따라서 subscribe() 함수는 실제로 observer(관찰자)와 Observable(관찰 대상)을 연결해주는 매개체라고 볼 수 있다!

    > 즉, subscribe()는 관찰대상을 구독한다는 의미를 가지고 있다고 생각하면 쉽다!

    위 emit() 함수에서는 just() 함수로 데이터 흐름을 정의해 둔 후 바로 subscribe() 함수를 호출해서 실제로 데이터 흐름이 실행되도록 한 것이다.

    subscribe() 함수는 인자의 개수에 따라 총 4개의 오버로딩된 함수가 존재한다. 다음은

    ![08](https://user-images.githubusercontent.com/31889335/73910520-ef29c900-48f2-11ea-933b-6664c790dc99.PNG)

    emit() 함수에서는 subscribe() 함수의 인자로 System.out::println 이 들어가 있으므로 위 subscribe() 함수 중 인자가 1개 들어가있는 subscribe() 함수가 실행될 것이다. 

    즉, onNext 알림으로 자바의 System.out::println 출력함수가 실행되는 것이다. 

    따라서 just()로 정의된 데이터 흐름의 데이터가 바뀔 때마다 onNext 알림이 호출되고 이 알림은 println이 되므로 

    ![01](https://user-images.githubusercontent.com/31889335/73636947-6fa9b900-46aa-11ea-9d43-7a8e0e0e9bfa.PNG)

    이렇게 출력되는 것이다!!

    > 즉, println은 onNext으로 구독자에게 전해진 알림이고 구독자는 이 println을 실행하는 주체라고 보면 될 것 같다! 구독자는 구독자라는 실체가 있는 것이 아니라 observable 개념을 이해하기 위해서 만들어낸 대상이다. 따라서 그냥 println이 실행되는 것 자체가 구독자가 observable이 전달한 알림에 반응하는 것이라고 이해하면 된다.

    <br>

- __3. 그 외의 알아두면 좋을 Observable 클래스 안에 정의되어 있는 함수들__

    - [create() 함수](http://reactivex.io/documentation/operators/create.html) --> just() 함수는 onNext 알림이 자동으로 전달되지만 create() 함수를 사용하면 onNext, onComplete, onError 등의 알림을 개발자가 직접 호출해야 한다.

    - fromArray() 함수
    
    - fromIterable() 함수

    - fromCallable() 함수

    - fromFuture() 함수

    - fromPubilsher() 함수

    <br>

    > 오오 Observable 은근 이해가 잘 되었다!!!!🙆

    <br>
    <br>

