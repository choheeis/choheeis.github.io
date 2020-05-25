---
layout: post
title:  "[안드로이드] 🤳🏻 왜 NestedScrollView를 써야 하나??"
date:   2020-05-25 18:34:10 +0700
categories: [안드로이드]
---

<br>

## 🤳🏻 NestedScrollView가 뭘까?
---

안드로이드 개발을 1년 반 정도 하다보니 예전에 사용하던 xml 뷰 태그랑 지금 사용하는 xml 뷰 태그가 바뀐 것이 있었다.

그 중 하나가 ScrollView 였다.

안드로이드 개발 초창기에는 화면을 스크롤하기 위해 ScrollView 라는 xml 태그를 사용했었는데 현재는 스크롤 하기 위해 NestedScrollView를 사용하고 있다.

하지만 이 차이가 무엇인지 알지 못하고 언제부터인가 NestedScrollView를 사용하기 시작했었다.

그래서 이번 포스팅을 계획하면서 ScrollView와 NestedScrollView의 차이를 알아보기로 하였다!

<br>

먼저 안드로이드 developers 사이트에서 ScrollView에 대한 reference를 읽어보면서 __[ScrollView](https://developer.android.com/reference/android/widget/ScrollView.html)__ 에 대해서 알아보자.

![01](https://user-images.githubusercontent.com/31889335/82780777-25d02300-9e93-11ea-8ff3-01ea9dc9d0f1.PNG)

ScrollView라는 클래스는 안드로이드 API level 1 수준에서 추가된 클래스이고, FrameLayout 이라는 클래스를 상속받고 있다.

ScrollView는 화면을 스크롤 할 수 있도록 하기 위해 사용할 수 있는데 주의할 점은 ScrollView 는 단 하나의 direct 자식만을 가질 수 있다는 점이다.

따라서 ScrollView 안에 여러 개의 View들을 넣고 싶다면 이 여러 개의 View들을 감싸는 단 하나의 View Group을 만들어야 한다.

즉, 아래와 같은 코드로 이해해보자.

![02](https://user-images.githubusercontent.com/31889335/82781246-3df47200-9e94-11ea-930f-52c21733cc80.PNG)

첫 번째 엑스 표시된 코드는 ScrollView안의 자식으로 두 개의 TextView가 있다.

이것은 ScrollView의 direct 자식이 두 개 존재하는 것이다.

하지만 ScrollView는 direct 자식이 단 하나만 존재해야 하므로 동그라미 표시된 두 번째 코드처럼 두 TextView를 하나의 ConstraintLayout 으로 묶어야 한다.

이렇게 하면 ScrollView 입장에서는 ConstraintLayout 하나가 direct 자식이 되기 때문이다.

<br>

또, ScrollView 자체는 수직 스크롤만 허용하므로 수평 스크롤을 사용하고 싶다면 API level 3 에서 추가된 [HorizontalScrollView](https://developer.android.com/reference/android/widget/HorizontalScrollView) 를 사용해야 한다.

ScrollView를 사용할 때 꼭 알아두어야 하는 것이 하나 더 있다!

RecyclerView나 ListView를 ScrollView 안에 넣어서는 절대 안된다.

ScrollView 안에 RecyclerView나 ListView를 넣는 경우에는 UI 적으로 좋지 않은 performance가 나타나고 좋지 않은 User Experience가 나타나게 된다.

performance 차이가 어떻게 나는지 직접 ScrollView와 NestedScrollView 안에 리사이클러뷰를 넣어보았다!

먼저 ScrollView 안에 12개의 아이템을 가지고 있는 리사이클러뷰를 넣은 모습은 다음과 같다.

![ScrollView](https://user-images.githubusercontent.com/31889335/82785530-bc094680-9e9d-11ea-9266-ae2a2f35b4b4.gif)

ScrollView 안에 리사이클러뷰를 넣었을 때는 한 번의 스크롤 동작으로 맨 마지막인 12번째 아이템까지 스크롤되지 않는다.

11번째 아이템까지만 한 번의 스크롤 동작으로 내려가고 12번째 아이템을 보기 위해 추가로 한 번 더 스크롤 동작이 필요하다!

<br>

다음은 NestedScrollView 안에 똑같은 리사이클러뷰를 넣은 모습이다.

![NestedScrollView](https://user-images.githubusercontent.com/31889335/82785533-bd3a7380-9e9d-11ea-8939-39297afe9402.gif)

NestedScrollView를 사용했을 때는 단 한 번의 스크롤 동작으로 12개 모든 아이템이 스크롤되는 모습이다.

따라서 ScrollView 안에 리사이클러뷰를 넣었을 경우에는 번거로운 두 번의 스크롤 동작을 해야하므로 좋지 않은 user experience가 발생하게 된다.

<br>

따라서 리사이클러뷰를 포함하는 스크롤 화면을 구현해야 할 경우, ScrollView 대신 NestedScrollView 를 사용하자!

[NestedScrollView](https://developer.android.com/reference/androidx/core/widget/NestedScrollView) 클래스에 정의된 멤버 함수들은 링크로 걸어둔 문서를 통해 확인하면 된다.

<br>








