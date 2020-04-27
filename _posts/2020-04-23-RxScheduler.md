---
layout: post
title:  "[RxJava] 🧾 RxJava에서 스케줄러!"
date:   2020-04-23 18:34:10 +0700
categories: [RxJava]
---

> [ReactiveX 공식 문서 중 Scheduler](http://reactivex.io/documentation/scheduler.html) 와 [RxJava 프로그래밍](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=116852658) 이라는 책을 참고하여 공부한 내용입니다😃

<br>

## 🧾 Scheduler 란??
---

만약 우리가 Observable의 연산자(operator)들의 체이닝(chaining) 안에서 다중 쓰레드를 사용하고 싶다면 그 연산자들에게 특정 쓰레드에서 작동하라고 지시하면 된다!

> 다중 쓰레드를 사용한다는 것은 작업 공간을 여러 개로 나누어 동시에 작업한다는 의미로 비동기처리라고도 한다.

<br>

Rx 연산자들 중에는 매개변수에 Scheduler를 가지는 모양의 연산자들이 있다.

그 중 대표적인 두 가지 연산자를 알아보자.

__subscribeOn__ 이라는 연산자는 특정한 스케줄러를 지정함으로써 어떤 쓰레드에서 Observable의 작동이 시작될지를 결정하는 연산자이다. 

반면에 __observerOn__ 이라는 연산자는 Observable이 사용하고 있는 쓰레드의 아래 단계 쓰레드에 영향을 미치는 연산자이다. 

더 자세히는 Observable이 observer에게 item을 방출하기 위해 사용될 쓰레드를 지정하는 연산자이다.

따라서 연산자 chain 안의 다양한 위치에서 observerOn 연산자를 여러 번 호출할 수 있고, 이로 인해 특정한 연산자들이 서로 다른 쓰레드에서 실행되도록 할 수 있다.

만약 subscribeOn 을 여러 번 호출했다면 가장 마지막에 적힌 subscribeOn 에 따라 Observable의 작업이 시작된다.

다음 마블 다이어그램을 보고 observeOn과 subscribeOn 연산자에 대해서 조금 더 쉽게 이해해보자.

![01](https://user-images.githubusercontent.com/31889335/80064305-b7860100-8572-11ea-874f-1a6e09fcb442.PNG)

위 그림을 보면 처음에는 파란색 쓰레드에서 Observable이 시작된다.

그리고 나서 observeOn 연산자를 통해 이 다음부터 나오는 메소드는 주황색 쓰레드에서 실행하라고 하는 모습이다.

따라서 map() 함수는 주황색 쓰레드에서 실행되어 observer에게 item을 방출한다. 

그 다음 observeOn 연산자를 통해 이 다음부터 나오는 메소드는 주확색 쓰레드에서 실행하라고 하였으므로 item들은 주황색 쓰레드로 방출되게 된다.

이 때, 중간의 subscribeOn은 처음 Observable이 시작되는 쓰레드가 파란색임을 나타낸 것으로 위 그림에서도 파란색 쓰레드에서 시작한 것을 알 수 있다.

<br>
그렇다면 이제 쉬운 코드를 보면서 subscribeOn과 observeOn 연산자에 대해서 자세히 이해해보자.

다음과 같은 코드의 출력 결과는 어떻게 될까?

~~~java
public class Test {

    public static void main(String[] args) {
        Observable.just("Hello", "RxJava3!!").subscribe(Test::getThread);
    }

    // 어느 쓰레드에서 실행되는지 출력해보기 위한 함수
    public static void getThread(Object obj){
        System.out.println(Thread.currentThread().getName());
    }
}
~~~

![02](https://user-images.githubusercontent.com/31889335/80338265-d77d3380-8896-11ea-9952-655842cf7af3.PNG)

Observable에서 observe에게 주는 item은 Hello와 RxJava3!! 이렇게 두 개이므로 getThread 함수는 이 두 item에 대해 두 번 실행될 것이다.

즉, 출력결과를 보면 getThread는 현재 쓰레드인 main 쓰레드에서 실행되고 있는 것을 알 수 있다.

만약 main 쓰레드 외의 다른 쓰레드를 사용하여 비동기로 동작하도록 해주려면?

이 때 스케줄러를 사용하는 것이다.

__스케줄러는 쓰레드를 지정할 수 있게 해주기 때문이다.__

Rx의 스케줄러는 새로운 방식으로 비동기 처리를 할 수 있게 해준다.

기존의 자바로 비동기 프로그래밍을 하기 위해 스레드를 만드는 것은 생각해야 하는 것들이 많아 매우 까다로웠다. 

하지만 Rx로 비동기 처리를 한다면 정말 간결한 코드로 비동기 프로그래밍을 할 수 있을 것이다!

> 자바로 비동기 처리를 안 해봐서 잘 모르겠지만 일단 자바보다 Rx로 비동기 처리 하는게 더 쉽다는 건 알겠다!! ㅋㅋㅋㅋ 

이제 다음 코드를 보고 쓰레드가 바뀌는 과정을 살펴보자.

~~~java
public class Test {

    public static void main(String[] args) {
        String[] objs = {"1", "2", "3"};
        Observable<String> source = Observable.fromArray(objs)
                .doOnNext(data -> System.out.println(Thread.currentThread().getName()))
                .subscribeOn(Schedulers.newThread())
                .observeOn(Schedulers.newThread())
                .map(data -> objs + "#");

        source.subscribe(Test::getThread);

        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public static void getThread(Object obj){
        System.out.println(Thread.currentThread().getName());
    }
}
~~~

위 코드에서 doOnNext 라는 함수는 Observable에서 onNext 이벤트가 발생하기 전에 실행되는 함수이다.

![03](https://user-images.githubusercontent.com/31889335/80340765-3abd9480-889c-11ea-8280-4e5f37b5a660.PNG)

그리고 subscribeOn() 연산자를 통해 observer가 subscribe() 함수로 Observable을 구독할 때 이 Observable의 작동이 시작될 쓰레드를 지정해주는 것이다.

마지막으로 observeOn() 을 통해 이 다음에 나오는 함수들이 실행될 쓰레드를 또 새로운 쓰레드로 바꿔준 것이다.

따라서 observeOn() 뒤에 나오는 map 함수는 기존 Observable 데이터 흐름이 있는 쓰레드가 아닌 새로운 쓰레드레서 실행될 것이다.

위 코드를 실행시켜 보면

![04](https://user-images.githubusercontent.com/31889335/80340992-99830e00-889c-11ea-9f6c-82d58da1360c.PNG)

이렇게 출력결과가 나올 것이다.

doOnNext()에 의해 기존의 데이터 1, 2, 3이 존재하는 쓰레드는 1번 쓰레드이고 그 뒤에 observeOn() 에 의해 map 함수에 적용되어 방출된 item은 subscribe(Test::getThread) 에 의해 새로운 쓰레드에서 실행되었음을 알 수 있게 된다.

따라서!

최초의 데이터 흐름이 발생하는 쓰레드와 map() 함수를 거쳐서 구독자에게 전달되는 쓰레드가 다르게 된다~!

이렇게 Rx를 통해 비동기 처리를 가능하도록 했는데 이 과정에서 subscribeOn() 함수와 observeOn() 함수만 사용했을 뿐 자바에서 사용하는 Runnable이나 Callable 객체를 전달한 적이 없다는 것을 알 수 있다!

> 오?? 진짜 엄청 간단하네!! 일단 Runnable 객체가 안 보이는 것부터 짱이다

이처럼 스케줄러를 활용하는 비동기 프로그래밍의 핵심은 __바로 데이터 흐름이 발생하는 스레드와 처리된 결과를 구독자에게 전달하는 스레드를 분리할 수 있다__ 는 것이다!

그렇다면! 만약 다음 코드와 같이 observeOn() 함수를 사용하지 않아 쓰레드를 바꿔주지 않는다면??

~~~java
public class Test {

    public static void main(String[] args) {
        String[] objs = {"1", "2", "3"};
        Observable<String> source = Observable.fromArray(objs)
                .doOnNext(data -> System.out.println(Thread.currentThread().getName()))
                .subscribeOn(Schedulers.newThread())
                //.observeOn(Schedulers.newThread())
                .map(data -> objs + "#");

        source.subscribe(Test::getThread);

        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public static void getThread(Object obj){
        System.out.println(Thread.currentThread().getName());
    }
}
~~~

![05](https://user-images.githubusercontent.com/31889335/80341780-034fe780-889e-11ea-97fb-e6e4865e4711.PNG)

이렇게 처음 Observable이 작동하기 시작한 스레드에서 map() 함수까지 실행된다! 

<br>

그렇다면 다음과 같이 subscribeOn() 함수를 통해 Observable의 시작 스레드까지 정해주지 않는다면??

~~~java
public class Test {

    public static void main(String[] args) {
        String[] objs = {"1", "2", "3"};
        Observable<String> source = Observable.fromArray(objs)
                .doOnNext(data -> System.out.println(Thread.currentThread().getName()))
                //.subscribeOn(Schedulers.newThread())
                //.observeOn(Schedulers.newThread())
                .map(data -> objs + "#");

        source.subscribe(Test::getThread);

        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public static void getThread(Object obj){
        System.out.println(Thread.currentThread().getName());
    }
}
~~~

위 코드를 실행시켜보면

![06](https://user-images.githubusercontent.com/31889335/80341987-604b9d80-889e-11ea-9bee-490d6405219f.PNG)

이렇게 출력된다. 

즉, 별도의 스케줄러를 지정하지 않으면 모든 동작이 main 스레드에서 동작한다!

그렇다면 이제 어떤 스케줄러들이 있는지 __스케줄러의 종류__ 에 대해서 알아보자!

<br>

## 🧾 스케줄러의 종류
---

Rx는 다양한 스케줄러를 제공한다. 

즉, 용도가 다른 다양한 스레드들을 제공한다는 것이다. 

Rx를 사용하면 특정 스케줄러를 사용하다가 다양한 다른 스케줄러로 변경하기 쉽다는 것이 큰 장점이므로 이것을 잘 활용하면 좋다.

RxJava에서 제공하는 스케줄러는 io.reactivex.schedulers 패키지의 Schedulers 클래스의 정적 팩토리 메서드로 생성할 수 있다.

> RxJava의 버전에 따라 지원하는 스케줄러가 있고 지원하지 않는 스케줄러가 있으니 버전을 잘 확인하자!








