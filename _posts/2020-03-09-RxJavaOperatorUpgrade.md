---
layout: post
title:  "[RxJava] 🏈 RxJava Operator 심화"
date:   2020-03-09 18:34:10 +0700
categories: [RxJava]
---

> ["RxJava 프로그래밍"](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=116852658)이라는 책과 [ReactiveX Operator Documentation](http://reactivex.io/documentation/operators.html)을 참고하여 공부한 내용입니다.😃

<br>

# 🏈 ReactiveX의 Operator을 더 알아보자!

ReactiveX의 연산자에 관한 이전 포스팅인 [Operator](https://choheeis.github.io/rxjava/2020/03/03/RxJavaOperator.html) 을 보면 ReactiveX의 연산자의 종류에 대해 간단히 알 수 있다. 

ReactiveX 연산자의 수는 정말 많아서 카테고리를 나누어 분류해 놓기도 한다.

이 포스팅에서는 카테고리 별로 나눠진 연산자 중 각 카테고리에는 어떤 연산자들이 있는지 조금 더 많이 알아볼 것이다!

연산자(=함수)를 사용하려면 연산자의 원형을 알아야한다. [ReactiveX 공식 홈페이지](http://reactivex.io/)에 들어가보니

![07](https://user-images.githubusercontent.com/31889335/76189696-af6e3e00-621e-11ea-802b-59f80741960c.PNG)

이렇게 ReactiveX를 지원하는 각 언어별 공식 깃헙에 접속할 수 있는 배너가 있다.

Rxjava를 클릭해보면 [RxJava 공식 깃헙](https://github.com/ReactiveX/RxJava) 에 연결되는데 리드미를 읽어보면 현재 ReactiveX 버전인 3버전에서 지원하는 기본 클래스에 대한 문서가 나온다!

![06](https://user-images.githubusercontent.com/31889335/76189827-f52b0680-621e-11ea-9329-4e8f6bcc4460.PNG)

위 기본 클래스 중에서 [Observable에 관련된 클래스](http://reactivex.io/RxJava/3.x/javadoc/io/reactivex/rxjava3/core/Observable.html)를 들어가보면 Observable 클래스 안에 정의되어 있는 연산자들의 원형과 설명을 볼 수 있다!

그럼 이제부터 각 카테고리에 속하는 연산자들을 몇 가지 알아보자.

<br>

# 1️⃣ 생성(Creating) 연산자

Rx 연산자의 첫 번째 카테고리는 생성연산자이다.

생성연산자는 데이터 흐름인 Obseravable을 생성할 때 사용하는 연산자이다.

생성 연산자의 가장 단순한 연산자는 just(), from() 계열의 함수, create() 가 있다.

이 외의 함수들에 대해서도 알아보자!

1. interval() 함수

2. timer() 함수

3. range() 함수

4. defer() 함수

5. repeat() 함수

<br>

## 👉🏾 interval() 함수

[interval()](http://reactivex.io/documentation/operators/interval.html) 함수는 주어진 시간 간격에 따라 일련의 정수를 무한히 방출하는 Observable을 생성해주는 함수이다.

interval() 함수의 마블 다이어그램은 다음과 같다.

![01](https://user-images.githubusercontent.com/31889335/76187987-44bb0380-621a-11ea-8af4-197bd3c86319.PNG)

위 마블 다이어그램을 보면 인자로 정해준 시간의 간격에 따라 0부터 1씩 증가하는 정수가 발행되는 것을 볼 수 있다.

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

따라서 두 번째 원형은 보통 초기 지연 없이 바로 데이터를 발행하기 위해 최초 지연 시간을 0으로 해서 많이 사용된다.

또, interval 함수는 main 쓰레드에서 실행되는 함수가 아니라 Computation(계산) 쓰레드에서 실행되는 함수이다! (이것도 Observable 라이브러리에 명시되어 있다.)

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

# 2️⃣ 변환(Transforming) 연산자

변환 연산자라는 것은 [Rx의 연산자 포스팅](https://choheeis.github.io/rxjava/2020/03/03/RxJavaOperator.html)에서 살짝 소개했던 Rx 연산자 카테고리 중 하나이다.

변환 연산자는 생성 연산자로 생성된 Observable(데이터 흐름)을 원하는 대로 변형할 수 있도록 해주는 연산자이다.

1. concatMap() 함수

2. switchMap() 함수

3. scan() 함수

4. groupBy() 함수

<br>


## 👉🏽 concatMap() 함수

concatMap() 함수는 [Rx 연산자 포스팅](https://choheeis.github.io/rxjava/2020/03/03/RxJavaOperator.html)에서 알아본 flatMap() 과 매우 비슷하다.

flatMap() 은 먼저 들어온 데이터를 처리하는 도중에 새로운 데이터가 들어오면 나중에 들어온 데이터의 처리 결과가 먼저 출력될 수도 있다.

이것을 interleaving(인터리빙, 끼어들기)라고도 한다.

하지만 concatMap() 함수는 먼저 들어온 데이터 순서대로 처리해서 결과를 낼 수 있도록 보장해주는 함수이다.

flatMap() 과 concatMap() 함수의 차이를 마블 다이어그램을 통해 알아보자.

일단 먼저 flatMap() 함수의 마블 다이어그램은 다음과 같다.

![16](https://user-images.githubusercontent.com/31889335/76926740-715cd280-6920-11ea-8d94-e119c1acb32b.PNG)

이 마블 다이어그램을 보면 초록 동그라미가 입력되어 두 개의 초록 다이아몬드가 출력되어야 하는데 두 번째 초록 다이아몬드가 출력되기 전에 파란 동그라미가 입력된 모습이다.

이 때, flatMap() 은 interleaving을 허용하므로 출력되는 Observable은 초록, 파랑, 초록 파랑 순으로 순서가 교차되게 된다.

하지만 concatMap() 함수의 마블 다이어그램을 보면 concatMap() 함수만의 특징을 바로 알 수 있을 것이다.

![17](https://user-images.githubusercontent.com/31889335/76926858-c0a30300-6920-11ea-81f0-1abc3ed69d1c.PNG)

위 마블 다이어그램이 concatMap() 함수의 마블 다이어그램인데 초록 동그라미 다음에 바로 파란 동그라미가 입력되어도 출력 Observable 에는 초록 동그라미 2개, 파란 동그라미 2개 순으로 출력되는 모습을 볼 수 있다!

즉, concatMap() 함수는 먼저 들어온 데이터 순서대로 처리해주는 함수인 것이다.

그렇다면 concatMap() 함수를 Rxjava로 사용한 예시를 봐보자!

~~~java
public static void main(String[] args) {
    String balls[] = {"1", "2", "3"};

    // 1, 2, 3이 차례대로 방출되는 Observable이 생성된다
    Observable<String> source = Observable.interval(100L, TimeUnit.MILLISECONDS)
            .map(Long::intValue).map(idx -> balls[idx])
            .take(balls.length)

            // 여기부터 1, 2, 3을 방출하는 Observable를 변형시킨다.
            .concatMap(ball -> Observable.interval(200L, TimeUnit.MILLISECONDS)
                .map(notUsed -> ball + "@")
                .take(2)
            );

    source.subscribe(System.out::println);
    try {
         Thread.sleep(2000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
~~~

위 코드의 1, 2, 3이 100ms 간격으로 발생하지만 변형시킨 Observable은 200ms 간격으로 발생하기 때문에 입력과 출력의 순서가 역전될 수 있다.

하지만 그것을 concatMap() 함수로 잡아준 것이고, 위 코드의 실행결과는 

![18](https://user-images.githubusercontent.com/31889335/76927541-99e5cc00-6922-11ea-9893-ed45af59612b.PNG)

이와 같다!

하지만 concatMap() 함수는 flatMap() 함수보다 수행 시간이 느리다.

왜냐하면 flatMap()은 interleaving을 허용하지만 concatMap()은 허용하지 않아 순서를 맞춰주는데 필요한 추가 시간이 들기 때문이다.

<br>

## 👉🏽switchMap() 함수

switchMap() 함수는 concatMap() 함수처럼 순서를 보장하지만 순서를 보장하기 위해 기존에 진행 중이던 작업을 중단한다.

따라서 여러 개의 값이 발행되었을 때 마지막에 들어온 값만 처리하고 싶을 때 사용하는 함수이다.

일단 무슨 말인지 잘 이해가 되지 않으므로 바로 마블 다이어그램을 봐보자.

![19](https://user-images.githubusercontent.com/31889335/76928219-32308080-6924-11ea-9266-dc35f5d350b1.PNG)

위 마블 다이어그램을 보면 초록 동그라미가 들어오고 그 결과값으로 Observable이 나오는데 이 때 파란 동그라미가 들어와 겹치게 된다. 

그러면 초록 사각형 발행을 중단하고 파랑 다이아몬드를 처리하는 것을 볼 수 있다.

아래 RxJava로 switchMap() 함수를 이용한 예시를 봐보자.

~~~java
public static void main(String[] args) {
    String[] balls = {"1", "2", "3"};

     Observable<String> source = Observable.interval(100L, TimeUnit.MILLISECONDS)
            .map(Long::intValue).map(idx -> balls[idx])
            .take(balls.length)

            .switchMap(ball -> Observable.interval(200L, TimeUnit.MILLISECONDS)
                .map(notUsed -> ball + "#")
                .take(2)
            );

    source.subscribe(System.out::println);
    try {
        Thread.sleep(2000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
~~~

위 코드를 보면 1, 2, 3이 100ms 시간 간격에 따라 하나씩 방출되어 새로운 Observable의 데이터 흐름이 되는데 새로운 Observable은 200ms 씩 item을 방출하는 형태이므로 3이 방출될 때 1의 출력과 겹치게 된다. 

이 때, switchMap()을 사용했으므로 기존에 처리되던 1의 출력 작업은 중단되게 된다.

따라서 위 코드의 출력결과는 다음과 같다.

![20](https://user-images.githubusercontent.com/31889335/76928580-4759df00-6925-11ea-939d-25c624b93b36.PNG)

이 switchMap() 함수는 센서의 값을 얻어와야 하는 경우에 많이 사용된다. 센서값은 중간값보다는 최종적인 값으로 결과를 처리하는 경우가 더 많기 때문이다.

<br>

## 👉🏽groupBy() 함수

[groupBy() 함수](http://reactivex.io/documentation/operators/groupby.html) 는 원래의 Observable을 어떠한 기준을 적용시켜 두 개의 Observable로 나누어주는 함수이다.

groupBy() 함수의 마블 다이어그램을 보면

![21](https://user-images.githubusercontent.com/31889335/76929127-bd127a80-6926-11ea-933c-c09438c7e046.PNG)

이와 같다. 이 그림을 잘 살펴보면 동그라미는 동그라미끼리 삼각형은 삼각형끼리 서로 다른 Observable로 나뉘어지는 모습을 볼 수 있다.

그렇다면 RxJava로 groupBy() 함수를 사용한 예시를 봐보자!

~~~java
public static void main(String[] args) {
    String[] objs = {"6", "4", "2-T", "2", "6-T", "4-T"};

    // GroupedObservable클래스는 Observable클래스와 동일하지만 getKey()라는 메서드를 제공하여
    // 구분된 그룹을 알 수 있게 해준다.
    // Test::getShape은 함수형 프로그래밍에서 인자로 함수를 넣을 때 사용하는 방식 중 하나이다.
    Observable<GroupedObservable<String, String>> source = Observable.fromArray(objs)
            .groupBy(Test::getShape);

    source.subscribe(obj -> {
        obj.subscribe(val -> System.out.println("GROUP:" + obj.getKey() + "\t Value:" + val));
    });
}

public static String getShape(String obj){
    if(obj.endsWith("-T")) return "TRIANGLE";
    else return "BALL";
}
~~~

위 코드의 실행결과는 다음과 같다!

![22](https://user-images.githubusercontent.com/31889335/76930872-d4ebfd80-692a-11ea-9aaf-52855a3e6a97.PNG)

<br>

## 👉🏽scan() 함수

[scan() 함수](http://reactivex.io/documentation/operators/scan.html) 는 [Rx 연산자 포스팅](https://choheeis.github.io/rxjava/2020/03/03/RxJavaOperator.html) 에서 알아본 reduce() 함수와 비슷하다.

reduce() 함수는 Observable에서 모든 데이터가 입력된 후 그것을 종합하여 마지막 1개의 데이터만을 구독자에게 발행했지만 scan() 함수는 실행할 때마다 입력값에 맞는 중간 결과 및 최종 결과를 구독자에게 발행한다.

일단 reduce() 함수의 마블 다이어그램을 다시 한번 봐보자.

![24](https://user-images.githubusercontent.com/31889335/76931118-59d71700-692b-11ea-9178-5d13cfd8c37a.PNG)

reduce() 함수는 1, 2, 3, 4, 5 라는 데이터를 모두 더해서 마지막 결과값인 15만을 구독자에게 넘겨준다.

하지만 scan() 함수의 마블 다이어그램을 보면 reduce() 함수와 다른 점을 볼 수 있을 것이다.

![23](https://user-images.githubusercontent.com/31889335/76931052-38762b00-692b-11ea-910d-2986788daf30.PNG)

위 마블 다이어그램에서와 같이 scan() 함수는 하나의 데이터가 합쳐질 때마다 그 결과를 구독자에게 넘겨준다.

즉, 마지막 데이터인 15만 넘겨지는 것이 아니라 합산된 결과를 그때 그때 넘겨주는 것이다.

그렇다면 RxJava로 scan() 함수를 사용한 예시를 봐보자.

~~~java
public static void main(String[] args) {
    String[] balls = {"1", "2", "3"};
    Observable<String> source = Observable.fromArray(balls)
            .scan((ball1, ball2) -> ball2 + "(" + ball1 + ")");
    source.subscribe(System.out::println);
}
~~~

위 코드의 실행결과는 다음과 같다.

![25](https://user-images.githubusercontent.com/31889335/77033141-c6b0e680-69e9-11ea-8054-94e3a0f8df68.PNG)

<br>

# 3️⃣ 결합(Combining) 연산자

결합 연산자라는 것은 [Rx의 연산자 포스팅](https://choheeis.github.io/rxjava/2020/03/03/RxJavaOperator.html)에서 살짝 소개했던 Rx 연산자 카테고리 중 하나이다.

위에서 보았던 생성연산자와 변환연산자는 한 개의 Observable(데이터 흐름)만을 다뤘다. 하지만 결합 연산자는 여러 개의 Observable들을 조합하여 활용하게 해준다.

1. zip() 함수

2. combineLatest() 함수

3. merge() 함수

4. concat() 함수

<br>

## 👉🏼 zip() 함수

[zip() 함수](http://reactivex.io/documentation/operators/zip.html) 는 여러 개의 Observable이 방출하는 데이터들을 특정한 함수를 통해서 결합시켜주는 함수이다. 

zip() 함수의 마블 다이어그램은 다음과 같다.

![26](https://user-images.githubusercontent.com/31889335/77034382-2c52a200-69ed-11ea-9cc2-2f5a26ce70ae.PNG)

이 마블 다이어그램을 살펴보면 먼저 두 개의 서로 다른 Observable이 존재하는데 결과적으로는 두 Observable에서 방출되는 item이 결합된 또 다른 새로운 Observable이 탄생한다는 것을 알 수 있다.

zip() 함수의 원형을 찾아보면 다음과 같다.

> Observable 클래스에 포함된 함수들을 모두 볼 수 있는 document --> [여기](http://reactivex.io/RxJava/3.x/javadoc/io/reactivex/rxjava3/core/Observable.html)

![27](https://user-images.githubusercontent.com/31889335/77034635-c581b880-69ed-11ea-8c0d-913d59975e74.PNG)

source1, source2 는 서로 다른 Observable이고 zipper 변수는 이 두 Observable을 결합시킬 원하는 함수이다. 

그렇다면 RxJava로 zip() 함수를 사용해 본 예시를 봐보자!

~~~java
public class Test {

    public static final String HEXAGON = "HEXAGON";
    public static final String OCTAGON = "OCTAGON";
    public static final String RECTANGLE = "RECTANGLE";
    public static final String TRIANGLE = "TRIANGLE";
    public static final String DIAMOND = "DIAMOND";
    public static final String PENTAGON = "PENTAGON";
    public static final String BALL = "BALL";
    public static final String STAR = "STAR";

    public static void main(String[] args) {
        String[] shapes = {"BALL", "PENTAGON", "STAR"};
        String[] coloredTriangles = {"2-T", "6-T", "4-T"};

        // 두 Observable을 생성하고 결합하기
        Observable<String> source = Observable.zip(
                Observable.fromArray(shapes).map(Test::getSuffix),
                Observable.fromArray(coloredTriangles).map(Test::getColor),
                (suffix, color) -> color + suffix);

        source.subscribe(System.out::println);

    }

    // 하이푼 전까지의 문자열을 추출하는 함수
    public static String getColor(String shape){
        // 인자로 들어온 shape의 모양이 다이아몬드로 끝날 경우
        if(shape.endsWith("◇")){
            return shape.replace("◇", "").trim();
        }

        // -의 위치를 인덱스로 저장
        int hyphen = shape.indexOf("-");

        if(hyphen > 0){
            return shape.substring(0, hyphen);
        }

        // 원의 경우
        return shape;
    }

    public static String getSuffix(String shape){
        if(HEXAGON.equals(shape)) return "-H";
        if(OCTAGON.equals(shape)) return "-O";
        if(RECTANGLE.equals(shape)) return "-R";
        if(TRIANGLE.equals(shape)) return "-T";
        if(DIAMOND.equals(shape)) return "◇";
        if(PENTAGON.equals(shape)) return "-P";
        if(STAR.equals(shape)) return "-S";

        // 원의 경우
        return "";
    }

}
~~~

위 코드의 실행결과는 다음과 같다.

![28](https://user-images.githubusercontent.com/31889335/77035640-8dc84000-69f0-11ea-9469-6ee593a308aa.PNG)

<br>

## 👉🏼 combineLatest() 함수

[combineLatest() 함수](http://reactivex.io/documentation/operators/combinelatest.html) 는 서로 다른 두 개의 Observable로부터 아이템이 방출되었을 때 각각의 Observable에서 가장 마지막으로 방출된 아이템을 특정한 함수에 적용시켜 결합시켜주는 함수이다. 

또한 combineLatest() 함수의 결과로 결합된 아이템을 방출하는 또 다른 새로운 Observable이 생성된다.

combineLatest() 함수의 마블 다이어그램을 봐보자!

![29](https://user-images.githubusercontent.com/31889335/77036094-9a996380-69f1-11ea-9094-2fc04ba4b328.PNG)

이 마블 다이어그램을 보면 combineLatest() 함수의 기능을 더욱 확실히 알 수 있을 것이다. 

위 마블 다이어그램에서는 일단 서로 다른 두 개의 Observable이 존재한다.

첫 번째 Observable이 1이라는 데이터를 방출하고 난 후에 두 번째 Observable이 A라는 데이터를 방출한다.

그러면 이 시점에서 각 Observable에서 가장 마지막으로 방출된 것은 1과 A이므로 이 둘을 결합하면 1A가 된다.

이어서 첫 번째 Observable에서 2를 방출하였다. 그러면 이 시점에서 각 Observable에서 가장 마지막으로 방출된 것은 2와 A이므로 이 둘을 결합한 것은 2A가 된다.

이어서 두 번째 Observable에서 B를 방출하게 되므로 이 시점에서 각 Observable이 가장 마지막으로 방출한 것은 2와 B가 된다. 

따라서 이 둘이 결합된 2B가 나오게 된다.

이런 식으로 어느 Observable에서 데이터의 변경이 일어나면 그 시점에서의 가장 마지막 데이터가 무엇인지 파악하고 결합해주는 함수가 combineLatest() 함수인 것이다.

이 함수의 원형도 위에서 언급한 문서에서 찾을 수 있으므로 찾아서 사용하면 된다!

<br>

## 👉🏼 merge() 함수

[merge() 함수](http://reactivex.io/documentation/operators/merge.html) 는 여러 개의 Observable을 하나의 Observable로 결합해주는 가장 단순한 함수이다.

각 Observable의 데이터를 변형하지 않고 순서대로 결합해준다.

merge() 함수의 마블 다이어그램을 봐보자.

![30](https://user-images.githubusercontent.com/31889335/77036604-db45ac80-69f2-11ea-853d-ea124f62a8cf.PNG)

이 다이어그램을 보면 서로 다른 두 개의 Observable이 하나의 Observable로 합쳐지는데 이 때 각 Observable의 데이터가 결합되거나 변형되지 않고 순서대로 끼워 넣어지는 모습을 볼 수 있다.

merge() 함수도 원형을 찾아서 인자에 무엇을 넣어야하는지 알아보고 사용하면 된다!

<br>

## 👉🏼 concat() 함수

[concat() 함수](http://reactivex.io/documentation/operators/concat.html) 는 두 개 이상의 서로 다른 Observable들을 이어 붙여주는 기능을 하는 함수이다.

concat() 함수는 서로 다른 Observable을 이어 붙일 때 interleaving(끼어들기, 인터리빙)을 하지 않고 그냥 쭈루룩 이어 붙여준다.

concat() 함수의 마블 다이어그램을 봐보자.

![31](https://user-images.githubusercontent.com/31889335/77036998-a5ed8e80-69f3-11ea-9bdb-c3a39ab1101d.PNG)

위 마블 다이어그램을 보면 두 Observable이 item을 방출하는 시간이 겹치지만 이것을 하나의 Observable로 합칠 때는 겹치는 시간을 고려하지 않고 뒤로 이어서 붙여지는 모습을 볼 수 있다.

여기서 주의할 점은 첫 번째 Observable이 onComplete 되지 않는다면 즉, Observable이 종료되지 않는다면 두 번째 Observable을 첫 번째 Observable이 끝날 때까지 계속 기다려야 하는 상황이 발생한다. 

따라서 잠재적인 메모리 누수의 위험을 내포하고 있다는 것을 알아두어야 하고, 꼭 Observable이 완료될 수 있게 해야 한다.

concat() 함수도 원형을 찾아보고 그에 맞게 사용하면 된다!

<br>


# 4️⃣ 조건 연산자

조건 연산자는 Observable의 흐름을 제어하는 역할을 하는 연산자이다. 

1. amb() 함수

2. takeUntil(other) 함수

3. skipUntil(other) 함수

4. all() 함수

<br>

## 👉🏻 amb() 함수

[amb() 함수](http://reactivex.io/documentation/operators/amb.html) 는 ambiguous(모호한)이라는 영어 단어의 줄임말이다.

이 함수는 여러 개의 Observable 중 item 방출이 가장 먼저 되는 Observable만 선택하고 나머지 Observable은 무시하는 함수이다.

amb() 함수의 마블 다이어그램은 다음과 같다.

![32](https://user-images.githubusercontent.com/31889335/77282919-81085c80-6d0e-11ea-81aa-c32dbf7ac65d.PNG)

이 마블 다이어그램을 보면 총 3개의 Observable이 있는데 그 중 첫 번째 item 방출 시간이 가장 빠른 두 번째 Observable만 선택받고 나머지는 무시되는 모습을 볼 수 있다!

amb() 함수의 원형은 다음과 같다.

![33](https://user-images.githubusercontent.com/31889335/77283013-c4fb6180-6d0e-11ea-8c54-557981ed8c79.PNG)

List처럼 Iterable\<Observable\<T>> 객체를 인자로 넣으면 그 중에서 가장 먼저 데이터를 발행하는 Observable만 선택해서 계속 값을 발행하도록 해준다.

그렇다면 Rxjava로 amb() 함수를 사용한 예시를 봐보자!

~~~java
public class Test {

    public static void main(String[] args) {
        String[] data1 = {"1", "3", "5"};
        String[] data2 = {"2-R", "4-R"};

        // 1) sources 변수에 data1을 데이터로 발행하는 Observable과
        //    100ms 동안 기다렸다가 data2를 데이터로 발행하는 Observable을 넣는다.
        List<Observable<String>> sources = Arrays.asList(
                Observable.fromArray(data1)
                    .doOnComplete(() -> System.out.println("Observable #1 : onComplete()")),
                Observable.fromArray(data2)
                    .delay(100L, TimeUnit.MILLISECONDS)
                    .doOnComplete(() -> System.out.println("Observable #2 : onComplete()"))
        );

        // 2) sources 변수를 amb() 함수의 인자로 넣는다.
        Observable.amb(sources)
                .doOnComplete(() -> System.out.println("Result : onComplete()"))
                .subscribe(System.out::println);

        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

}
~~~

위 코드의 실행결과는 

![34](https://user-images.githubusercontent.com/31889335/77283535-f294da80-6d0f-11ea-8df8-9ca5eda14b7d.PNG)

이와 같다.

<br>

## 👉🏻 takeUntil() 함수

[takeUntil() 함수](http://reactivex.io/documentation/operators/takeuntil.html) 는 [take()](http://reactivex.io/documentation/operators/take.html) 라는 함수에 조건을 설정할 수 있는 함수이다.

더 정확하게는 takeUntil() 함수의 인자로 받은 Observable에서 어떤 값을 발행하면 현재 Observable의 데이터 발행을 중단하고 즉시 완료 (onComplete) 하게 해준다.

takeUntil() 함수의 마블 다이어그램을 봐보자.

![35](https://user-images.githubusercontent.com/31889335/77283752-78188a80-6d10-11ea-81d7-87b554a15075.PNG)

위 마블 다이어그램을 보면 알 수 있듯이 인자로 받은 걸로 보여지는 두 번째 Observable에서 첫 번째 item이 방출되는 순간 현재 Observable가 종료되는 것을 볼 수 있다.

즉, take() 함수처럼 일정 개수만 값을 발행하도록 하는 대신 그 기준을 다른 Observable에서 값을 발행하는지로 판단하는 것이다.

takeUntil() 함수의 원형은 다음과 같다.

![36](https://user-images.githubusercontent.com/31889335/77283905-cc236f00-6d10-11ea-8a4e-c576b3006bda.PNG)

즉, 마침의 기준이 되는 other Observable이 필요한 것이다!

그렇다면 RxJava로 takeUntil() 함수를 사용해보자.

~~~java
public class Test {

    public static void main(String[] args) {
        String[] data = {"1", "2", "3", "4", "5", "6"};

        // 1) 현재 Observable은 100L 마다 데이터를 방출한다.
        // 2) 기준이 되는 Other Observable은 500L 마다 데이터를 방출한다.
        Observable<String> source = Observable.fromArray(data)
                .zipWith(Observable.interval(100L, TimeUnit.MILLISECONDS), (val, notUsed) -> val)
                .takeUntil(Observable.timer(500L, TimeUnit.MILLISECONDS));

        source.subscribe(System.out::println);
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

}
~~~

위 코드에서 기준이 되는 Other Observable은 500L 마다 데이터를 방출하므로 현재 Observable이 데이터 5를 방출할 때 처음 데이터가 방출되게 된다. 따라서 현재 Observable은 이 시점에서 종료된다.

위 코드의 실행결과는 

![37](https://user-images.githubusercontent.com/31889335/77284492-27a22c80-6d12-11ea-90b6-14fdbd959b83.PNG)

이와 같다.

<br>

## 👉🏻 skipUntil() 함수

[skipUntil() 함수](http://reactivex.io/documentation/operators/skipuntil.html) 는 takeUntil() 함수와 정반대의 기능을 가진 함수이다.

인자로 기준이 되는 other Observable을 받는다는 점은 같지만 skipUntil() 함수는 other Observable에서 첫 데이터를 발행할 때까지 현재 Observable이 방출하는 데이터를 무시한다.

skipUntil() 함수의 마블 다이어그램은 다음과 같다.

![38](https://user-images.githubusercontent.com/31889335/77284643-810a5b80-6d12-11ea-9c06-0b52e3ab24a7.PNG)

위 마블 다이어그램을 보면 알 수 있듯이 skipUntil() 함수는 기준이 되는 Observable로 보이는 두 번째 Observable의 첫 데이터가 방출되기 전까지 현재 Observable에서 방출되는 데이터를 무시한다.

<br>

## 👉🏻 all() 함수

[all() 함수](http://reactivex.io/documentation/operators/all.html) 는 방출되는 모든 item이 주어진 조건에 100% 맞을 때만 true 값을 발행하고 조건에 맞지 않는 데이터가 하나라도 발행되면 바로 false 값을 발행하는 함수이다.

all() 함수의 마블 다이어그램은 다음과 같다.

![39](https://user-images.githubusercontent.com/31889335/77284806-e3fbf280-6d12-11ea-818a-3221d0d170a6.PNG)

위 마블 다이어그램을 보면 알 수 있듯이 all() 함수에 들어오는 모든 데이터가 10보다 작으므로 마지막에 true가 발행된 것이다.

all() 함수의 원형은 다음과 같다.

![40](https://user-images.githubusercontent.com/31889335/77285334-135f2f00-6d14-11ea-867f-ce50e196ba09.PNG)

<br>


# 5️⃣ 기타 연산자

RxJava에는 유독 시간을 다루는 함수들이 많다.

1. delay() 함수

2. timeInterval() 함수

<br>

## 👉 delay() 함수

[delay() 함수](http://reactivex.io/documentation/operators/delay.html) 는 시간을 인자로 받아서 그 시간 만큼이 지난 후로 item들을 미뤄주는 함수이다.

delay() 함수의 마블 다이어그램은 다음과 같다.

![41](https://user-images.githubusercontent.com/31889335/77285785-2de5d800-6d15-11ea-84ac-6a9bdfec9809.PNG)

이 다이어그램을 보면 알 수 있듯이 일정 시간 후로 item 방출이 밀려있다.

delay() 함수의 원형을 찾아보고 적합하게 사용하면 된다!

<br>

## 👉 timeInterval() 함수

[timeInterval() 함수](http://reactivex.io/documentation/operators/timeinterval.html) 는 어떤 데이터 값이 발행되었을 때 그 이전 값이 발행된 이후 얼마나 시간이 흘렀는지를 알려주는 함수이다. 

timeInterval() 함수의 마블 다이어그램은 다음과 같다.

![42](https://user-images.githubusercontent.com/31889335/77285906-861cda00-6d15-11ea-8fea-90ae4f786200.PNG)

위 마블 다이어그램에서 알 수 있듯이 timeInterval() 함수는 어떤 데이터가 발행될 때 그 이전의 시간 간격을 알려준다.

<br>

> 지금까지 Rx에서 지원하는 Observable클래스 안에 정의되어 있는 연산자(함수)들의 대장정이였다!!!
> 
> Rx에는 이것들 외에도 무수히 많은 함수들이 있으므로 사용할 수 있는 함수들의 가능성을 열어두자~! 👍

