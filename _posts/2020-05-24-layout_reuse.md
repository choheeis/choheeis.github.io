---
layout: post
title:  "[안드로이드] 🌾 layout 재사용하기"
date:   2020-05-24 18:34:10 +0700
categories: [안드로이드]
---

<br>

## 🌾 layout을 재사용 할 수 있다??
---

![01](https://user-images.githubusercontent.com/31889335/82750071-c0275c80-9de8-11ea-9200-4f23f46e176f.PNG)

안드로이드 developers 사이트의 가이드 부분을 보면 UI 카테고리가 있다.

UI 카데고리 안에는 Improving layout perfomances 라는 제목으로 layout 성능을 향상시킬 수 있는 방법에 대해 설명되어 있다!

layout 성능을 향상시킬 수 있는 방법에는 몇 가지가 있지만 이 포스팅에서는 __\<include> 태그__ 를 이용해 향상시키는 방법에 대해 알아볼 것이다.

<br>

안드로이드 개발을 하다 보면 똑같은 UI를 반복적으로 사용해야 하는 경우가 있다.

예를 들어, 

![02](https://user-images.githubusercontent.com/31889335/82757258-38f2dc80-9e1a-11ea-862a-b50a3bffdaf3.PNG)

멜론 어플의 경우 맨 위쪽 상단의 title 뷰가 거의 비슷한 모습으로 디자인 되어 있다.

이런 경우 외에도 아예 똑같은 title 뷰가 여러 Activity 상단을 차지하는 어플도 많은데 이럴 때마다 title 뷰를 Activity xml에 계속 작성하는 것은 비효율적이다.

따라서 이렇게 부분적인 layout의 반복이 일어날 경우, __\<include>__ 태그를 사용하면 더 효율적으로 xml을 작성할 수 있다.

[Re-using layouts with \<include>](https://developer.android.com/training/improving-layouts/reusing-layouts) 라는 문서를 읽어보면서 어떤 방법으로 layout을 재사용할 수 있는지 알아보자!

<br>

layout을 효과적으로 재사용할 수 있는 방법은 크게 두 가지가 있다.

- \<include> 태그를 사용한 방법

- \<merge> 태그를 사용한 방법

위 두 가지 방법을 통해 어떤 layout 안에 반복적으로 사용될 layout을 넣을 수 있다.

특히나 layout을 재사용할 수 있다는 것은 복잡한 layout을 서로 다른 화면에서 반복적으로 사용해야 하는 경우에 막강해질 것이다.

따라서 위 멜론 어플의 title 뷰는 상대적으로 단순한 뷰의 반복이지만 조금 더 복잡한 뷰의 반복일 경우 layout의 재사용을 꼭 해야한다.

<br>

## 🌾 \<inclue> 태그를 통해 layout 재사용하는 방법
---

그럼 지금부터 본격적으로 [Re-using layouts with \<include>](https://developer.android.com/training/improving-layouts/reusing-layouts) 문서를 통해 layout을 재사용하는 방법에 대해 알아보자.

제일 먼저 할 일은 재사용해야 할 layout을 새로운 xml 파일에 정의하는 것이다. 즉, xml 파일로 layout 디자인을 작성하는 것이다.

이렇게 작성된 layout을 임의로 titlebar.xml 이라고 하자.

그 다음, 이 titlebar.xml layout을 넣어야 할 다른 layout xml 파일 안에 아래 코드와 같이 \<include> 태그를 작성하면 된다.

![03](https://user-images.githubusercontent.com/31889335/82757606-7d7f7780-9e1c-11ea-9935-59c9bb5b2bbf.PNG)

\<include> 태그의 속성에는 layout 이라는 속성이 있는데 이 속성값으로 어떤 layout을 include 할 것인지 명시해주어야 한다.

layout 속성 외에도 아래와 같이 여러 속성들이 있다.

![04](https://user-images.githubusercontent.com/31889335/82757605-7ce6e100-9e1c-11ea-8fd8-84fc93f0e2a0.PNG)

위 코드는 재사용될 layout이 얼만큼의 width와 height 크기를 가질 것인지 정해주는 모습이다.

이것 외에도 android:layout_ 으로 시작하는 모든 속성을 사용할 수 있다.

만약 titlebar.xml 을 아래와 같이 ConstraintLayout를 ViewGroup으로 하여 작성했다면

include 태그 자리에는 다음과 같은 코드가 들어간다고 생각하면 된다.

![05](https://user-images.githubusercontent.com/31889335/82758260-60e53e80-9e20-11ea-9275-a3c65d0b216c.PNG)

따라서 include 태그를 사용해서 layout을 재사용할 경우, titlebar.xml 의 textView만 들어가는 것이 아니라 textView를 감싸고 있는 ConstraintLayout까지 모두 들어가서 layout의 깊이가 깊어진다는 단점이 있다.

<br>

## 🌾 \<merge> 태그를 통해 layout 재사용하는 방법
---

\<merge> 태그는 재사용될 layout을 다른 화면에 포함시킬 때 필요없는 layout을 제거할 수 있도록 도와준다.

위 include 태그를 사용하여 layout 재사용을 할 경우, 단점은 layout의 깊이가 깊어진다는 것이였다.

이러한 점을 해결하기 위해서는 merge 태그를 사용하면 되는데 titlebar.xml 을 

![06](https://user-images.githubusercontent.com/31889335/82758355-07c9da80-9e21-11ea-9025-213eea64de62.PNG)

이렇게 ConstraintLayout 대신, merge 태그로 감싸주면 된다.

이렇게 한 후, include 태그의 layout 속성값으로 titlebar.xml 을 정의해주면 딱 TextView만 들어가게 된다.

<br>

이렇게 layout을 재사용함으로써 성능을 좋게 만들 수 있는 방법 2가지를 알아보았다.

include 태그 안의 속성이나 merge 태그에 대해 조금 더 자세한 내용을 보고 싶다면 [여기](https://developer.android.com/guide/topics/resources/layout-resource#include-element) 를 읽어보자!

<br>







