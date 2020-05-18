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

따라서 Rxjava로 스케줄러를 사용하고 싶다면

~~~java
import io.reactivex.rxjava3.schedulers.Schedulers;
~~~

위와 같이 schedulers 패키지의 Schedulers 클래스를 import 해야 한다.

> RxJava의 버전에 따라 지원하는 스케줄러가 있고 지원하지 않는 스케줄러가 있으니 버전을 잘 확인하자!

<br>

1️⃣ newThread(뉴 쓰레드) 스케줄러
---

[Schedulers 클래스 Docs](http://reactivex.io/RxJava/3.x/javadoc/io/reactivex/rxjava3/schedulers/Schedulers.html) 를 참고하여 공부해보자.

위 문서에는 Schedulers 라는 이름의 클래스에 정의된 여러 스케줄러 호출 함수들을 볼 수 있다.

그 중 __newThread()__ 라는 함수에 대해서 알아보자.

이 함수는 이름처럼 새로운 스레드를 생성할 때 호출하는 함수이다.

새로운 스레드를 만들어 어떤 동작을 실행하고 싶을 때 Schedulers.newThread() 함수를 인자로 넣어주면 되고, 이 뉴 쓰레드 스케줄러 함수는 요청을 받을 때마다 새로운 스레드를 생성해준다.

아래 코드는 newThread() 함수를 subscribeOn() 연산자의 인자로 넣어 새로운 쓰레드를 생성하고 그 안에서 작업을 하는 코드이다.

~~~java
public class Test {

    public static void main(String[] args) {
        String[] orgs = {"1", "2", "3"};
        Observable.fromArray(orgs)
                .map(data -> "<<" + data + ">>")
                .doOnNext(data -> System.out.println("Original Data : " + data))
                .subscribeOn(Schedulers.newThread())
                .subscribe(Test::getThread);
        try {
            sleep(500);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        Observable.fromArray(orgs)
                .map(data -> "##" + data + "##")
                .doOnNext(data -> System.out.println("Original Data : " + data))
                .subscribeOn(Schedulers.newThread())
                .subscribe(Test::getThread);
        try {
            sleep(500);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public static void getThread(Object obj){
        System.out.println(Thread.currentThread().getName());
    }
}
~~~

![07](https://user-images.githubusercontent.com/31889335/81553393-f5cb4f00-93bf-11ea-815b-ad6960cdbc9e.PNG)

위 코드를 실행시키면 이와 같은 출력 결과가 나온다. 쓰레드 이름으로 보아 main 쓰레드가 아닌 새로운 1번 쓰레드와 2번 쓰레드에서 각각 동작이 실행된 것을 볼 수 있다.

newThread() 함수를 통해 새로운 쓰레드 생성을 호출하는 방법은 적극적으로 추천하는 방법은 아니다.

왜냐하면 RxJava에는 newThread() 스케줄러 보다 활용도가 높은 계산 스케줄러와 IO 스케줄러 같은 것이 있기 때문이다.

<br>

2️⃣ 계산 스케줄러
---

![09](https://user-images.githubusercontent.com/31889335/81554052-13e57f00-93c1-11ea-9225-f1bc8b74c13c.PNG)

Rx 연산자를 공부하면서 알게 된 interval() 이라는 함수는 사실 기본적으로 계산 스케줄러에서 동작하는 함수이다.

interval() 함수의 원형을 보면 SchedulerSupport 어노테이션을 통해 computation 스케줄러에서 실행된다는 사실을 알 수 있다.

![08](https://user-images.githubusercontent.com/31889335/81553956-ebf61b80-93c0-11ea-9339-21a149b7d93a.PNG)

물론 위처럼 같은 interval() 함수여도 오버로드되어 스케줄러를 custom 할 수 있는 함수도 있다.

계산 스케줄러를 사용하면 '계산' 작업을 할 때 대기 시간 없이 빠르게 결과를 도출할 수 있게 된다.

계산 작업은 I/O(입출력) 작업이 아닌 작업이라고 생각하면 된다.

계산 스케줄러는 Schedulers.compatation() 함수를 통해 호출할 수 있다.

<br>

3️⃣ IO 스케줄러
---

IO 스케줄러는 네트워크 요청을 처리하거나 데이터 베이스 쿼리 작업을 하거나 각종 입출력 작업을 실행하기 위한 스케줄러이다.

계산 스케줄러와 IO 스케줄러의 가장 큰 차이점은 생성되는 스레드 개수가 다르다는 것이다.

계산 스케줄러는 CPU 개수만큼 스레드를 생성하지만 IO 스케줄러는 필요할 때 마다 스레드를 계속 생성한다.

또한, 입출력 작업은 비동기로 실행하여도 결과를 얻기까지 대기 시간이 길다는 특징이 있다.

IO 스케줄러는 Schedulers.io() 함수를 호출함으로써 생성할 수 있다.

<br>

4️⃣ 트램펄린(Trampoline) 스케줄러
---

트램펄린 스케줄러는 새로운 스레드를 생성하지 않고 현재 스레드에 무한한 크기의 대기 행렬(큐)을 생성하는 스케줄러이다.

~~~java
public class Test {

    public static void main(String[] args) {
        String[] orgs = {"1", "2", "3"};
        Observable<String> source = Observable.fromArray(orgs);

        source.subscribeOn(Schedulers.trampoline())
                .map(data -> "<<" + data + ">>")
                .subscribe(Test::getThread);

        source.subscribeOn(Schedulers.trampoline())
                .map(data -> "##" + data + "##")
                .subscribe(Test::getThread);

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

위 코드는 main 쓰레드를 트램펄린 스케줄러를 통해 길게 늘려준 것이다.

![10](https://user-images.githubusercontent.com/31889335/81556200-e4d10c80-93c4-11ea-8200-8271f5322297.PNG)

이렇게 하면 새로운 스레드를 생성하지 않고 main 스레드에서 모든 작업을 실행한다.

대기 행렬인 큐에 작업을 넣은 후 1개씩 꺼내어 동작하므로 첫 번째 구독과 두 번쨰 구독 순서가 바뀌는 경우는 발생하지 않는다.

<br>

5️⃣ 싱글 스레드 스케줄러
---

싱글 스레드 스케줄러는 RxJava 내부에서 단일 스레드를 별도로 생성하여 작업을 처리할 때 사용하는 스케줄러이다.

싱글 스레드는 여러 번 생성 요청이 와도 하나의 스레드를 공통으로 사용한다는 점이 특징이다.

~~~java
public class Test {

    public static void main(String[] args) {
        Observable<Integer> numbers = Observable.range(100, 5);
        Observable<Integer> chars = Observable.range(0, 5);

        numbers.subscribeOn(Schedulers.single())
                .subscribe(Test::getThread);
        chars.subscribeOn(Schedulers.single())
                .subscribe(Test::getThread);
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

위 코드는 두 개의 Observable에 대한 subscribe 작업을 single 스레드에서 처리한 코드이다.

![11](https://user-images.githubusercontent.com/31889335/81556955-23b39200-93c6-11ea-840a-e1384d98dded.PNG)

위 코드의 출력결과는 위와 같다. main 쓰레드가 아닌 새로운 스레드에서 작업되는 모습을 볼 수 있다.

싱글 스레드 스케줄러를 생성하면 여러 개의 Observable 이 있어도 별도로 마련된 단일 스레드에서 작업이 차례대로 실행된다.

<br>

## 🧾 Scheduler 로 콜백 지옥(callback hell) 벗어나기
---

Rx 프로그래밍을 이용하면 서버와 통신하는 네트워크 프로그래밍을 할 때 빈번히 발생하는 __콜백 지옥__ 을 해결할 수 있다.

일단 Rx의 스케줄러를 이용했을 때 장점을 다시 한번 짚어보자.

스케줄러를 이용하면 스레드를 생성하는 것과 같은 비동기 프로그래밍을 해야 할 경우 Callable, Runnable 객체를 실행하는 코드는 필요 없다. 

따라서 Reactive Programming 은 서버와 통신하는 비동기 프로그래밍을 작성할 때 가장 큰 힘을 발휘한다.

<br>

그렇다면 __콜백 지옥이 뭔지__ 에 대해서 알아보자.

예를 들어, 다음과 같이 서버에 http 요청을 하는 java 코드가 있다고 가정해보자.(OkHttp3기반)

~~~java
Request request = new Request.Builder()
    .url("https://~~~")
    .build();

Client.newCall(request).enqueue(new Callback()){

    // 서버 통신 실패시 호출
    @Override
    public void onFailure(Call call, IOException e){
        e.printStackTrace();
    }

    // 서버 통신 성공시 호출
    @Override
    public void onResponse(Call call, Response response) throws IOException{
        Log.i(response.body().string());
    }
}
~~~

이처럼 서버 통신에 사용할 코드에는 기본적으로 성공할 경우와 실패할 경우를 나눠 각각 콜백함수로 작성하곤 한다.

만약 위 코드에서 서버 통신에 성공할 경우에 또 다른 URL로 서버 통신을 하고 싶다고 가정해보자.

즉, 서버 통신을 중첩해야 해결되는 기능이 있는 것이다.

![12](https://user-images.githubusercontent.com/31889335/81897980-ab7de400-95f2-11ea-8b4a-c4bfd6e458c0.PNG)

위 코드만 봐도 콜백 함수를 4번 작성해야 하는 걸 알 수 있다.

하지만 두 번째 서버 통신 성공 시 한 번 더 서버 통신을 해야 한다면??

콜백함수가 총 6개가 생길 것이다.

이 결과 코드의 가독성이 떨어지고 정상적인 로직과 예외 처리도 섞여있을 가능성이 크다.

이런 현상을 콜백지옥(Callback Hell)이라고 한다.

<br>

하지만 이러한 콜백지옥을 Rx 스케줄러를 이용하면 해결할 수 있다.

아래 코드는 RxJava로 콜백 지옥을 해결한 모습을 보여줄 것이다.

~~~java
Observable<String> source = Observable.just("https://~~~")
    .subscribeOn(Schedulers.io())
    .map(OkHttpHelper::get)
    .concatWith(Observable.just("https://~~두번째URL")
        .map(OkHttpHelper::get));
source.subscribe();
~~~

IO 스케줄러를 이용해 네크워크 처리를 할 것이라는 것을 subscribeOn() 연산자를 통해 명시하였고, concatWith() 함수를 통해 현재의 Observable에 새로운 Observable을 결합하여 해결하였다.

위 코드는 OkHttpHelper클래스의 get() 함수에는 서버 통신에 필요한 enqueue() 메소드가 정의되어 있다고 가정한 코드이다.

이처럼 Rx 스케줄러로 네트워크 통신을 처리하면 비즈니스 로직과 비동기 프로그래밍을 분리할 수 있어 프로그램의 효율을 향상시킬 수 있다.

<br>

## 🧾 observeOn() 함수 사용하기
---

지금까지는 subscribeOn() 함수를 통해 스케줄러를 사용한 예시만 공부했었다.

그러나 스케줄러의 핵심은 스케줄러의 종류를 선택한 후 subscribeOn() 과 observeOn() 함수를 호출하는 것이다.

스케줄러 초반에 알아본 아래 마블 다이어그램으로 다시 한번 subscribeOn() 과 observeOn() 의 차이를 짚어보자.

![13](https://user-images.githubusercontent.com/31889335/81899016-d406dd80-95f4-11ea-8ac4-300668e6b370.PNG)

<br>

- subscribeOn() 함수는 Observable에서 구독자가 subscribe() 함수를 호출했을 때 데이터 흐름을 발행하는 스레드를 지정해주는 함수이다.

    subscribeOn() 함수는 처음 지정한 스레드를 고정시키므로 다시 subscribeOn() 함수를 호출해도 두 번째 subsribeOn() 함수는 무시한다는 특징이 있다.

- 반면 observeOn() 함수는 처리된 결과를 구독자에게 전달하는 스레드를 지정해주는 함수이다.

<br>

일단 위 마블 다이어그램에서 가장 먼저 나오는 subsribeOn() 함수는 파란색 스레드를 인자로 가지고 있다.

즉, 위 Observable을 구독자가 subscribe 하면 데이터를 발행하는 스레드는 파란색 스레드로 고정된다는 의미이다.

그렇다면 위 다이어그램에서 가장 먼저 나오는 observeOn() 함수는 주황색 스레드를 인자로 가지고 있다.

즉, observeOn()이 호출된 후부터는 주황색 스레드에서 작업이 실행된다는 의미이다.

따라서 마블 다이어그램의 map() 함수는 스레드 변경과는 상관 없는 함수이므로 계속 주황색 스레드에서 실행이 된다.

마지막으로 분홍색 스레드를 인자로 갖는 observeOn() 이 호출되면 그 다음 데이터 흐름은 분홍색 스레드에서 실행된다.

<br>

지금까지 Rx 프로그래밍의 스케줄러에 대해서 공부해보았다.

안드로이드 앱과 같은 UI 프로그래밍을 하려면 스케줄러에 대한 이해가 필수적이고, 특히 observeOn() 함수가 매우 유용하다!!

<br>

