---
layout: post
title:  "[Kotlin] ⭐️ Inner class"
date:   2020-05-29 18:34:10 +0700
categories: [Kotlin]
---

<br>

## Inner class 가 뭘까?
---

이전에는 안드로이드 개발을 하며 recyclerView를 위한 Adapter를 만들 때 ViewHolder 클래스를 Adapter 클래스와 따로 분리했었다.

하지만 프로그라피 6기 활동을 하면서 recyclerView의 수가 많아질수록 Adapter 클래스도 많아지고 동시에 ViewHolder 클래스도 많아지게 되므로 ViewHolder 클래스를 Adapter 클래스 안으로 넣어버리자는 의견이 나왔었다.

ViewHolder 클래스를 Adapter 클래스 안으로 넣기 위해 사용한 코틀린 예약어는 __inner class__ 였다.

이 때 사용한 inner class에 대해서 kotlin 문서를 보고 공부해보자!

<br>

![01](https://user-images.githubusercontent.com/31889335/83166158-7670a600-a149-11ea-9189-d79500666fb1.PNG)

inner class에 대한 설명은 코틀린 클래스와 코틀린 객체 카테고리 안의 [Nested Classes](https://kotlinlang.org/docs/reference/nested-classes.html) 부분을 읽어보면 된다.

이 문서를 읽어보니 __코틀린 클래스는 하나의 클래스 안에 중첩으로 다른 클래스를 포함할 수 있다__ 고 설명되어 있었다.

아래 코드를 보고 더 자세히 이해해보자.

~~~kotlin
class Outer{
    private val bar: Int = 1

    class Nested{
        fun foo() = 2
    }
}

// demo = 2
val demo = Outer.Nested().foo()
~~~

위 코드를 보면 Outer라는 클래스 안에 Nested 라는 클래스가 중첩되어 있는 것을 확인할 수 있다.

Nested 클래스는 멤버 함수로 foo() 라는 함수를 가지고 있다.

이 foo() 함수를 어떻게 사용하는지에 대해서는 demo 변수를 초기화 하는 부분을 보면 된다.

<br>

문서를 계속 읽어보니 이렇게 하나의 클래스 안에 포함된 다른 클래스를 __inner__ 이라는 예약어로도 표시할 수 있다고 설명되어 있었다.

다만 inner 예약어를 사용하여 중첩 클래스를 나타낼 경우에는 중첩 클래스에 접근하는 방법이 위와 조금 달라진다. 

아래 코드를 통해 어떤 차이가 있는지 알아보자!

~~~kotlin
class Outer{
    private val bar: Int = 1

    inner class Nested{
        fun foo() = bar
    }
}

// demo = 1
val demo = Outer().Nested().foo()
~~~

위 코드와 첫 번째 코드는 다른 점이 하나 있다.

foo() 함수의 값이 2가 아니라 bar 값인 1이라는 점이다.

왜 예시 코드에 이런 차이를 두었을까?

inner 라는 예약어를 사용해서 중첩 클래스를 나타내면 중첩 클래스의 바깥 클래스가 가지는 멤버에 접근할 수 있다는 것을 보여주기 위해서이다.

즉, bar 변수는 Nested 클래스의 멤버 변수가 아니라 Outer 클래스의 멤버 변수이지만 Nested 클래스의 멤버 함수인 foo() 에서 접근할 수 있다!

이 점을 보여주기 위해 예시 코드가 조금 달라졌다는 것을 깨달을 수 있을 것이다.

<br>

inner class 중에 익명으로 생성되는 클래스들도 있는데 예를 들어,

~~~kotlin
window.addMouseListener(object : MouseAdapter(){
    override fun mouseClicked(e : MouseEvent){
        ...
    }

    override fun mouseEntered(e : MounseEvent){
        ...
    }
})
~~~

위와 같은 코드에서 마우스 리스너의 인자로 들어가 있는 MouseAdapter 가 익명 inner class라고 할 수 있다.

익명 inner class를 생성할 때는 __object :__ 를 앞에 붙여서 생성해야 한다.

<br>
