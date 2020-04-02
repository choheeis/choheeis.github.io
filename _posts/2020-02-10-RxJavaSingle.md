---
layout: post
title:  "[RxJava] 🐭 Single 클래스"
date:   2020-02-10 18:34:10 +0700
categories: [RxJava]
---

> ["RxJava 프로그래밍"](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=116852658)이라는 책과 [ReactiveX Single 클래스 Documentation](http://reactivex.io/documentation/single.html)을 참고하여 공부한 내용입니다.😃

## 🐭 Single 클래스란?
---

RxJava에는 Single 이라는 클래스가 존재하는데 이 클래스는 앞 포스팅에서 알아본 [Observable 클래스](https://choheeis.github.io/rxjava/2020/02/03/RxJavaObservable.html)의 특수한 케이스로 추가적으로 개발되고 있는 클래스이다. 

Single 클래스는 Observable 클래스와  비슷하지만 Single 클래스 만의 특징을 가지고 있다! 

Observable는 item을 연속으로 무한히 발행할 수 있지만 Single 클래스는 단 하나의 item만 발행하거나 하나의 에러 notification만 발행한다는 점이 Single 클래스만의 특징이다.

> item을 발행하는 것을 data를 발행한다고도 부른다.

이런 Single 클래스의 특징 때문에 Single 클래스로 Observable을 생성하면 Observable로 부터 onNext, onError, onCompleted 함수가 호출되는 것이 아니라 __onSuccess__ 와 __onError__ 함수만 호출된다.

<br>

- __onSuccess 함수__

    Single 클래스로 생성된 Observable은 단 한 개의 아이템을 방출하고 이 때 Single은 onSuccess 함수를 호출한다. 
    
    따라서 방출된 단 한 개의 item만이 이 onSuccess 함수와 함께 사용된다.

- __onError__

    onError 함수가 호출되면 Single 클래스로 생성된 Observable이 item을 방출할 수 없게 된 원인을 알 수 있다.

    <br>

Single Observable은 위 두 함수 중 단 하나의 함수만을 딱 한 번 호출하게 된다.

item 방출에 성공할 경우에는 onSuccess 함수 하나만을 호출하고, item 방출에 실패할 경우에는 onError 함수 하나만을 호출하기 때문이다.

그리고 두 함수 중 하나를 호출한 후에는 Single이 종료되고 observer와의 연결도 끊어지게 된다!

<br>


## 🐭 Single 연산자(Operator)을 통한 Single 구성하기
---

Observable과 마찬가지로 Single도 여러 다양한 연산자를 통해 조작할 수 있다.

어떤 연산자들은 Observable과 Single 사이의 interface로 사용되기 때문에 Observable과 Single을 혼합할 수 있게 해준다.

Single과 관련된 다양한 연산자들은 [여기](http://reactivex.io/documentation/single.html) 에서 알아볼 수 있다!

<br>

## 🐭 RxJava 코드로 Single 만들어보기!
---

Single은 Observable을 생성하는 것과 비슷한 방법으로 생성할 수 있다.

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

<br>

## 🐭 Observable에서 Single 클래스 사용하기
---

위에서 Observable과 Single을 혼합할 수 있다는 말이 언급된 적이 있다!

이 말은 Observable에서 Single 클래스를 사용할 수 있다는 말이다.

Observable에서 Single 클래스를 사용한 다양한 코드를 한 번 봐보자.

<br>

~~~java
public static void main(String[] args) {
    Observable<String> source = Observable.just("Hello Single");
    Single.fromObservable(source).subscribe(System.out::println);
}
~~~

위와 같은 코드에서는 먼저 String 형의 item을 발행하는 Observable을 생성하였다.

그리고 이 Observable을 Single로 바꿀 때 사용되는 fromObservable연산자를 사용하여 Single로 바꾼 후, 구독하는 모습이다.

따라서 이 코드의 실행결과는 

![05](https://user-images.githubusercontent.com/31889335/75150153-1849b680-5747-11ea-8877-426b15f56ba1.PNG)

이와 같다!

이 때, just() 함수의 인자로 여러 데이터를 넣게되면 Single 클래스의 특징에서 벗어나는 것이므로 error가 발생한다!

이 외에도 Observable에서 Single을 사용하는 여러 예시 코드가 있으니 필요하다면 자세히 살펴보도록!

~~~java
// 1. Single() 함수를 호출해 Single 객체 생성하기 
// --> 출력결과는 "Hello Single"
    public static void main(String[] args) {
        Observable.just("Hello Single")
            .single("default item").subscribe(System.out::println);
    }


// 2. first() 함수를 호출해 Single 객체 생성하기
// --> 출력결과는 "Red"
    public static void main(String[] args) {
        String[] colors = {"Red", "Blue", "Gold"};
        Observable.fromArray(colors)
                .first("default value").subscribe(System.out::println);
    } 

// 3. 빈 Observable에서 Single 객체 생성하기
// --> 출력결과는 "default value"
    public static void main(String[] args) {
        Observable.empty()
                .single("default value").subscribe(System.out::println);
    }   
~~~

위 3개의 추가 코드에서의 공통점은 모두 Single 객체로 변환되어 item이 단 한 개만 출력된다는 점이다!

> Reactive Programming은 함수형 프로그래밍 기법을 활용하므로 Reactive 연산자를 함수라고 부르기도 한다!
>
> 여기까지 Single 클래스 스터디 끝!! 💗

<br>