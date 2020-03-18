---
layout: post
title:  "[RxJava] 🏈 RxJava Operator 심화"
date:   2020-03-09 18:34:10 +0700
categories: [RxJava]
---

> ["RxJava 프로그래밍"](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=116852658)이라는 책과 [ReactiveX Operator Docs](http://reactivex.io/documentation/operators.html)을 참고하여 공부한 내용입니다.😃

<br>

# 🏈 ReactiveX의 Operator을 더 알아보자!

ReactiveX의 연산자에 관한 이전 포스팅인 [Operator](https://choheeis.github.io/rxjava/2020/03/03/RxJavaOperator.html) 을 보면 ReactiveX의 연산자의 종류에 대해 간단히 알 수 있다. 이 포스팅에서 볼 수 있듯이 ReactiveX 연산자의 수는 정말 많아서 카테고리를 나눠놓고 분류해 놓는다. 

이 포스팅에서는 카테고리 별로 나눠진 연산자 중 각 카테고리에는 어떤 연산자들이 있는지 조금 더 많이 알아보자!

연산자(=함수)를 사용하려면 연산자의 원형을 알아야한다. [ReactiveX 공식 홈페이지](http://reactivex.io/)에 들어가보니

![07](https://user-images.githubusercontent.com/31889335/76189696-af6e3e00-621e-11ea-802b-59f80741960c.PNG)

이렇게 ReactiveX를 지원하는 각 언어별 공식 깃헙에 접속할 수 있는 배너가 있다.

Rxjava를 클릭해보면 [RxJava 공식 깃헙](https://github.com/ReactiveX/RxJava) 에 연결되는데 리드미를 읽어보면 현재 ReactiveX 버전인 3버전에서 지원하는 기본 클래스에 대한 문서가 나온다!

![06](https://user-images.githubusercontent.com/31889335/76189827-f52b0680-621e-11ea-9329-4e8f6bcc4460.PNG)

위 기본 클래스 중에서 [Observable에 관련된 클래스](http://reactivex.io/RxJava/3.x/javadoc/io/reactivex/rxjava3/core/Observable.html)를 들어가보면 Observable 클래스 안에 정의되어 있는 연산자들의 원형과 설명을 볼 수 있다!

<br>

# 🏈 생성(Creating) 연산자

생성연산자는 데이터 흐름인 Obseravable을 생성할 때 사용하는 연산자이다.

생성 연산자의 가장 단순한 연산자는 just(), from() 계열의 함수, create() 가 있다.

이 외의 함수들에 대해서도 알아보자!

<br>

## 👉🏾 interval() 함수

[interval()](http://reactivex.io/documentation/operators/interval.html) 함수는 주어진 시간 간격에 따라 일련의 정수를 무한히 방출하는 Observable을 생성해주는 함수이다.

interval() 함수의 마블 다이어그램은 다음과 같다.

![01](https://user-images.githubusercontent.com/31889335/76187987-44bb0380-621a-11ea-8af4-197bd3c86319.PNG)

위 마블 다이어그램을 보면 정해준 시간의 간격에 따라 1씩 증가하는 정수가 발행되는 것을 볼 수 있다.

이 때, 발행되는 정수는 Long객체이다.

[Observable 라이브러리](http://reactivex.io/RxJava/3.x/javadoc/io/reactivex/rxjava3/core/Observable.html)에서 interval() 함수를 찾아보면 interval() 함수의 원형은 총 4가지인 것을 알 수 있고, 각 원형마다 함수의 기능이 조금씩 다르다는 것을 알 수 있다.

이 4가지 중 자주 사용되는 interval() 함수의 원형은 다음 두 가지이다.

![02](https://user-images.githubusercontent.com/31889335/76188442-623c9d00-621b-11ea-9607-29670b516f7e.PNG)

![03](https://user-images.githubusercontent.com/31889335/76188441-61a40680-621b-11ea-9ce4-3545bf4253d6.PNG)

첫 번째 원형은 일정 시간 쉬었다가 데이터를 발행하도록 한다.

두 번째 원형은 첫 번째 원형과 동작은 같지만 최초 지연 시간을 정해줄 수 있다. 

위에서 본 interval() 함수의 마블 다이어그램을 자세히 보면

![04](https://user-images.githubusercontent.com/31889335/76189232-800b0180-621d-11ea-86e8-6d78ccd055a8.PNG)

이렇게 첫 번째 item이 발행되는 것도 일정 간격이 지난 후이다. 이렇게 첫 번째 item이 발행되기 전에 흐르는 일정 시간을 최초 지연 시간이라고 하는데 두 번째 원형을 통해 최초 지연 시간을 0으로 지정하면 이 시간이 없어지게 된다.

따라서 두 번째 원형은 보통 초기 지연 없이 바로 데이터를 발행하기 위해 최초 지연 시간을 0으로 하기 위해 많이 사용된다.

또, interval 함수는 main 쓰레드에서 실행되는 함수가 아니라 Computation(계산) 쓰레드에서 실행되는 함수이다! (이 사실도 Observable 라이브러리에서 찾을 수 있다.)

그렇다면 RxJava를 사용해서 interval() 함수를 사용한 간단한 코드를 작성해보자.

~~~java
public static void main(String[] args) {
    // interval() 함수를 사용하여 100ms 간격으로 0부터 데이터를 발행한 후 map() 함수를 호출하여 1을 더해주고 100을 곱해주어서 100, 200, 300 ... 등의 데이터를 발행한다.
    // filtering 함수인 take 함수를 통해 최초 5개의 데이터만 취한다.
    Observable<Long> source = Observable.interval(100L, TimeUnit.MILLISECONDS)
                .map(data -> (data + 1) * 100).take(5);
    source.subscribe(data -> System.out.println(data));

    // interval 함수는 Computation 쓰레드에서 실행되므로 메인 쓰레드가 잠깐 기다려줘야 한다.
    try {
        // 1초 동안 쉬기
        Thread.sleep(1000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
~~~

위 코드를 보면 Observable을 생성하고, 구독하는 부분은 잘 이해가 된다.

하지만 왜 쓰레드를 sleep() 해주어야 하는지는 의문이였다.(sleep()은 쓰레드를 지정 시간동안 일시정지 시키는 함수이다.)

찾아보니 interval() 함수는 main 쓰레드에서 실행되는 것이 아니라 Computation이라는 계산 쓰레드에서 실행되는 함수이다.

즉, interval()로 생성한 Observable가 계산 쓰레드에서 작동하는 동안 main 쓰레드를 멈춰
주지 않으면 main 쓰레드에서 할 일이 아무것도 없으므로 main 쓰레드가 실행되자마자 종료되어 프로그램이 종료되게 된다.

따라서 원하는 출력 코드를 실행시키기 위해서는 main 쓰레드도 동시에 무슨 일을 하거나 main 쓰레드에서 할 일이 없는 경우에는 잠깐 멈춰주어야 한다.

위 코드를 실행시켜보면

![08](https://user-images.githubusercontent.com/31889335/76191222-3244c800-6222-11ea-9ee7-6aa98c7d6b42.PNG)

이와 같이 출력되는 것을 볼 수 있다.

<br>

## 👉🏾 timer() 함수

[timer()](http://reactivex.io/documentation/operators/timer.html) 함수는 주어진 일정 시간이 지나면 특정한 item 한 개를 방출하는 Observable을 생성할 때 사용된다.

timer() 함수의 마블 다이어그램은 아래와 같다.

![05](https://user-images.githubusercontent.com/31889335/76189493-32db5f80-621e-11ea-9e5f-51e1b8c0283f.PNG)

위 마블 다이어그램을 보면 subscribe()를 하면 주어진 일정 시간이 지나고 하나의 item을 방출하는 모습을 볼 수 있다.

한 개의 item 방출이 끝나면 onComplete() 이벤트가 호출되면서 Observable은 종료되게 된다.

또한 timer() 함수도 interval() 함수와 마찬가지로 Computation 쓰레드에서 실행되는 함수이다.

timer() 함수는 보통 일정 시간이 지난 후에 어떤 동작을 해야 할 때 많이 사용된다.

timer()함수도 여러 원형을 가지고 있는데 그 중

![09](https://user-images.githubusercontent.com/31889335/76191386-9bc4d680-6222-11ea-9107-72d63710a836.PNG)

이러한 원형의 timer() 함수를 RxJava로 작성해보자.

~~~java
public static void main(String[] args) {
    // 500ms 후에 현재 시간을 출력하는 동작을 한다.
    // 500ms 후에 item인 0이 방출되지만 이 0을 실제로 map() 함수에서 사용할 필요가 없으므로 람다 표현식의 인자 이름을 notUsed라고 지었다.
    Observable<String> source = Observable.timer(500L, TimeUnit.MILLISECONDS)
                .map(notUsed -> {
                    return new SimpleDateFormat("yyyy/MM/dd HH:mm:ss").format(new Date());
                });
    source.subscribe(data -> System.out.println(data));
    try {
        Thread.sleep(1000);
    } catch (InterruptedException e) {
         e.printStackTrace();
    }
}
~~~

위와 같은 코드를 실행시킨 결과는 다음과 같다.

![10](https://user-images.githubusercontent.com/31889335/76191914-c7948c00-6223-11ea-96ea-4a212ad92cdd.PNG)

<br>

## 👉🏾 range() 함수

[range() 함수](http://reactivex.io/documentation/operators/range.html)는 지정해준 범위내에 속하는 정수를 item으로 방출하는 Observable을 생성할 때 사용되는 함수이다.

range() 함수의 마블 다이어그램은 다음과 같다.

![11](https://user-images.githubusercontent.com/31889335/76192029-22c67e80-6224-11ea-8a89-641129f5a3de.PNG)

위 그림을 보면 range() 함수 안의 인자로는 n과 m을 지정해주었는데 방출되는 item을 보니 n부터 시작하여 m개의 item을 방출해주는 모습임을 알 수 있다.

즉, range() 함수는 첫 번째 인자로 지정한 숫자부터 두 번째 인자로 지정해준 숫자 만큼의 item을 방출해주는 Observable을 생성한다.

예를 들어 range(1, 5) 라고 하면 Observable은 1부터 차례대로 5개의 숫자를 방출하기 시작한다. 따라서 1, 2, 3, 4, 5까지의 숫자가 방출된다.

또한 range() 함수는 다른 쓰레드에서 실행되지 않고 현재 쓰레드에서 실행되는 함수이다.

range() 함수를 통해 for문이나 while문 같은 반복문을 대체할 수 있다.

그렇다면 RxJava로 range() 함수를 사용하여 반복문을 대체한 코드를 작성해보자.

~~~java
public static void main(String[] args) {
    Observable<Integer> source = Observable.range(1, 10)
                .filter(number -> number % 2 == 0);
    source.subscribe(System.out::println);  
}
~~~

위 코드는 1 ~ 10까지의 수 중 짝수인 수만 출력하게 하는 반복문 대체 코드이다.

실행결과는 다음과 같다.

![12](https://user-images.githubusercontent.com/31889335/76591380-8659f000-6533-11ea-96c6-f9f23f896b9c.PNG)

그리고 range() 함수는 현재 쓰레드에서 실행되기 때문에 Thread.sleep() 함수를 호출해줄 필요가 없다.

<br>

## 👉🏾 defer() 함수

[defer()](http://reactivex.io/documentation/operators/defer.html) 함수는 구독자가 subscribe() 함수를 호출하기 전까지는 Observable을 생성하지 않고 각각의 observer에게 새로운 Observable을 생성해주는 함수이다.

defer() 함수의 마블 다이어그램은 다음과 같다.

![13](https://user-images.githubusercontent.com/31889335/76592181-efdafe00-6535-11ea-83fc-f7ca994aaf8b.PNG)


> defer() 함수 다시 읽어보고 쓰기

<br>

## 👉🏾 repeat() 함수

[repeat()](http://reactivex.io/documentation/operators/repeat.html) 함수는 어떤 특정한 item을 반복적으로 계속 방출하는 Observable을 만들어야 할 때 사용하는 함수이다.

repeat() 함수가 가장 많이 사용되는 경우는 서버와 통신을 할 경우 해당 서버가 살아있는지 확인하는 코드를 작성할 때 사용한다.

> 해당 서버가 살아있는지 확인하는 과정은 ping 이라고 부르거나 heart beat 이라고 부른다!

repeat() 함수의 마블 다이어그램은 다음과 같다.

![15](https://user-images.githubusercontent.com/31889335/76592821-dd61c400-6537-11ea-93c1-e0ba7133a7b1.PNG)

repeat() 함수의 인자에는 반복할 횟수를 넣어준다. 만약 인자에 아무것도 없다면 영원히 반복하게 된다.

<br>



