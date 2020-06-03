---
layout: post
title:  "[안드로이드] 🔨 Android 프로젝트에서 사용되는 아키텍처(2탄)"
date:   2020-06-01 18:34:10 +0700
categories: [안드로이드]
---

<br>

## 🔨 안드로이드 app Architecture 이어서!
---

6️⃣ 데이터 지속하기

[안드로이드 아키텍처 1탄](https://choheeis.github.io/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C/2020/05/28/AndroidArchitecture.html) 에서는 데이터를 가져오는 곳이 서버였다.

이번에는 데이터를 가져오는 곳이 다음 그림에서 표시한 것과 같이 내부 저장소인 SQLite 인 경우, 안드로이드 아키텍처를 어떻게 구성해야 하는지 알아보자!

![04](https://user-images.githubusercontent.com/31889335/83410645-b2f61780-a451-11ea-9b9d-1c1b2fcf70d8.PNG)

