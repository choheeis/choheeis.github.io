---
layout: post
title:  "[안드로이드] ✋ Bottom Navigation 만드는 나만의 라이브러리"
date:   2020-03-01 18:34:10 +0700
categories: [안드로이드]
---

> [BottomNavigation 도큐먼트](https://developer.android.com/reference/android/support/design/widget/BottomNavigationView.html#inherited-constants) 와 [머터리얼 디자인 Bottom navigation](https://material.io/components/bottom-navigation/#) 을 참고한 포스팅!

# ✋ Bottom Navigation 만드는 나만의 라이브러뤼~

1. __Bottom Navigation 도큐먼트 찾아보기__

    일단 무조건 Bottom Navigation에 대한 공식 도큐먼트가 있는지 찾아봐야겠지?

    [BottomNavigation 도큐먼트](https://developer.android.com/reference/android/support/design/widget/BottomNavigationView.html#inherited-constants) 있다!

    그럼 이제 이 도큐먼트를 읽어보자!

    ![01](https://user-images.githubusercontent.com/31889335/75611533-a3b7b300-5b5e-11ea-8421-0f7e23c2ecaa.PNG)

    위 그림처럼 BottomNavigationView 라는 제목 위에 작은 글씨로 쓰여있는 부분이 있었다!

    읽어보니 이 BottomNavigation은 안드로이드 컴파일 버전 26.1.0 부터 추가되었고, com.android.support:design:27.1.0 버전에 속해있다고 하는 것 같다.

    즉, 이 BottomNavigation은 처음부터 있던 뷰가 아니라 중간에 추가된 뷰이고 안드로이드 지원 라이브러리 중 디자인 라이브러리(android.support:design)에 속해있는 뷰라는 것이다.

    > --> [android.support 라이브러리에 대한 도큐먼트](https://developer.android.com/topic/libraries/support-library?hl=ko)

    __그렇기 때문에 프로젝트에 android.support:design:27.1.0 버전을 implement 해야한다.__

    <br>

2. __android.support:design:27.1.0 버전 implement 하기__

    build.gradle(Module: app) 파일에 

    ![02](https://user-images.githubusercontent.com/31889335/75611848-8b956300-5b61-11ea-92e8-f35566a4bd65.PNG)

    이렇게 작성한 후 sync를 했더니 그대로 빨간줄이 떠있었다.

    오류 내용을 읽어보니 안드로이드 버전 28까지만이 기존의 안드로이드 라이브러리를 지원하는 마지막 버전이고, 29 이후부터는 AndroidX 라이브러리로 이동하라고 쓰여있었다. 

    지금 내가 만든 프로젝트가 컴파일되는 SDK 버전이 29여서 그런 것 같았다. 

    AndroidX 라이브러리로 이동하는 방법은 안드로이드 스튜디오에서 Refactor --> Migrate to AndroidX 를 누르라고 되어있었다.

    ![03](https://user-images.githubusercontent.com/31889335/75611925-2d1cb480-5b62-11ea-83b9-917ca9e11bab.PNG)

    그랬더니 이렇게 refactoring 되는 부분을 보여주면서 AndroidX 라이브러리로 바뀌는 2개의 라이브러리를 보여주었다.

    이 2개의 라이브러리를 모두 화살표로 가리킨 Do Refactor을 클릭하여 Migrate 시켜주었더니 빨간줄이 사라졌다!

    <br>

3. __머티리얼 디자인 가이드보고 따라하기__

    찾아보니 android.support 라이브러리는 머티리얼 디자인 사용자 인터페이스 추천사항 구현을 위한 여러 클래스를 제공한다고 한다. 

    그러니 머터리얼 디자인 도큐먼트에 Bottom Navigation이 있는지도 찾아보자!

    [머티리얼 디자인 Bottom Navigation 가이드](https://material.io/develop/android/components/bottom-navigation-view/) 가 있다!

    이제 이 가이드를 읽어보자.

    ![04](https://user-images.githubusercontent.com/31889335/75611957-808f0280-5b62-11ea-8351-459cebff3e94.PNG)

    가이드를 읽어보니 BottomNavigationView는 위와 같은 __bottom navigation bar를 만들어주는 뷰__ 이고, 이 bottom navigation bar는 각 탭에 연결된 뷰들을 쉽게 바꿀 수 있도록 도와주는 역할을 한다고 설명되어 있다.

    이 때, 각 탭에 연결된 뷰들은 어플을 처음 실행시켰을 때 가장 맨 위에 있는 top-level 뷰인 경우이다.

    이 bottom navigation bar는 어플이 3개 ~ 5개의 top-level뷰를 가지고 있어야 할 때 사용되는 패턴이다.

    그럼 이제 머티리얼 디자인 가이드 문서에 나와있는 Bottom Navigation Bar 만드는 법에 대해 알아보자.

    <br>

# ✋ Bottom Navigation Bar 만드는 법

1. __Bottom Navigation Bar에서 탭이 될 메뉴 resource를 최대 5개까지 만든다. (5개 이상은 불가하다.)__

    먼저 안드로이드 스튜디오의 res 폴더를 우클릭한 후, New --> Android Resource Directory 를 클릭한다. 

    ![05](https://user-images.githubusercontent.com/31889335/75612052-77526580-5b63-11ea-962b-aba33d827de5.PNG)

    그러면 이런 창이 나오는데 이 때 type을 꼭 menu로 바꿔주어야 한다.

    ![06](https://user-images.githubusercontent.com/31889335/75612065-92bd7080-5b63-11ea-9e46-e785a74cb942.PNG)

    그러면 이렇게 res폴더 내에 새로운 menu 폴더가 생성되게 된다.

    이 menu 폴더를 다시 우클릭 후, New --> Menu resource file를 클릭하면 

    ![07](https://user-images.githubusercontent.com/31889335/75612085-ca2c1d00-5b63-11ea-81b6-31d5a4258cfa.PNG)

    다음과 같은 창이 나온다. 파일의 이름을 결정하고 파일을 만들면 된다!

    이와 같은 과정은 [Bottom Navigation View 도큐먼트](https://developer.android.com/reference/android/support/design/widget/BottomNavigationView.html#inherited-constants) 의

    ![08](https://user-images.githubusercontent.com/31889335/75612118-052e5080-5b64-11ea-8082-d9d21251c0cf.PNG)

    저 빨간 동그라미 부분을 보고 만든 것이다!

    이제 menu 폴더 안에 새롭게 만든 xml 에 위와 같은 코드를 똑같이 작성해보자.

    ![09](https://user-images.githubusercontent.com/31889335/75612140-2ee77780-5b64-11ea-9c83-85335cb1f6c7.PNG)

    그러면 이렇게 빨간색으로 마구 표시가 될 것이다.

    이유는 예시를 그대로 복사했기 때문에 내 프로젝트에는 아직 예시에서 사용한 아이콘이나 문자열들이 없기 때문이다.

    여기서 icon은 Bottom Navigation Bar에서 

    ![10](https://user-images.githubusercontent.com/31889335/75612191-9ef5fd80-5b64-11ea-989e-af7194ae397e.PNG)

    이 부분에 들어갈 icon을 말하고, title은

    ![11](https://user-images.githubusercontent.com/31889335/75612188-9dc4d080-5b64-11ea-801c-27b96d5b2e48.PNG)

    이 부분에 들어갈 컨텐츠를 말한다.

    이것들을 내가 개발하고자 하는 UI에 맞게 바꿔주면 된다.

    <br>

2. __BottomNavigationView라는 레이아웃 추가하기__

    [머티리얼 디자인 가이드 BottomNavigation](https://material.io/components/bottom-navigation) 에서는 다음과 같은 형식으로 BottomNavigationView 레이아웃을 추가하라고 되어있다.

    ![12](https://user-images.githubusercontent.com/31889335/75612342-ddd88300-5b65-11ea-9c5a-8323881e0b83.PNG)

    나는 Bottom Navigation Bar를 어플을 시작한 후 첫 화면에 띄울 것이기 때문에 activity_main.xml에 다음과 같이 작성하였다.

    ![13](https://user-images.githubusercontent.com/31889335/75619831-38083100-5bc4-11ea-91d9-6482f4fdf711.PNG)

    > FrameLayout을 꼭 써주자! Fragment를 교체할 때 필요하기 때문.

    BottomNavigationView의 속성으로 이전에 만든 menu를 연결해주는 모습을 볼 수 있다!

    여기까지 하면 이렇게 하단에 BottomNavigationBar가 생기고 각 탭을 클릭할 수 있게 된다!

    ![bottomNavigation](https://user-images.githubusercontent.com/31889335/75622653-2df72a00-5be6-11ea-8dc3-5b0c5e695c75.gif)

    > Material Design의 버전이 업그레이드 될 때마다 같은 Bottom Navigation이여도 모션이 조금씩 달라질 수 있다!
    >
    > __implementation 'com.google.android.material:material:1.2.0-alpha05'__ 을 적용시켰을 때의 Bottom Navigation은
    >
    > ![BottomNavigation2](https://user-images.githubusercontent.com/31889335/78767342-6fd57600-79c5-11ea-80ea-cda70d17c71b.gif)
    >
    > 이렇게 bottom Navigation item을 눌렀을 때 모션이 기존의 모션과 조금 달라진 것을 확인할 수 있다!!

    <br>

3. __Bottom navigation의 icon과 text 색깔 바꾸기__

    만약 Bottom navigation의 각 탭을 눌렀을 경우와 누르지 않았을 경우 색을 다르게 하고 싶다면 selector를 이용하면 된다.

    이 때, selector는 res 폴더 안에 color라는 폴더를 새로 생성하고 이 안에 xml 로 만들어야 한다. 

    ~~~xml
    <!-- color 폴더 안에 bottom_navigation_colors.xml이라는 selector를 만든 모습 -->
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item
            android:state_checked="true"
            android:color="@color/colorAccent" />
        <item
            android:state_checked="false"
            android:color="#ffffff" />
    </selector>
    ~~~

    그 후, 

    ~~~xml
    <com.google.android.material.bottomnavigation.BottomNavigationView
    android:id="@+id/bottomNavigationBar"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintBottom_toBottomOf="parent"
    android:layout_width="0dp"
    android:layout_height="wrap_content"
    app:menu="@menu/bottom_nav_items"
    android:background="@color/colorPrimary"
    app:itemTextColor="@color/bottom_navigation_colors"
    app:itemIconTint="@color/bottom_navigation_colors" />
    ~~~

    처럼 itemTextColor라는 속성과 itmeIconTint라는 속성에 selector를 적용시켜 주면 된다!

    <br>

4. __Fragment과 bottom navigation의 메뉴 item을 연결하기__

    > --> [참고한 유튜브 영상](https://www.youtube.com/watch?v=5EZZroOg4eU) 

    <br>

    - 일단 Bottom Navigation에서 사용한 item 갯수 만큼의 Fragment를 생성한다.

        ![14](https://user-images.githubusercontent.com/31889335/78767931-2f2a2c80-79c6-11ea-85fd-d7e89e7dce29.PNG)

        이렇게 필요한 3개의 Fragment를 각각 만들어주었다.

        <br>

    - 그 다음 Bottom Navigation이 있는 Activity 파일에서 Fragment를 교체해주는 함수를 작성한다.

        ![16](https://user-images.githubusercontent.com/31889335/78769351-381bfd80-79c8-11ea-8cfc-96be71b80ba3.PNG)

        이렇게 private 함수로 replaceFragment 라는 함수를 만들었다.

        이 함수는 인자로 들어오는 Fragment로 기존의 Fragment를 교체해주는 함수이다.

        이 함수에서 화살표로 가리킨 부분은 교체될 Fragment가 위치하는 부분을 말한다.

        따라서 위 코드에서 Fragment가 위치하는 부분은 아래 코드에서 frameLayout이다.

        ![15](https://user-images.githubusercontent.com/31889335/78769489-66014200-79c8-11ea-9a71-bfce8f7055a3.PNG)

        <br>

    - 마지막으로 __setOnNavigationItemSelectedListener__ 를 이용해 Bottom Navigation의 item이 선택되었을 경우 실행되는 이벤트 리스너를 만들어주면 된다.

        ![17](https://user-images.githubusercontent.com/31889335/78768761-56cdc480-79c7-11ea-8d89-b9a72294ef7b.PNG)

        위에서도 계속 참고했던 Bottom Navigation Material 디자인 가이드를 보면 위와 같이 Usage라고 쓰여있는 부분에 네 번째 단계로 

        __setOnNavigationItemSelectedListener__ 을 사용해서 이벤트를 감지하라고 나와있다.

        따라서 다음과 같이 setOnNavigationItemSelectedListener를 작성해주면 완성이다~

        ![18](https://user-images.githubusercontent.com/31889335/78769828-c85a4280-79c8-11ea-9073-4fb4c20ec799.PNG)

        <br>

        ![BottomNavigation3](https://user-images.githubusercontent.com/31889335/78770253-49193e80-79c9-11ea-908f-5191d319b6e8.gif)

        이렇게 Bottom Navigation의 각 메뉴 아이템에 Fragment가 연결된 것이 완성되었다~~

        <br>















