---
layout: post
title:  "[RxJava] 🏊‍♀️ RxAndroid!"
date:   2020-05-18 18:34:10 +0700
categories: [RxJava]
---

> [RxJava 프로그래밍](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=116852658) 이라는 책을 참고하여 공부한 내용입니다😃

<br>

## 🏊‍♀️ Java를 이용한 안드로이드 개발의 한계점
---

Java를 이용한 안드로이드 개발에는 한계점이 나타나게 되었다.

아직까지 Java라는 언어는 함수형 프로그래밍을 지원하지 못한다는 점이 한계에 대한 원인이다.

이러한 이유로 안드로이드에서는 함수형 프로그래밍을 하기 위해 RxJava를 도입하게 되었고, RxJava를 활용할 수 있는 __RxAndroid__ 라는 라이브러리가 등장하게 되었다.

RxAndroid 라는 라이브러리에 대해 알아보기 위해 [RxAndroid 공식 깃헙](https://github.com/ReactiveX/RxAndroid) 에 들어가보자.

<br>

RxAndroid는 RxJava에 최소한의 클래스를 추가하여 안드로이드 앱에서 Reactive 구성 요소를 쉽고 간편하게 사용할 수 있도록 도와주는 라이브러리이다.

조금 더 자세히 말하자면 RxJava의 구조에 안드로이드의 각 컴포넌트를 사용할 수 있게 변경해 놓은 것을 말한다.

RxAndroid는 기본적으로 RxJava의 reactive 라이브러리를 이용한다.

이 말의 의미는 RxJava 는 여러개의 reactive 라이브러리를 포함하고 있다는 뜻이다.

예를 들어 RxJava 1.x 이하 버전에서는 각종 이벤트 처리 부분이 RxJava 라이브러리에 포함되어 있었지만 경량화 작업과 유지보수 문제로 다음 버전부터는 다른 라이브러리화 되었다.

즉, RxJava 또한 여러 라이브러리를 사용하기 때문에 RxJava를 사용하는 RxAndroid 도 여러 라이브러리를 사용한다는 것이다.

[여기](https://github.com/ReactiveX/RxAndroid/wiki) 를 보면 안드로이드에서 사용할 수 있는 다양한 reactive API 및 라이브러리의 목록을 볼 수 있다.

<br>

## 🏊‍♀️ RxKoltin 을 사용하여 개발 스타뜨~
---

지금까지 Reactive Programming 에 대한 이해를 RxJava를 사용하여 해왔다.

하지만 나는 Kotlin에 익숙하기 때문에 RxJava를 통해 개념 공부를 했다면 실제 개발은 RxKotlin으로 할 것이다!

먼저 __안드로이드 스튜디오 환경 설정__ 을 해보자.

안드로이드 개발을 하면서 Reactive Programming을 하려면 RxAndroid 등 Reactive Programming을 할 수 있도록 도와주는 라이브러리들을 프로젝트에 추가해야 한다.

안드로이드 스튜디오가 빌드 도구로 사용하는 gradle에 사용할 라이브러리들의 종속성을 추가하자.

[RxAndroid 공식 github](https://github.com/ReactiveX/RxAndroid) 과 [RxKotlin 공식 github](https://github.com/ReactiveX/RxKotlin) 의 리드미를 보고 종속성을 추가하면 된다!

<br>

RxAndroid와 RxKotlin에 대한 종속성을 프로젝트에 추가했다면

__Hello World!를 입력받아 텍스트뷰에 보여주는 간단한 앱을 만들어보자!__







