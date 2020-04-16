---
layout: post
title:  "[안드로이드] 🗂 Resource 에 대해서"
date:   2020-04-16 18:34:10 +0700
categories: [안드로이드]
---

## 🗂 안드로이드 프로젝트 중 res 폴더에 대한 고찰!
---

![09](https://user-images.githubusercontent.com/31889335/79442938-2532aa00-8014-11ea-9dd1-9f3d5b688010.PNG)

안드로이드 프로젝트 구조를 보면 res 라고 이름 붙여져 있는 폴더가 있다.

이 폴더의 역할과 기능에 대해서 알아보고 싶어서 [Android App resource](https://developer.android.com/guide/topics/resources/providing-resources#AliasResources) 에 대해서 알아보았다.

링크가 걸려있는 문서를 보면 리소스라는 것에 대한 설명이 있다.

한 번 읽어보자!

<br>

안드로이드 개발에 있어서 __리소스(resource)__ 는 코드(자바 or 코틀린)에서 사용하는 __"추가적인 파일"__ 이나 __"정적인 콘텐츠"__ 를 의미한다.

예를 들어 비트맵, 레이아웃, 사용자 인터페이스 문자열 등이 리소스에 해당한다.

__그렇다면 왜 리소스 폴더가 따로 있는 걸까?__

우리가 안드로이드 어플을 개발할 때 이미지나 문자열과 같은 리소스들은 항상 코드(자바 or 코틀린)에서 외부화(분리)해야 하기 때문이다.

그래야 리소스들을 코드와 독립적으로 유지 및 관리를 할 수 있게 된다.

이렇게 외부화된 리소스들은 __클래스 R__ 이라는 클래스에서 발생하는 리소스 __id__ 로 엑세스할 수 있다!

<br>

## 🗂 리소스에는 여러가지 유형이 있다?
---

리소스(resource) 라는 것이 안드로이드 플랫폼에서 무슨 역할을 하는지 알게 되었을 것이다.

그렇다면 이제 리소스에 해당하는 것들은 어떤 것들이 있는지 알아보자!

다음은 간단한 프로젝트의 파일 계층이다.

![10](https://user-images.githubusercontent.com/31889335/79452454-261f0800-8023-11ea-9cd7-0bf2210d4c48.PNG)

여러개의 폴더 중, res 라는 폴더 하위에는 __drawable__, __layout__, __mipmap__, __values__ 등의 또 다른 폴더가 있는 것을 알 수 있다.

이 4가지 폴더는 각각 resource 유형이라고 볼 수 있다. 

drawable 폴더에 저장되는 resource 들은 png, jpg, gif, xml 등의 파일이고

layout 폴더에 저장되는 resource 들은 사용자 인터페이스 레이아웃(UI 레이아웃) 을 정의하는 xml 파일이다.

이와 같이 각 폴더에 저장될 수 있는 resource의 유형이 나뉘어져 있다!

또한 이 4개의 폴더는 프로젝트를 생성했을 때 기본적으로 제공되는 폴더이고, 필요할 시에는 추가로 폴더를 생성할 수 있다.

다만 추가로 폴더를 생성할 때는 폴더 이름이 중요하다. 왜냐하면 res 디렉토리 내부에서 지원되는 하위 디렉토리가 정해져 있기 때문이다.

res 폴더 안에서 생성할 수 있는 폴더의 이름에는

- animator

- anim

- color

- drawable

- mipmap

- layout

- menu

- raw

- values

- xml

- font

가 있다.

> 각 폴더별 기능은 [Android App resource](https://developer.android.com/guide/topics/resources/providing-resources#AliasResources) 의 표 1에서 확인할 수 있다.






