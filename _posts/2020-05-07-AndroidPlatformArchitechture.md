---
layout: post
title:  "[안드로이드] 📱 안드로이드 플랫폼의 아키텍처"
date:   2020-05-07 18:34:10 +0700
categories: [안드로이드]
---

> [안드로이드 developers 사이트 - 플랫폼 아키텍처](https://developer.android.com/guide/platform?hl=ko) 를 보고 공부한 내용입니다.

<br>

## 📱 '안드로이드' 가 뭔가!
---

안드로이드 developers 사이트에 들어가보면 위쪽 배너에 __플랫폼__ 이라는 배너가 있다. 

플랫폼이라는 배너 안에는 또 다른 배너들이 있는데 그 중 __기술__ 이라는 배너 안에 들어가보면 __플랫폼 아키텍처__ 라는 카테고리가 있을 것이다!

![01](https://user-images.githubusercontent.com/31889335/81201416-3746ad80-9000-11ea-98e1-5c15f882acdb.PNG)

이 부분을 읽어보며 안드로이드가 무엇이고, 안드로이드 플랫폼의 아키텍처가 어떻게 구성되어 있는지 알아보자!

<br>

Android는 다양한 기기에 사용할 수 있도록 제작된 __Linux 운영체제 기반의 오픈소스 소프트웨어__ 이다!

이러한 안드로이드 플랫폼을 구성하고 있는 요소들은 다음과 같다.

![02](https://user-images.githubusercontent.com/31889335/81201819-af14d800-9000-11ea-947e-bfcf03432024.PNG)

가장 밑에 깔려 Android 플랫폼의 기반이 되는 것은 __Linux 커널__ 이다. 

즉, Android 런타임인 ART는 쓰레드나 메모리 관리 등과 같은 기본 기능을 하기 위해 Linux 커널을 사용한다.

Linux 커널은 각종 드라이버들을 제공하는데 Audio 드라이버, Display 드라이버, Camera 드라이버, Bluetooth 드라이버, USB 드라이버, WIFI 드라이버 등 다양한 드라이버들을 제공한다.

이와 같은 Linux에서 제공하는 드라이버들은 각각에 해당하는 하드웨어를 조작하고 통제하는 역할을 한다.

<br>

Linux 커널 위에 위치하는 __[Hardware Abstraction Layer(HAL = 하드웨어 추상 계층)](https://source.android.com/devices/architecture/hal?hl=ko)__ 에 대해서 알아보자.

![03](https://user-images.githubusercontent.com/31889335/81205414-a1ae1c80-9005-11ea-850a-66564c8c5172.PNG)

하드웨어 추상화 계층에는 Linux 커널에서 제공되는 드라이버들이 통제하는 하드웨어들이 포함된다.

따라서 안드로이드 phone의 Audio, Camera, Sensor 등이 이 하드웨어 추상화 계층에 존재한다.

또한 Android 플랫폼 아키텍처에서 하드웨어 추상화 계층보다 상위에 있는 Java API 프레임워크에 기기 하드웨어 기능을 노출해주는 표준 인터페이스가 된다. 

즉, 프레임워크 API 가 기기의 하드웨어에 접근하기 위해 API 호출을 수행하면 Android 시스템이 해당 하드웨어 구성 요소에 접근할 수 있도록 interface를 제공하는 것이다.

이를 위해 하드웨어 추상화 계층은 여러 라이브러리 모듈로 구성되어 있고, 결국 프레임워크 API 가 기기의 하드웨어에 접근을 위해 API 호출을 수행하면 이 HAL의 라이브러리 모듈이 로드되는 것이다.

<br>

이번에는 HAL 상위에 존재하는 __[Android Runtime(ART)](https://source.android.com/devices/tech/dalvik/index.html?hl=ko)__ 에 대해서 알아보자.

> 일단 Android Runtime은 컴파일러라고 생각하자!

![04](https://user-images.githubusercontent.com/31889335/81205475-b7234680-9005-11ea-89ee-88aefce944c3.PNG)

Android 버전 5.0( = API 레벨 21) 이상을 실행하는 기기의 경우에는 각 앱이 자체 프로세스 내에서 Android Runtime(ART)를 인스턴스로 실행한다.

위의 그림과 같이 Android Runtime에는 Java 프레임워크가 사용하는 몇 가지 [Java 8 언어 기능](https://developer.android.com/guide/platform/j8-jack?hl=ko) 까지 포함하여 대부분의 Java 프로그래밍 언어 기능을 제공하는 핵심 런타임 라이브러리( = Core Libraryies )도 포함되어 있다.

<br>

그렇다면 이제 __Native C/C++ Libraries__ 에 대해서 알아보자.

![05](https://user-images.githubusercontent.com/31889335/81206350-f1411800-9006-11ea-865a-9c6e3b38d03c.PNG)

사실 Android Runtime과 하드웨어 추상화 계층(HAL) 등 Android 시스템을 구성하는 핵심 요소들이 빌드 될 때, C와 C++로 작성된 native library 를 필요로 한다.

<br>

그럼 __Java API 프레임워크__ 는 뭘까?

![06](https://user-images.githubusercontent.com/31889335/81206632-5d238080-9007-11ea-97d6-fc3e3f6aa851.PNG)

일단 Android는 Java 언어로 작성된 API 를 통해 엑세스할 수 있도록 되어 있다.

Java 언어로 작성된 API를 Java API Framework라고 한다.

Java API Framework 에는 Activity Manager, Resource Manager, Notification Manager 등이 포함되어 있고, View System 또한 포함되어 있다.

<br>

마지막으로 가장 최상위 구성 요소인 __System App(시스템 앱)__ 에 대해 알아보자!

![07](https://user-images.githubusercontent.com/31889335/81207114-02d6ef80-9008-11ea-8564-6ae9e9779a21.PNG)

시스템 앱이라는 것은 안드로이드 운영체제를 사용하는 phone을 구입하면 기본적으로 설치되어 있는 앱들을 말한다고 생각하면 된다.

즉, SMS 메시징기능, 캘린더, 주소록 등의 기본 앱들을 말하는 것이다. 

<br>

여기까지 간략한 안드로이드 플랫폼의 아키텍처 공부 끝~~ 📱📱

<br>