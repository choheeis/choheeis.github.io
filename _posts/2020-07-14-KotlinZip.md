---
layout: post
title:  "[Kotlin] 🤐 안드로이드에서 사용되는 코틀린 언어 압축 스터디"
date:   2020-07-14 18:34:10 +0700
categories: [Kotlin]
---

## 🤐 안드로이드 개발에 사용되는 코틀린 언어 압축 스터디
---

<img width="756" alt="01" src="https://user-images.githubusercontent.com/31889335/87916593-9ddf5180-caae-11ea-9b97-b04a98e77b49.png">

[Android Developers - Kotlin 언어 배우기](https://developer.android.com/kotlin/learn?hl=ko) 문서를 통해 안드로이드 개발에 필요한 기초 코틀린 지식을 다시 한번 성립해보자!

<br>

## 🤐 목차
---

* 변수 선언
* 타입 추론
* Null 안전성
* 조건문
* 함수
* 익명함수
* 고차함수
* 클래스
* 클래스 캡슐화
* 자바와의 상호운용성

<br>

##  변수 선언
---

코틀린은 변수를 선언할 때 __var__, __val__ 이 두 가지 키워드를 사용하여 선언한다.

* __val__ : 선언할 데이터의 값이 변경되지 않을 경우 사용 (value의 약자)
* __var__ : 선언할 데이터의 값이 변경될 수 있을 경우 사용 (variable의 약자)

val을 사용하여 변수를 선언하면 선언 이후에 변수의 값을 바꿀 수 없다.

~~~kotlin
// var로 변수를 선언할 경우 값을 10에서 5로 바꿀 수 있다.
var count: Int = 10
count = 5

// val로 변수를 선언할 경우 값을 Kotlin에서 Java로 바꿀 수 없다.
val language: String = "Kotlin"
language = "Android" // 컴파일 에러
~~~

<br>

##  타입 추론
---

코틀린 컴파일러는 변수에 처음 값을 할당할 때 할당된 데이터의 타입을 추론할 수 있다. 

아래 코드를 보고 이해해보자.

~~~kotlin
// 아래 두 코드는 같은 코드이다.
val language: String = "Kotlin"
val language = "Kotlin"
~~~

두 번째 코드에는 String이라는 변수 타입을 명시하지 않았음에도 컴파일시 문제가 되지 않는다.

이러한 코틀린의 타입 추론 기능은 코드의 간결성와 타입 안정성을 보장하기 위해 등장하였다.

<br>

## Null 안전성
---

Kotlin을 제외한 다른 일부 언어에서는 변수의 초기 값을 명시적으로 제공하지 않고도 변수를 선언할 수 있다. 일반적으로 이러한 경우에는 __null__ 값이 변수의 값으로 들어가게 되지만 코틀린의 변수는 기본적으로 null 값을 보유할 수 없다.

따라서 아래 코드를 컴파일하면 에러가 난다.

~~~kotlin
// 컴파일 에러
val language: String = null
~~~

하지만 코틀린에서도 null 값을 보유하게 할 수 있는 방법이 있는데 바로 __Nullable__ 타입으로 변수를 선언하는 것이다.

~~~kotlin
// 컴파일 성공
val language: String? = null
~~~

Nullable 타입은 위 코드와 같이 __?__ 를 변수 타입의 접미사로 붙임으로써 표현할 수 있다.

하지만 Nullable 타입으로 변수를 선언한다는 것은 __NullPointerException(NPE)__ 이 발생할 가능성이 있는 것이므로 이를 염두해두어야 한다!

자바와 코틀린의 차이점 중 하나가 이 null 안전성에 있다.

자바에서는 null이 발견되는 경우 프로그램이 비정상 종료하게 된다.(앱이 꺼져버린다ㅠㅠ) 반면 코틀린에서 Nullable 타입의 변수에 null이 들어오는 경우에는 비정상 종료는 발생하지 않고, NullPointerException만(에러만) 발생하게 된다.

<br>

## 조건문
---

코틀린에서는 조건부 논리를 구현하기 위한 몇 가지 방법을 제공하는데 그 중 가장 일반적인 것은 __if - else__ 문이다. 다른 언어에서 사용되는 if - else 문과 똑같다.

~~~kotlin
if (count == 42) {
    println("first")
} else if (count == 52) {
    println("second")
} else {
    println("third")
}
~~~

하지만 코틀린에서는 일반적인 if - else 기능 외에 반복적인 일을 제거하기 위한 기능이 있다.

~~~kotlin
// answer 변수에 각 조건에 해당하는 String 값이 들어간다.
val answer: String = if (count == 42) {
    "first"
} else if (count == 52) {
    "second"
} else {
    "third"
}
~~~

또한, 코틀린에서는 __삼항 연산자가 없다!__ 따라서 삼항 연산자 대신 이러한 if - else 문을 직접 작성해줘야 한다.

조건부 논리를 구현하기 위한 또 다른 방법 중 조건의 복잡도가 증가할 경우 사용하면 좋은 __when__ 구문도 존재한다.

~~~kotlin
val answer = when {
    count == 42 -> "first"
    count == 52 -> "second"
    else -> "third"
}
~~~

when 구문은 위 코드와 같이 __->__ 화살표로 조건에 대한 결과를 나타낸다.

마지막으로 조건부에서는 코틀린의 강력한 기능 중 하나인 __Smart Conversion(스마트 변환)__ 을 사용할 수 있따.

아래 코드는 null 값 확인을 위한 코드이며 스마트 변환의 예를 잘 보여줄 것이다.

~~~kotlin
val language: String? = null
if(language != null) {
    // language?.toUpperCase() 라고 하지 않아도 된다.
    // 왜? --> 이 if 문의 실행부는 조건에 의하여 language가 null이 아닐 경우임이 확실하므로 변수 language에 절대 null 이 들어올 수 없다고 판단할 수 있기 때문이다!
    println(language.toUpperCase())
}
~~~

위와 같은 현상을 Smart Conversion 이라고 한다.

<br>

## 함수
---

코틀린에서 함수를 선언하는 방법은 다음과 같은 형식을 지켜야 한다.

~~~kotlin
fun generateAnswerString(count: Int): String {
    val answer: String = if (count == 42) {
        "First"
    } else {
        "Second"
    }

    return answer
}
~~~

즉, __fun__ 이라는 키워드를 바로 앞에 쓴 후 __함수 이름__ 을 쓰고, 이름 뒤에는 __괄호__ 를 사용하여 __함수의 매개변수__ 를 정의하고 그 뒤에는 __:__ 을 쓴 후 __반환값의 타입__ 을 작성해주면 된다.

위 코드의 함수를 호출하려면 

~~~kotlin
val answerString = generateAnswerString(42)
~~~

이와 같이 함수 이름과 괄호 안에 함수가 필요로하는 매개변수를 지정하여 호출하면 된다.

코틀린에서는 __함수 선언 단순화__ 라는 기능도 제공해준다.

코틀린에서는 위 코드를 다음과 같이 바꿀 수 있다. 

~~~kotlin
// return 에 if - else 문을 포함시킬 수 있다. = 함수 선언 단순화
fun generateAnswerString(count: Int): String {
    return if (count == 42) {
        "Frist"
    } else {
        "Second"
    }
}
~~~

즉, answer이라는 로컬 변수를 선언하지 않고도 함수의 반환값으로 String형 데이터를 반환할 수 있게 된다.

심지어 함수 선언 단순화의 결과인 위 코드를 더 단순화시킬 수도 있다.

~~~kotlin
// return 키워드를 사용하지 않고 함수에 대입연산자 = 를 사용하여 바로 return 하는 방법
fun generateAnswerString(count: Int): String = if (count == 42) {
    "First"
} else {
    "Second"
}
~~~

<br>

## 익명함수
---

간단한 작업을 담당하는 함수를 따로 만들기에는 함수의 수만 늘어날 것 같은 경우가 있다.

코틀린에서는 이럴 경우 일반적인 함수 형식(fun 키워드로 생성되는 함수)을 따르지 않고 이름 없는 함수를 생성할 수 있다.

~~~kotlin
// stringLengthFunc이라는 변수는 결과적으로 인자로 들어온 문자열의 길이를 담게된다.
// 변수의 반환 형태를 적는 곳에 바로 함수를 작성하는 방법
val stringLengthFunc: (String) -> Int = { input ->
    input.length
}

val answer: Int = stringLengthFunc("Android")
~~~

<br>

## 고차함수
---

코틀린에서 함수는 또 다른 함수를 인자로 가질 수 있다. 이렇게 다른 함수를 인자로 갖는 함수를 __고차 함수__ 라고 한다.

고차 함수같이 다른 함수를 인자로 갖는 패턴은 자바에서 __콜백 인터페이스__ 를 사용할 때와 동일한 기능이여서 응답을 기다려야 하는 통신을 할 때 유용하다.

~~~kotlin
// 함수에는 두 개의 인자가 있는데 그 중 두 번째 인자에 익명 함수가 들어가 있는 모습
// 인자로 들어온 mapper 함수를 stringMapper 함수 내에서 사용할 수 있다.
fun stringMapper(str: String, mapper: (String) -> Int): Int {
    // Invoke function
    return mapper(str)
}

// stringMapper 호출하는 모습
stringMapper("Android", { input -> 
    input.length
})
~~~

고차 함수의 호출은 위 예시와 같지만 __마지막 인자가 함수인 고차함수일 때__ 는 다음과 같은 호출 방식을 많이 따르기도 한다.

~~~kotlin
// 다른 인자들은 괄호 안에 작성하고 마지막 인자인 함수만 괄호 밖으로 빼내는 방식
stringMapper("Android") { input ->
    input.length
}
~~~

<img width="498" alt="02" src="https://user-images.githubusercontent.com/31889335/87916599-a041ab80-caae-11ea-8498-16c2d6064ffa.png">

주로 위 코드와 같이 리스너의 형태가 고차 함수인 경우가 많다!

<br>

## 클래스
---

코틀린에서 클래스를 생성하기 위해서는 __class__ 라는 키워드를 사용하면 된다.

~~~kotlin
class Car
~~~

클래스의 멤버 변수를 생성하려면 다음과 같이 클래스 안에 변수를 생성하면 된다.

~~~kotlin
class Car {
    val wheels = listOf<Wheel>()
}
~~~

위 코드에서 wheels은 __public 변수__ 이다. 즉, 클래스 외부에서 wheels 변수에 접근할 수 있다는 것이다. 만약 접근할 수 없는 변수를 만들고 싶다면 __private__ 키워드를 val 앞에 붙이면 된다.

Car 클래스안의 멤버 변수나 함수들을 사용하기 위해서는 먼저 Car 클래스의 __인스턴스를 생성__ 해야 한다.

인스턴스를 생성하려면 클래스의 생성자를 호출하면 되는데 위 코드의 Car 클래스는 생성자가 따로 명시되어 있지 않으므로 다음과 같이 생성자를 호출하면 된다.

~~~kotlin
val carClass = Car()
~~~

클래스의 멤버변수인 wheels를 고정으로 초기화 하지 않고 맞춤으로 초기화하려면 클래스를 생성할 때 다음과 같이 __맞춤 생성자__ 를 클래스 이름 옆에 괄호로 만들어주면 된다.

~~~kotlin
class Car(val wheels: List<Wheel>)
~~~

<br>

## 클래스 캡슐화
---

객체 지향 개념에서 사용되는 클래스 캡슐화를 코틀린에서도 할 수 있다.

~~~kotlin
class Car(val wheels: List<Wheel>) {
    // lock 변수는 외부에서 접근이 불가한 비공개이다 = 캡슐화
    // 따라서 외부에서 잠금을 해제하려면 외부에서 접근이 가능한 public 함수인 unLock함수를 호출해야 한다.
    private val lock: Lock = ...

    fun unLock(key: Key): Boolean {
        // key값이 유효하면 true를 반환하고 그렇지 않으면 false를 반환한다.
    }
}
~~~

<br>

## 자바와의 상호운용성
---

코틀린의 가장 중요한 점은 __JAVA와 유연하게 상호작용할 수 있다는 점__ 이다. 즉, 코틀린은 __JVM 바이트 코드로 컴파일되기 때문__ 에 기존 자바 라이브러리를 코틀린에서 직접 사용할 수 있다는 의미이다.

또한 대부분의 Android API도 자바로 작성되어 있어 코틀린에서 이 API들을 호출하여 안드로이드 앱 개발을 할 수 있는 것이다.

<br>

> 여기까지 코틀린에 대한 기초적인 지식 및 사용법을 알아보았다! 다시 한번 정리하니 머릿속에 더 박히는 느낌!

