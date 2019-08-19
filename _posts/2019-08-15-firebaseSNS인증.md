---
layout: post
title:  "[파이어베이스] ✋ SNS 로그인기능?!"
date:   2019-08-15 18:34:10 +0700
categories: [firebase]
---

> ✋ 요즘 안드로이드 어플 개발을 할 때, 로그인 기능에 소셜 로그인(SNS로그인) 기능을 넣는 경우가 많다!
>
> Firebase에서 소셜 로그인 기능을 지원해준다고 들었기 때문에 이것에 대해 공부해보고 적용시켜보자!

<br>

일단, 파이어베이스에 프로젝트를 등록한 후, 파이어베이스 콘솔 사이트에 들어가보자.

> 프로젝트를 등록하는 방법은 [여기](https://github.com/choheeis/Android_YoungChaYoungCha) 의 FirebaseInit 에 대한 설명을 보면 된다!

<br>

콘솔 사이트의 왼쪽 배너를 보면 다음과 같이 개발에 관한 배너들이 모여 있는 것을 볼 수 있다.

![01](https://user-images.githubusercontent.com/31889335/63079590-af081000-bf79-11e9-8b07-284d1cf539f2.PNG)

이 중, 맨 위에 위치하는 Authentication 이 바로 소셜 로그인 기능을 위해 필요한 배너이다!

Authentication(인증)에 들어가보면 

![02](https://user-images.githubusercontent.com/31889335/63079646-e4acf900-bf79-11e9-9de8-3c72348b40e2.PNG)

이와 같은 화면을 볼 수 있다. 

[자세히 알아보기](https://firebase.google.com/products/auth/?authuser=0)와 [문서 보기](https://firebase.google.com/docs/auth/?authuser=0) 를 들어가서 공부해보자!

<br>

## ✋ 자세히 알아보기
---

> ✋ 자세히 알아보기를 들어가보니 __간편한 무료 다중 플랫폼 로그인__ 이라고 설명된 __Firebase Authentication__ 에 대해서 알려주는 유튜브 동영상들이 있었다!

파이어베이스는 우리가 로그인을 할 때 필요한 인증과정을 Facebook, Google, github, Twitter 등등의 업체와 협력하여 제공해준다.

또, 로그인 화면 UI를 직접 디자인 할 수도 있지만 구글이 제공하는 맞춤형 오픈소스 UI를 바로 사용할 수도 있다고 한다. 

파이어베이스는 사용자 인증이 확인되면 사용자 정보가 콜백을 통해 기기로 반환되는데 이 정보에 포함되는 사용자 고유 ID를 통해 엑세스할 수 있는 데이터를 구분해낸다고 한다.

<br>

## ✋ 문서보기
---

이 문서에 따르면 Firebase에서 로그인 기능을 처리할 때는 __FirebaseUI를 완전 삽입형 인증 솔루션으로 사용하는 방법__ 도 있고, __Firebase 인증 SDK를 사용해 하나 이상의 로그인 방법을 앱에 통합하는 방법__ 도 있다고 한다!

<br>

- _FirebaseUI를 완전 삽입형 인증 솔루션으로 사용하는 방법_

	이 방법은 Firebase 인증 SDK를 바탕으로 구축된 라이브러리이며 전체 로그인 시스템을 추가해야 할 때 권장되는 방법이다!

	이 방법은 이메일로 로그인하거나 구글 로그인, 페이스북 로그인 등 인기 소셜 로그인을 구현해야 할 경우의 UI 흐름을 처리하는 삽입형 인증 솔루션을 제공하는 방법이다. 

	또, 이 방법은 UI를 변경하기 쉬운 오픈 소스이므로 커스터마이징도 가능하다!

	이 문서에서는 위와 같은 설명이 되어 있었고, 안드로이드에서 이 방법을 적용시키는 방법도 나와있었다!

	> --> [FirebaseUI 인증](https://firebase.google.com/docs/auth/android/firebaseui?authuser=0)

	<br>

- _SDK 를 사용하는 방법_
	
	이 부분은 다음에!




