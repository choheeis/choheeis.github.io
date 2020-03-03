---
layout: post
title:  "[RxJava] 🥎 Operator"
date:   2020-03-03 18:34:10 +0700
categories: [RxJava]
---

> ["RxJava 프로그래밍"](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=116852658)이라는 책과 [ReactiveX Operator Docs](http://reactivex.io/documentation/operators.html)을 참고하여 공부한 내용입니다.😃

<br>

# 🥎 ReativeX의 연산자(Operator)란?

ReactiveX 공식 홈페이지에 들어가보면 Docs 배너에 Operator 만을 위한 부분이 있다!

![01](https://user-images.githubusercontent.com/31889335/75751687-6a539300-5d6a-11ea-8299-9d5fc6693ec1.PNG)

이제 이 부분을 한 번 읽어보자.

ReactiveX를 구현해놓은 각각의 언어들은 연산자(Operator) 집합 또한 구현해놓았다고 한다.

각각의 언어들은 이 연산자 집합을 구현할 때 겹치는 부분도 있겠지만 또 각 언어만의 연산자도 있다.

또한, 각 언어들은 연산자의 이름을 이미 비슷한 역할을 하는 메소드와 비슷한 이름으로 지으려는 경향이 있다.

ReactiveX의 연산자들은 버전이 Rx 버전이 높아지면서 계속 늘어나고 있다.

또, 이 연산자들을 모두 알아야만 Reactive Programming을 할 수 있는 것도 아니니 필수 연산자들의 개념만 알아두면 된다!

그리고 ReactiveX 연산자들은 언어마다 기능이 크게 다르지 않아서 RxJava의 연산자 하나만 공부하면 RxKotlin이나 RxJS에서도 쉽게 사용할 수 있다.

ReactiveX에서 연산자를 __함수__ 라고 부른다.

자바 관점에서는 형식적으로 Observable 클래스 안에 정의되어 있는 method 이기도 하다.

하지만 함수형 프로그래밍의 원리에 따르면 ReactiveX에서의 연산자는 부수 효과가 없는 순수 함수이다.

따라서 함수라고 하는 것이 조금 더 자연스럽다.

<br>

# 🥎 체인(Chaining) 연산자(Operator)

ReactiveX의 대부분의 연산자들은 Observable을 작동시키고 Observable의 return값으로 무엇인가를 하도록 되어있다.

또 우리는 이 연산자를 Chain의 형태로 차례대로 적용시킬 수 있다.

이 Chain의 형태에서 각각의 연산자들은 이 전 연산자의 작동으로 인한 결과로 Observable을 수정한다.

이 과정을 다른 말로는 __Builder Pattern__ 이라고도 한다.

다시 말하면 어떤 특정한 클래스 안에 있는 다양한 메소드들이 같은 클래스의 item을 작동시킨다는 것이다.

> 이 부분 해석이 좀.. 잘 안되네 나중에 다시 해석해보자.

<br>

# 🥎 ReactiveX의 연산자들

[ReactiveX Operator Docs](http://reactivex.io/documentation/operators.html) 를 보면 모든 연산자를 분류해놓은 리스트와 각각의 상세 설명에 대해 알 수 있고, use case에 따라 어떠한 연산자를 사용하는 것이 더 좋은지를 나타내주는 dicision tree에 대해서도 볼 수 있다!

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

## 🏹 map( ) 함수

![02](https://user-images.githubusercontent.com/31889335/75752520-39745d80-5d6c-11ea-8327-d442bf3198d0.PNG)

변환(Transforming) 연산자에 속해있는 [map()](http://reactivex.io/documentation/operators/map.html) 함수에 대해서 알아보자.

map() 함수는 Observable로부터 방출된 item을 어떠한 함수에 적용시켜서 다시 방출해주는 함수이다.

![03](https://user-images.githubusercontent.com/31889335/75752851-dc2cdc00-5d6c-11ea-8ef4-89c4d83cb581.PNG)

위 마블다이어그램을 봐보면 Observable이 item을 방출할 때 map() 함수 안에 있는 식에 item을 대입한 후 그 결과대로 방출해주는 모습을 볼 수 있다.

즉, 조금 더 자세히 말하면 map() 함수는 원래 Observable이 방출하는 item에 개발자가 지정해준 함수를 적용시킨 __또 다른 Observable__ 을 return 한다.

이렇게 return 된 Observable은 원래 Observable의 각 item들이 함수에 적용된 결과를 item으로 방출하는 Observable이다.

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

