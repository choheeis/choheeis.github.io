---
layout: post
title:  "[RxJava] 👽 Reactive Programming"
date:   2020-01-22 18:34:10 +0700
categories: [RxJava]
---

> SOPT라는 대외활동을 하면서 만나게 된 짱짱걸 언니랑 겨울 방학동안에 RxJava를 공부해보기로 했다!! 
>
> 익숙하지가 않아서 어려운 느낌이지만 시작이 반이니까!
>
> ["RxJava 프로그래밍"](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=116852658)이라는 책을 바탕으로 공부한 내용이다.

<br>

## 👽 Reactive Programming이란?
---

Reactive Programming 이라는 것을 이해하기 위해서 기존의 프로그래밍 방식데 대해 생각해보자.

기존의 프로그래밍 방식은 컴퓨터 하드웨어에게 무엇인가를 처리해달라는 명령을 하는 방식이였고, 그 명령들은 어떠한 절차에 따라 순서대로 실행된다.

하지만 Reactive Programming은 이러한 기존의 프로그래밍 방식과 조금 다른 방식으로 작동한다.

> Reactive Programming은 프로그래밍의 패러다임 중 하나이다!

Reactive Programming은 명령들이 실행되는 일정한 절차에 초점을 두는 것이 아니라 데이터 흐름에 먼저 초점을 둔다.

간단히 설명하자면 데이터 흐름을 먼저 정의하고 이 데이터들이 변경되었을 때 이와 연관되어 있는 명령어나 함수가 실행되는 방식이다. 

__데이터가 변경되었다는 것은 시스템에 어떤 이벤트가 발생했다__ 는 것과 같다고 볼 수도 있다. 즉, __어떤 이벤트가 발생했을 때 필요한 작업을 처리하는 방식__ 이 Reactive Program의 기본 방식이다.

조금 더 이해하기 쉽게 정리해보면 Reactive Programming이라는 것은 결국 Reactive 프로그램을 만들기 위한 방법이다. 

여기서 Reactive 프로그램이 어떠한 프로그램이냐면 __"주변 환경과 끊임없이 상호작용을 하면서 프로그램이 주도하는 것이 아닌 환경이 주도하고 환경에 의한 이벤트를 받아 동작하는 프로그램"__ 이다. 

<br>

## 👽 그렇다면 RxJava 라는 것은 무엇일까?
---

위에서는 Reactive 프로그램과 Reactive Programming이 무엇인지 알아보았다!

그럼 이제 Reactive 프로그램을 개발하려면 어떻게 해야할까? 

Programming을 한다는 것은 누군가가 프로그래밍을 할 수 있는 기반 시설을 제공해주었다는 의미이다. 

즉, 내가 어떠한 코드를 짜면 이것이 Reactive Programming의 방식으로 돌아간다는 것을 누군가가 알아차려서 실행시켜주는 것이다.

여기서 그 누군가가 바로 __JVM위의 자바 언어로 구현해놓은 라이브러리인 RxJava__ 이다!

<br>

> 여기까지 Reactive Programming에 대해서 간략하게 알아보았고, RxJava에 관해서는 다른 포스팅에서 자세히 알아보자!

<br>
