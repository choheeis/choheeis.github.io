---
layout: post
title:  "[RxJava] 🥎 Operator"
date:   2020-03-03 18:34:10 +0700
categories: [RxJava]
---

> ["RxJava 프로그래밍"](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=116852658)이라는 책과 [ReactiveX Operator Documentation](http://reactivex.io/documentation/operators.html)을 참고하여 공부한 내용입니다.😃

<br>

# 🥎 ReativeX의 연산자(Operator)란?

ReactiveX 공식 홈페이지에 들어가보면 Docs 배너에 Operator 만을 위한 부분이 있다!

![01](https://user-images.githubusercontent.com/31889335/75751687-6a539300-5d6a-11ea-8299-9d5fc6693ec1.PNG)

이제 이 부분을 한 번 읽어보자.

<br>

ReactiveX를 구현해놓은 각각의 언어들은 Rx에서 사용되는 연산자(Operator) 집합 또한 구현해놓았다.

각각의 언어들이 이 연산자 집합을 구현할 때 겹치는 부분도 있겠지만 또 각 언어만의 연산자도 있다.

또한, 각 언어들은 연산자의 이름을 이미 비슷한 역할을 하는 그 언어만의 메소드와 비슷한 이름으로 지으려는 경향이 있다.

ReactiveX의 연산자들은 Rx 버전이 높아지면서 그 수가 계속 늘어나고 있다.

하지만 이 연산자들을 모두 알아야만 Reactive Programming을 할 수 있는 것이 아니기 때문에 필수 연산자들의 개념만 알아두면 된다!

그리고 ReactiveX 연산자들은 언어마다 기능이 크게 다르지 않아서 RxJava의 연산자 하나만 공부하면 RxKotlin이나 RxJS에서도 쉽게 사용할 수 있다.

<br>

ReactiveX에서는 연산자를 __함수__ 라고 부르기도 한다!

자바 관점에서는 형식적으로 Observable 클래스 안에 정의되어 있는 method 이기도 하다.

하지만 함수형 프로그래밍의 원리에 따르면 ReactiveX에서의 연산자는 부수 효과가 없는 순수 함수이다.

따라서 함수라고 하는 것이 조금 더 자연스럽다.

<br>

# 🥎 체이닝(Chaining) 연산자(Operator)

ReactiveX의 대부분의 연산자들은 Observable을 입력값으로 가지고 작동시킨 후, 다시 Observable을 return 하도록 되어 있는 경우가 많다.

우리는 Rx 연산자의 이러한 특징을 사용해서 연산자를 Chain의 형태로 작성할 수 있다!

연산자를 Chain의 형태로 작성할 수 있다는 것은

~~~java
Observable.fromArray(balls).map(ball -> ball + "!");
~~~

위 코드와 같이 fromArray() 라는 함수 뒤에 바로 map()이라는 함수가 또 온 것과 같은 형태를 말한다.

Rx 연산자는 인자로 들어오는 값과 반환되는 값이 Observable인 경우가 많아서 이와 같이 여러개의 연산자들을 체인 형식으로 작성해도 연산자의 인자 타입과 반환 타입이 일치하므로 올바른 실행이 가능하다.

즉, 바로 이전의 연산자에서 반환되는 Observable로 다음 연산자의 입력값으로 들어갈 수 있다는 것이다.

이 Chain의 형태로 작성되어 있는 각각의 연산자들은 Observable을 각각 수정한다는 점을 알아두어야 한다.

Rx 연산자의 Chaining 과 비슷하지만 다른 패턴이 하나 있는데 Builder Pattern 이라고 부르는 것이 있다.

이 패턴은 어떤 한 클래스에 정의되어 있는 여러 연산자들이 Chain 형식으로 작성되어 동작하는 형식이다. 

하지만 이 Builder Pattern 과 Rx 연산자의 Chaining 패턴 사이에는 차이점이 있다.

Builder Pattern에서 Chain형식으로 엮어있는 연산자들은 각각 독립적으로 어떠한 객체를 작동시키기 때문에 연산자를 작성하는 순서를 고려하지 않아도 된다.

하지만 Rx 연산자의 Chain형식에서는 각각의 연산자들이 서로 연관되어 Observable을 변형시키기 때문에 연산자를 작성하는 순서가 중요하다.

즉, 이전 연산자의 반환값으로 나온 Observable을 다음 연산자가 사용하는 것이므로 연산자들의 순서를 꼭 고려해야한다.

<br>

# 🥎 ReactiveX의 연산자들

[ReactiveX Operator Documentation](http://reactivex.io/documentation/operators.html) 를 보면 모든 연산자를 분류해놓은 리스트와 각각의 상세 설명에 대해 알 수 있고, use case에 따라 어떠한 연산자를 사용하는 것이 더 좋은지를 나타내주는 dicision tree에 대해서도 볼 수 있다!

ReactiveX의 연산자를 크게 분류해보면

1. __Creating Observable__ 에 사용되는 연산자

    생성 연산자라고도 하며 Observable, Single 클래스 등으로 데이터의 흐름을 만들어내는 함수이다. 

    모든 RxJava 프로그래밍은 이 생성 연산자에서 시작하게 된다.

2. __Transformming Observables__ 에 사용되는 연산자 

    변환 연산자라고도 하며 어떤 입력을 받아서 원하는 출력 결과를 내는 함수이다.

3. __Filtering Observables__ 에 사용되는 연산자

    필터 연산자라고도 하며 입력 데이터 중에 원하는 데이터만 걸러서 받아내는 함수이다.

    즉, Observable으로부터 선택적으로 item을 방출하게 할 때 사용된다.

4. __Combining Observables__ 에 사용되는 연산자

    합성 연산자라고도 하며 생성, 변환, 필터 연산자가 주로 단일 Observable을 다룬다면 합성 연산자는 여러 Observable을 조합하는 역할을 한다. 

    여러 개의 Observable을 생성하고 조합할 때 사용된다.

5. __Error Handling__ 에 사용되는 연산자

    오류 처리 연산자라고도 한다.

6. __Observable Utility__ 연산자

    유틸리티 연산자라고도 한다.

7. __Conditional and Boolena__ 연산자

    조건 연산자라고도 하며 Observable의 흐름을 제어하는 역할을 한다.

8. __Mathematical and Aggregate__ 연산자

    수학과 집합형 연산자라고도 하며 수학 함수와 연관이 있는 연산자이다.

9. __Backpressure__ 연산자

    배압 연산자라고도 하며 배압 이슈에 대응하는 연산자이다.

10. __Connectable Observable__ 에 사용되는 연산자

11. __Operators to Convert Observables__ 에 사용되는 연산자

로 나눌 수 있다.

각 연산자에 대해서는 사이트에 직접 들어가서 확인해보자.

<br>

# 🥎 자주 사용되는 연산자

이제 ReactiveX 연산자들 중 자주 사용되는 연산자 4개에 대해서 간단히 알아보자.

1. map() 함수
2. flatMap() 함수
3. filter() 함수
4. reduce() 함수

<br>

## 🏹 map( ) 함수

![02](https://user-images.githubusercontent.com/31889335/75752520-39745d80-5d6c-11ea-8327-d442bf3198d0.PNG)

변환(Transforming) 연산자에 속해있는 [map()](http://reactivex.io/documentation/operators/map.html) 함수에 대해서 알아보자.

map() 함수는 Observable로부터 방출된 item을 어떠한 함수에 적용시켜서 다시 방출해주는 함수이다.

![03](https://user-images.githubusercontent.com/31889335/75752851-dc2cdc00-5d6c-11ea-8ef4-89c4d83cb581.PNG)

위 마블다이어그램을 봐보면 Observable이 item을 방출할 때 map() 함수 안에 있는 식에 item을 대입한 후 그 결과대로 방출해주는 모습을 볼 수 있다.

조금 더 자세히 말하면 map() 함수는 Observable이 방출하는 item에 개발자가 지정해준 함수를 적용시킨 __또 다른 Observable__ 을 return 해주는 함수이다.

예를 들어, String형 item을 integer형 item으로 변환할 때 map() 이 사용된다.

그렇다면 RxJava에서 map() 함수를 사용하는 예시를 봐보자.

~~~java
    public static void main(String[] args) {
        String[] balls = {"1", "2", "3", "5"};
        
        // 배열 balls를 item으로 갖는 Observable생성하되, item의 모습을 map()에 따라 바꾸어서 생성하기
        Observable<String> source = Observable.fromArray(balls).map(ball -> ball + "!");
        source.subscribe(data -> System.out.println(data));
    }
~~~

이 코드의 실행결과는 

![04](https://user-images.githubusercontent.com/31889335/75753388-0337dd80-5d6e-11ea-9b70-9b3533849e08.PNG)

이와 같다.

item으로 가지고 있던 1, 2, 3, 5 뒤에 모두 !가 붙어 방출된 모습임을 알 수 있다.

이 때, map() 함수의 인자로는 자바 8에서 새로 추가된 람다 표현식을 써준다.

<br>

## 🏹 flatMap( ) 함수

[flatMap()](http://reactivex.io/documentation/operators/flatmap.html) 함수는 위에서 알아본 map() 함수를 좀 더 발전시킨 함수이다. 

하지만 다른 점이 있다면 map() 함수는 입력값을 어떤 함수에 넣어서 변환시켜주는 일대일 함수이지만 flatMap() 함수는 입력값을 함수에 넣으면 결과가 Observable로 나온다는 점이 다르다.

즉, flatMap() 함수의 결과로 나온 Observable은 그 스스로도 item을 방출할 수 있다.

또한 flatMap() 함수는 결과로 나온 Observable이 방출하는 item들을 합칠 수도 있다.

따라서 flatMap() 함수는 일대다 혹은 일대일 Observable 함수이다.

flatMap() 함수의 마블 다이어그램을 다음과 같다.

![05](https://user-images.githubusercontent.com/31889335/76058118-d9c1c080-5fbe-11ea-807c-a5c964d58bcb.PNG)

위 마블 다이어그램에서 나타내는 flatMap() 의 함수는 동그라미가 입력으로 들어오면 다이아몬드 두 개를 방출하는 Observable이 나오는 함수이다.

위 그림의 빨간 네모 표시 부분을 보면 Observable임을 알 수 있을 것이다.

그리고 위 마블 다이어그램에서 flatMap() 함수를 표시하는 네모 박스 아래에 있는 하나의 Observable은 각 결과로 나온 Observable이 하나로 합쳐진 것을 의미한다.

그렇다면 RxJava로 flatMap() 함수를 사용한 예시를 봐보자.

~~~java
public static void main(String[] args) {
    Function<String, Observable<String>> getPicture = ball -> Observable.just(ball + "*", ball + "#");

    String[] balls = {"1", "2", "3"};
    Observable<String> source = Observable.fromArray(balls).flatMap(getPicture);
    source.subscribe(data -> System.out.println(data));
}
~~~

위 코드의 실행결과는 다음과 같다.

![06](https://user-images.githubusercontent.com/31889335/76058757-90727080-5fc0-11ea-9076-14f1facca839.PNG)

balls 배열의 각 원소가 flatMap() 함수의 입력값으로 들어갈때마다 *와 #가 붙어져 나오는 Observable이 생성되고 마지막에는 이 Observable들이 합쳐져서 위와 같은 출력결과가 나오는 것이다!

<br>

## 🏹 filter( ) 함수

[filter()](http://reactivex.io/documentation/operators/filter.html) 함수는 Observable이 모든 item을 방출하는 것이 아니라 어떤 조건에 맞는 item만을 방출하도록 해야할 때 사용하는 함수이다.

즉, Observable이 가지고 있는 item중 원하는 데이터만 걸러내는 역학을 하는 것이다.

filter() 함수의 마블 다이어그램은 아래와 같다.

![10](https://user-images.githubusercontent.com/31889335/76061815-b2232600-5fc7-11ea-97c7-a8ab0921a5e2.PNG)

위 마블 다이어그램을 보면 filter() 함수를 더욱 쉽게 이해할 수 있을 것이다.

filter() 함수의 인자로는 item을 걸러낼 조건이 정의된다.

위 마블 다이어그램에 따르면 10보다 큰 수만이 filter() 함수를 통과하여 방출될 수 있다.

그렇다면 RxJava를 이용하여 filter() 함수를 사용하는 예시를 봐보자.

~~~java
public static void main(String[] args) {
    String[] objs = {"1 circle", "2 diamond", "3 triangle", "4 diamond", "5 circle", "6 hexagon"};

    Observable<String> source = Observable.fromArray(objs).filter(obj -> obj.endsWith("circle"));
    source.subscribe(System.out::println);
}
~~~

위 코드는 배열 objs의 원소 중 마지막에 "circle" 이라는 문자열이 포함된 item만 걸러져서 나오게 하는 코드이다.

이 코드의 실행결과는 다음과 같다.

![11](https://user-images.githubusercontent.com/31889335/76062162-7d639e80-5fc8-11ea-9fa3-e7555bca2c15.PNG)

또, 주어진 데이터 중 짝수만 골라내는 코드를 작성해보자.

~~~java
public static void main(String[] args) {
    Integer[] data = {100, 34, 27, 99, 50};

    Observable<Integer> source = Observable.fromArray(data).filter(number -> number%2 == 0);
    source.subscribe(System.out::println);
}
~~~

위 코드의 실행결과는

![12](https://user-images.githubusercontent.com/31889335/76062356-da5f5480-5fc8-11ea-9a98-b6b2dc8bc828.PNG)

이와 같이 짝수만 잘 걸러져 나옴을 알 수 있다.

filter()과 같이 ReactiveX의 연산자 중 filtering Observables에 속하는 연산자는 

![13](https://user-images.githubusercontent.com/31889335/76062447-11356a80-5fc9-11ea-9c0d-602335c6c862.PNG)

이와 같이 다양하다.

이 중 first(), last(), take(), takeLast(), skip(), skipLast() 들이 자주 사용되는 filtering 함수들이다.

<br>

## 🏹 reduce( ) 함수

[reduce()](http://reactivex.io/documentation/operators/reduce.html) 함수는 Observable이 방출하는 각각의 item에 어떠한 함수를 적용시킨 후 마지막 결과값을 반환해주는 함수이다.

reduce() 함수의 마블 다이어그램을 보고 이해해보자!

![14](https://user-images.githubusercontent.com/31889335/76063057-40001080-5fca-11ea-8b59-5f7030826f90.PNG)

위 다이어그램을 보면 item인 1, 2, 3, 4, 5의 모든 합인 15만이 방출되는 것을 알 수 있다.

reduce() 함수는 Observable이 방출하는 첫 번째 item을 함수에 적용시킨 후 그 결과값을 두 번째 item과 함께 다시 함수에 적용시키는 과정으로 작동한다.

<br>

# 🥎 구구단 만들기

이제 지금까지 알아본 개념들을 사용해서 구구단을 만들어보자.

~~~java
public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    System.out.println("구구단몇단? :");
    int dan = Integer.parseInt(in.nextLine()); // 몇단인지 입력

    // 1 ~ 9 까지의 int형 데이터를 item으로 방출하는 Observable 선언
    Observable<Integer> source = Observable.range(1,9);
    // Observable 구독시작
    source.subscribe(row -> System.out.println(dan + " * " + row + " = " + dan*row));
}
~~~

위 코드는 구구단을 만드는 코드이고 이 코드의 실행결과는

![07](https://user-images.githubusercontent.com/31889335/76060125-ec8ac400-5fc3-11ea-876d-b489ff9d600a.PNG)

이와 같다!

이제 구구단을 만드는 또 다른 방법으로 flatMap() 함수를 이용해서 구구단을 만들어보자.

~~~java
public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    System.out.println("Enter your input : ");
    int dan = Integer.parseInt(in.nextLine());

    Function<Integer, Observable<String>> gugudan = num -> Observable.range(1, 9)
            .map(row -> num + " * " + row + " = " + dan*row);

    Observable<String> source = Observable.just(dan).flatMap(gugudan);
    source.subscribe(System.out::println);
}
~~~

위 코드의 실행결과는 다음과 같다.

![08](https://user-images.githubusercontent.com/31889335/76060645-1395c580-5fc5-11ea-8b32-3677fdebb9e8.PNG)

flatMap() 함수의 예시를 잘 생각해보면서 위 코드를 이해해보면 잘 이해할 수 있을 것이다.

flatMap() 함수에 적용되는 함수를 따로 만들지 않고 flatMap() 의 인자로 바로 만들어보기도 해보자.

~~~java
public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    System.out.println("Enter your input : ");
    int dan = Integer.parseInt(in.nextLine());

    Observable<String> source = Observable.just(dan)
                .flatMap(num -> Observable.range(1, 9).map(row -> num + " * " + row + " = " + dan*row));
    source.subscribe(System.out::println);
}
~~~

위 코드의 실행결과는 

![09](https://user-images.githubusercontent.com/31889335/76061092-21981600-5fc6-11ea-9ad6-137e3ad15ed3.PNG)

이와 같다.

<br>

