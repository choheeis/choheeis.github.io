---
layout: post
title:  "[RxJava] 🔥 RxJava 시작하기!"
date:   2020-01-22 18:34:10 +0700
categories: [RxJava]
---

## 🔥 RxJava가 뭘까?

> RxJava에 대해 알고 싶다면 [Reactive Programming]() 포스팅을 한 번 읽고 오자! 여기에 RxJava가 무엇인지도 간단하게 언급되어 있기 때문!

<br>

RxJava는 Reactive Programming을 할 수 있게 해주는 __"JVM 위의 자바 언어로 구현해놓은 라이브러리"__ 이다!

한 마디로 RxJava는 자바 라이브러리인데 자바 말고 다른 언어에서도 RxJava와 같이 Reactive Programming을 지원해주는 라이브러리가 있을까?

있다!!!

> Rx는 ReactiveX의 줄임말이고 ReactiveX는 __Reactive Extensions__ 의 줄임말이다. 

[ReactiveX 의 공식 깃헙](https://github.com/ReactiveX)에 들어가보면 RxJava외에도 RxKotlin, RxSwift, RxPY, RxAndroid, RxCpp, RxRuby 등등 이미 많은 언어들이 Reactive Programming을 지원하는 라이브러리를 제공하고 있음을 알 수 있다!

> RxJava공식 깃헙 링크 --> [여기](https://github.com/ReactiveX/RxJava) 

RxJava는 사실 2013년에 넷플릭스가 처음 만든 라이브러리이다. 당시 넷플릭스는 REST 기반의 API 호출 시 콜백 지옥에 빠지는 점을 해결하기 위해 RxJava를 만들게 되었고, 2016년에는 __RxJava2.0__ 가 새로 작성될 정도로 꾸준히 업데이트 되고 있는 라이브러리이다. 

<br>

## 🔥 RxJava로 프로젝트 시작하기!
---