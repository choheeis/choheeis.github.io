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

RxJava는 사실 2013년에 넷플릭스가 처음 만든 라이브러리이다. 당시 넷플릭스는 REST 기반의 API 호출 시 콜백 지옥에 빠지는 점을 해결하기 위해 RxJava를 만들게 되었고, 지금은 __RxJava3__ 이 작성될 정도로 꾸준히 업데이트 되고 있는 라이브러리이다. 

<br>

## 🔥 RxJava로 프로젝트 시작하기!
---

> 사용한 IDE - Intellij

<br>

1. __프로젝트 생성하기__

    > Intellij는 이번 RxJava 스터디를 하면서 처음 사용해본 IDE였다. Android Studio에 익숙해서 그런지 Intellij도 많이 낯설지는 않은 편이였다.

    <br>

    프로젝트를 생성하기 위해 먼저 

    ![01](https://user-images.githubusercontent.com/31889335/73610164-e21d8900-4617-11ea-9f21-e548199915ea.PNG)

    이렇게 Gradle 프로젝트를 만든다.

    그러면 다음과 같은 구조를 가지고 있는 프로젝트가 생성될 것이다.

    ![04](https://user-images.githubusercontent.com/31889335/73610237-7a1b7280-4618-11ea-89b0-4f38a019f622.PNG)

    <br>

2. __dependency 설정하기__

    RxJava는 라이브러리이기 때문에 다음과 같이 프로젝트의 dependency로 직접 추가해줘야 한다.

    [RxJava 공식 깃허브 리드미](https://github.com/ReactiveX/RxJava) 를 보면 Rxjava 실습을 위한 __Gradle compile dependency__ 로 RxJava3 버전이 명시되어 있다. 현재 포스팅일 기준으로는
    
    ~~~java
    implementation "io.reactivex.rxjava3:rxjava:3.0.0-RC9"
    ~~~

    이 최신 버전이다.

    위 프로젝트 구조 그림에서 __build.gradle__ 파일에서 다음과 같이 추가해주면 된다.

    ![02](https://user-images.githubusercontent.com/31889335/73610163-e184f280-4617-11ea-8985-c8d4d1daf926.PNG)

    <br>

3. __실습할 파일 만들기__

    이제 위 프로젝트 구조 그림에 있는 java 폴더안에서 실습을 위한 클래스 파일을 하나 생성해주자.

    java 폴더를 더블 클릭하니 다음과 같은 Project Structur가 나와서 Dependencies 설정에서 앞에서 추가한 라이브러리를 체크해주었다!
    
    ![03](https://user-images.githubusercontent.com/31889335/73610162-e184f280-4617-11ea-8ddb-1959e39458e6.PNG)

    그 후, java 폴더위에서 마우스 오른쪽 클릭 후, 원하는 이름의 자바 클래스를 하나 생성하면 실습할 준비 끝이다!

    <br>








