---
layout: post
title:  "[안드로이드] 🔨 Android 프로젝트에서 사용되는 아키텍처"
date:   2020-05-28 18:34:10 +0700
categories: [안드로이드]
---

<br>

## 🔨 안드로이드 app Architecture
---

[Android Jetpack이 뭐지?](https://choheeis.github.io/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C/2020/05/25/jetpack.html) 포스팅에서 Jetpack이 지원하는 것 중 Architecture에 대한 것이 있었다!

jetpack 라이브러리들 중 Architecture 카테고리로 분류되어 있는 것들에는

![01](https://user-images.githubusercontent.com/31889335/83118033-c8dda280-a108-11ea-9e2e-201751a17ef1.PNG)

이런 것들이 있다!

Jetpack에서 지원하는 이런 라이브러리들을 사용해서 안드로이드에서 추천하는 Architecture를 구성할 수 있다.

안드로이드에서 추천하는 아키텍처는 어떤 모습인지 [앱 아키텍처 가이드](https://developer.android.com/jetpack/docs/guide) 문서를 읽어보고 알아보자!

이 문서에서 제시하는 안드로이드 아키텍처는 고품질의 강력한 앱을 빌드하기 위한 권장 아키텍처이다. 

<br>

안드로이드 권장 아키텍처를 본격적으로 살펴보기 전에 모바일 앱 사용자 환경에 대해 살짝 알아보자.

<br>

## 🔨 모바일 앱 사용자 환경 이란?
---

일반적인 Android 앱에는 Activity, Fragment, Service, Content provider, Broadcast receiver 을 비롯한 여러 App Component(앱 구성요소) 들이 포함되어 있다.

개발자는 manifest 파일에서 이러한 앱 구성요소 대부분을 선언하고, Android 운영체제가 이 파일을 사용하여 기기의 전반적인 사용자 환경에 앱을 통합하는 방법을 결정한다.

Android 앱을 사용하는 유저는 짧은 시간 내에 여러 앱을 실행할 때가 많으므로 앱이 사용자 중심의 다양한 workflow에 맞게 조정될 수 있도록 해야한다.

예를 들어, 사용자가 SNS 앱에서 사진을 업로드 하는 작업은 어떻게 실행될까?

제일 먼저 SNS 앱이 카메라 인텐트를 트리거할 것이다. 그러면 Android 운영체제에서 이 요청을 처리하기 위해 카메라 앱을 실행시킨다.

이 순간에 사용자는 카메라 앱으로 이동함으로써 SNS 앱에서 나간 상황이지만 사용 환경은 끊김없이 연결되어 있는 상태라고 볼 수 있다.

그 다음, 카메라 앱에서 앨범을 보기 위해 또 다른 인텐트를 트리거하여 다른 앱을 실행할 수도 있다.

이 후에 사용자가 다시 SNS 앱으로 돌아와서 앨범에서 선택한 사진이나 카메라 앱으로 직접 찍은 사진을 업로드할 것이다.

여기서 중요한 것은 이 과정에서 언제든지 전화나 알림에 의해 사용 환경의 흐름이 끊어질 수가 있다는 것이다.

사용자는 이러한 흐름 중단에 대응하고 난 후 다시 SNS 앱으로 돌아가 하던 작업을 이어서 계속 하길 원할 것이다.

모바일 환경에서는 이렇게 다양한 앱을 시도 때도 없이 바꾸면서 사용하기 때문에 앱에서 이러한 흐름을 올바르게 처리해야 한다.

또한 휴대폰은 하드웨어 자원(메모리 등)이 제한되어 있으므로 Android 운영체제에서 새로운 앱을 위한 공간을 확보하도록 언제든지 일부 앱 프로세스를 종료할 수도 있어야 한다.

이러한 모바일 환경 조건을 고려해 볼 때 App Component(앱 구성요소)는 개별적이고 비순차적으로 실행될 수 있으며 운영체제나 사용자가 언제든지 앱 구성요소를 제거할 수 있다.

따라서 개발자가 이러한 여러 상황을 직접 제어할 수 없기 때문에 __앱 구성요소에 앱 데이터나 상태를 저장해서는 안 되며 앱 구성요소가 서로 종속되면 안된다!__

<br>

## 🔨 Android Architecture 기존 원칙
---

위에서 "앱 구성요소에 앱 데이터와 상태를 저장하면 안된다" 라는 것을 깨닫게 되었을 것이다.

그렇다면 앱을 어떻게 설계해야 할까?

이 질문에 대한 대답으로 앱을 설계하는데 필수적인 몇 가지 원칙들을 알아보자.

<br>

__첫 번째 원칙! "관심사를 분리하라"__

이 원칙은 안드로이드 앱 아키텍처 원칙 중 가장 중요한 원칙이다.

Activity나 Fragment에 모든 코드를 작성하는 것은 흔히 일어나는 "실수" 이다.

Activity나 Fragment 클래스를 UI 기반 클래스라고 하는데 UI 기반 클래스는 UI 및 운영체제와 상호작용을 처리하는 로직만 포함해야 한다.

UI 기반 클래스를 최대한 가볍게 유지하여야 많은 수명 주기 관련 문제를 피할 수 있다.

Activity와 Fragment 클래스는 Android 운영체제와 Application(앱) 사이의 계약을 나타내도록 이어주는 클래스일 뿐이다.

즉, 여러 수명 주기를 통해 사용자가 어떤 동작을 하더라도 그 흐름이 끊어지지 않도록 해주는 것이 UI 기반 클래스라고 생각하면 된다.

따라서 사용자 환경 흐름이 자연스러은 앱을 만들고자 한다면 UI 클래스에 대한 코드 의존성을 최소화하는 것이 좋다.

<br>

__두 번째 원칙! "Model(모델)에서 UI 만들기"__

안드로이드 앱 아키텍처 원칙 중 두 번째로 중요한 원칙은 모델에서 UI를 만들어야 한다는 것이다.

모델은 __앱의 데이터 처리를 담당하는 구성요소__ 로, 앱의 View 객체 및 앱 구성요소와 독립되어 있는 존재이다.

따라서 모델은 앱의 수명 주기나 여러 사용자 환경 흐름에 영향을 받지 않는다.

<br>

## 🔨 권장하는 Android Architecture
---

위에서 알아본 두 가지 원칙을 어떤 방법으로 지켜야 하는지 조금 더 자세히 알아보자.

예를 들어, 사용자 프로필을 나타내는 UI를 제작한다고 상상해보자.

또 REST API를 사용해서 프로필에 필요한 데이터를 서버를 통해 가져온다고 가정하자.

<br>

우리는 사용자 프로필 기능을 제작하기 위해 다음과 같은 아키텍처를 구성할 것이다.

![02](https://user-images.githubusercontent.com/31889335/83138586-fbe25f00-a125-11ea-9a82-be29de3fc4b0.PNG)

위 다이어그램을 보면 각 구성요소(Activity, ViewModel, Model, Repository, Remote Data Source)들이 한 단계 아래의 구성요소에만 종속되어 있음을 알 수 있다.

예를 들어 Activity나 Fragment는 ViewModel에만 종속되어 있는 것이다.

하지만 특이하게도 Repository(저장소)는 여러 개의 다른 클래스에 종속되는 유일한 요소임을 알 수 있다.

위 그림에서 Repository는 Model과 Remote Data Source에 종속되어 있다.

위와 같은 아키텍처로 앱을 구성하면 사용자가 이 앱을 닫은 후 몇 분 혹은 며칠 후에 다시 사용해도 앱이 로컬에 저장된 사용자 정보를 사용하여 프로필을 그대로 띄울 수 있게 될 것이다.

이러한 기대를 가지고 본격적으로 사용자 프로필 기능을 구현해보자.

<br>

__1. 사용자 프로필 UI 만들기__

이 예시에서 UI는 user_profile_layout.xml 이라는 xml 파일과 UserProfileFragment 라는 클래스로 구성된다고 생각하자.

UI를 만들려면 UI 안에 들어갈 정보들이 Model의 데이터 요소로 있어야 한다.

UI 안에 들어갈 정보들은 아래와 같이 총 두 가지라고 가정해보자.

- __사용자 ID__ : 사용자 ID는 Android 운영체제에서 이 앱의 프로세스를 제거해도 이 정보가 유지되어 앱을 다시 시작할 때 ID를 재사용할 수 있도록 하는 것이 좋다.

    따라서 사용자 ID는 프래그먼트 인수를 사용하여 프래그먼트에 전달하는 것이 좋다.

- __사용자 Object__ : 사용자에 관한 세부 정보를 가지고 있는 데이터 클래스이다.

<br>

__2. ViewModel 클래스 만들기__

위에서 본 다이어그램을 보면 Activity/Fragment 아래 단계에 ViewModel이 있는 것을 볼 수 있다.

그렇기 때문에 우리는 UI 를 구현하고 난 후 ViewModel 클래스를 생성해야 한다.

UserProfileViewModel이라는 이름으로 ViewModel 클래스를 만들자.

그렇다면 대체 ViewModel이 뭐냐!

__ViewModel은 UI 구성요소에 관한 데이터를 제공하고 Model과 커뮤니케이션하기 위한 데이터 처리 비즈니스 로직__ 이다.

예를 들어, ViewModel은 데이터를 로드하기 위해 위 다이어그램에 포함된 다른 구성요소를 호출하고 사용자 요청을 전달하여 데이터를 수정할 수 있다.

또한 __ViewModel은 UI 구성요소에 대해 알지 못하므로 구성 변경의 영향을 받지 않는다.__

여기서 구성 변경이란 예를 들면 기기 회전시 Activity 재생성 같은 이벤트를 말한다.

<br>

지금까지

- user_profile.xml : 화면의 UI 레이아웃 정의 파일

    ~~~kotlin
    class UserProfileViewModel : ViewModel() {
        // TODO() 는 원하는 작업 코드를 작성하라는 의미.
       val userId : String = TODO()
       val user : User = TODO()
    }
    ~~~

- UserProfileViewModel : UserProfileFragment 에서 데이터를 띄워줄 데이터를 준비하고 사용자 상호작용에 반응하는 클래스

- UserProfileFragment : 데이터를 표시하는 UI 컨트롤러

    ~~~kotlin
    class UserProfileFragment : Fragment() {
    private val viewModel: UserProfileViewModel by viewModels()

        override fun onCreateView(
            inflater: LayoutInflater, container: ViewGroup?,
            savedInstanceState: Bundle?
        ): View {
            return inflater.inflate(R.layout.user_profile, container, false)
        }
    }
    ~~~

이렇게 총 3개의 파일을 만들었을 것이다.

<br>

위 코드를 읽어보면 UI에 데이터를 띄워주는 부분이 없다는 것을 알 수 있다.

즉, UserProfileViewModel 클래스에서 user 에 들어갈 작업이 완료되면 이 user를 UI에 알리는 과정이 필요하다.

먼저 UserProfileViewModel에 있는 userId를 가져오려면 ViewModel에서 프래그먼트 인수에 엑세스해야 한다.

프래그먼트 인수를 전달할 수도 있고, 더 나은 방법으로 [SavedState 모듈](https://developer.android.com/topic/libraries/architecture/viewmodel-savedstate)을 사용해 ViewModel에서 직접 인수를 읽도록 할 수도 있다.

> Savedstate 모듈은 [여기](https://developer.android.com/topic/libraries/architecture/viewmodel-savedstate) 에서 자세히 알아볼 수 있다.

우리는 SavedState 모듈을 사용하는 걸로 해보자!

SavedState 모듈을 사용하기 위해 다음과 같이 UserProfileViewModel 클래스에 코드를 추가해보자.

~~~kotlin
class UserProfileFragment : Fragment() {
    private val viewModel: UserProfileViewModel by viewModels() {
        val userId : String = savedStateHandle["uid"] ?:
              throw IllegalArgumentException("missing user id")
       val user : User = TODO()
    }

        override fun onCreateView(
            inflater: LayoutInflater, container: ViewGroup?,
            savedInstanceState: Bundle?
        ): View {
            return inflater.inflate(R.layout.user_profile, container, false)
        }
    }
~~~

또 이 ViewModel 클래스를 Fragment 클래스에서 사용하도록 해야하므로 UserProfileFragment 클래스에 다음과 같은 코드를 추가하자.

~~~kotlin
private val viewModel: UserProfileViewModel by viewModels(
       factoryProducer = { SavedStateVMFactory(this) }
       ...
    )
~~~

여기까지는 userId를 사용하기 위해 ViewModel과 UI에 코드를 추가한 것이였다.

이제 user object를 Fragment에 알리는 코드를 작성해보자.

이 과정에서 Jetpack의 Architecture 라이브러리 중 하나인 __LiveData__ 가 사용된다.

> LiveData에 대한 자세한 내용은 [여기](https://developer.android.com/topic/libraries/architecture/livedata) 에서 알아볼 수 있다.

LiveData는 식별 가능한 데이터 홀더라고 생각하면 된다. 앱의 다른 구성요소에서는 이 홀더를 사용하여 상호 간에 명시적이고 종속적인 경로를 만들지 않고도 객체 변경사항을 모니터링 할 수 있다.

또 LiveData 는 Activity, Fragment, Service 등과 같은 앱 구성요소의 생명 주기 상태를 고려하여 객체 유출과 과도한 메모리 소비를 방지하기 위한 로직을 포함하고 있다.

LiveData를 사용하기 위해 UserProfileViewModel의 변수 user 부분을 다음과 같이 변경해야 한다.

~~~kotlin
class UserProfileViewModel(
       savedStateHandle: SavedStateHandle
    ) : ViewModel() {
    val userId : String = savedStateHandle["uid"] ?:
              throw IllegalArgumentException("missing user id")
    val user : LiveData<User> = TODO()
}
~~~

이렇게 하면 이제 데이터가 업데이트되면 UserProfileFragment에 정보가 전달될 것이다.

또 LiveData를 사용하면 수명 주기를 인식하기 떄문에 더 이상 필요하지 않은 참조를 자동으로 정리한다.

이 다음으로 데이터를 관찰하고 UI를 업데이트하도록 UserProfileFragment 클래스에 다음과 같은 코드를 추가하자.

~~~kotlin
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
       super.onViewCreated(view, savedInstanceState)
       viewModel.user.observe(viewLifecycleOwner) {
           // update UI
       }
}
~~~

위 코드로 인해 사용자 프로필 데이터가 업데이트될 떄마다 onChanged() 콜백이 호출되고 UI가 새로고침된다!

LiveData는 수명 주기를 자동으로 관찰하고 있다고 했으므로 onStop() 메소드 같은 수명주기에 관련된 메소드를 재정의하지 않아도 된다.

따라서 fragment가 활성 상태가 아니라면 onChanged() 콜백이 자동으로 호출되지 않는다.

또 LiveData는 Fragment의 onDestroy() 메소드가 호출되면 자동으로 관찰자를 삭제하기까지 한다.

이렇게 ViewModel을 작성할 때 주의할 점은 ViewModel에서 View 객체를 직접 참조하도록 하면 안된다는 점이다.

<br>

__3. 데이터 가져오기__




