---
layout: post
title:  "[RxJava] 👽 Reactive Programming"
date:   2020-01-22 18:34:10 +0700
categories: [RxJava]
---

> 같이 공부하는 언니랑 겨울 방학동안에 RxJava를 공부해보기로 했다!! 
>
> 익숙하지가 않아서 어려운 느낌이지만 시작이 반이니까!
>
> [RxJava 프로그래밍](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=116852658) 책을 바탕으로 공부한 내용이다.

<br>

## 👽 Reactive Programming이란?
---

Reactive Programming 이라는 것을 이해하기 위해서 기존의 프로그래밍 방식에 대해서 생각해보자.

기존의 프로그래밍 방식은 컴퓨터 하드웨어에게 무엇인가를 해주라는 명령을 하는 방식이고, 명령은 절차에 따라 순서대로 실행된다.

이러한 기존의 프로그래밍 방식과 다르게 작동하는 것이 Reactive Programming 이다. 

> Reactive Programming은 프로그래밍의 패러다임 중 하나이다!

Reactive Programming은 절차에 초점을 두는 것이 아니라 데이터 흐름에 먼저 초점을 두는 방식이다.

따라서, 데이터 흐름을 먼저 정의하고 이 데이터들이 변경되었을 때 이와 연관되어 있는 명령어나 함수가 실행되는 방식이다. 

그리고 더 나아가서 __데이터가 변경되었다는 것은 시스템에 어떤 이벤트가 발생했다__ 는 것과 같다. 여기서 다시 한번 정리하자면 Reactive Programming은 __어떤 이벤트가 발생했을 때 해당 작업을 처리하는 방식__ 이다.

> 안드로이드 프로그래밍에서 서버 통신을 할 때 사용되는 callback도 개념상으로는 Reactive Programming이라고 할 수 있다!

조금 더 이해하기 쉽게 정리해보면 Reactive Programming이라는 것은 결국 Reactive Program을 만들기위한 방법이다. 여기서 Reactive Program은 어떠한 프로그램이냐면 __"주변 환경과 끊임없이 상호작용을 하면서 프로그램이 주도하는 것이 아니라 환경이 변하면 이벤트를 받아 동작하는 프로그램"__ 이다. 

여기서 무엇인가와 상호작용하는 프로그램은 Reactive Program 외에도 서버와 통신을 하는 프로그램도 있을 것이다. 통신을 하는 프로그램은 자신의 속도에 맞춰 일하지만 Reactive Programming은 외부 요구 반응에 맞춰 일한다.

<br>

## 👽 그렇다면 요즘 자주 듣게 되는 RxJava 라는 것은 무엇일까?
---

위에서는 Reactive Programm과 Reactive Programming이 무엇인지 알아봤다!

그렇다면 이제 Reactive Program을 구현하려면 어떻게 해야할까? 

Programming을 하려면 누군가 프로그래밍을 할 수 있는 기반 시설을 제공해주어야 한다. 즉, 내가 어떠한 코드를 짜면 이것이 Reactive Programming의 방식으로 돌아간다는 것을 누군가가 알아차려서 실행시켜주어야 한다.

그 누군가가 바로 __JVM위의 자바 언어로 구현해놓은 라이브러리인 RxJava__ 이다!

<br>

이렇게 RxJava가 있듯이 여러가지 Reactive Programming에 사용되는 

> 여기까지 Reactive Programming에 대해서 간략하게 알아보았고, RxJava에 관해서는 다른 포스팅에서 자세히 알아보자!

<br>
