---
layout: post
title:  "[안드로이드] 🔨 Android 프로젝트에서 사용되는 아키텍처(1탄)"
date:   2020-05-28 18:34:10 +0700
categories: [안드로이드]
---

<br>

## 🔨 안드로이드 app Architecture
---

[Android Jetpack이 뭐지?](https://choheeis.github.io/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C/2020/05/25/jetpack.html)에 대한 포스팅에서 Jetpack의 4가지 분류 중 Architecture 부분이 있었을 것이다!

jetpack 라이브러리들 중 Architecture 카테고리로 분류되어 있는 것들에는

![01](https://user-images.githubusercontent.com/31889335/83118033-c8dda280-a108-11ea-9e2e-201751a17ef1.PNG)

이런 것들이 있다!

위와 같은 Jetpack에서 지원하는 이런 라이브러리들을 사용해서 안드로이드 app을 개발할 때 프로젝트의 Architecture를 구성할 수 있다.

안드로이드에서 권장하는 아키텍처는 어떤 구조인지 [앱 아키텍처 가이드](https://developer.android.com/jetpack/docs/guide) 문서를 읽어보고 알아보자!

이 문서를 읽어보면 좋은 퀄리티의 강력한 앱을 개발하기 위한 권장 아키텍처가 무엇인지 알 수 있을 것이다.

<br>

## 🔨 모바일 앱 사용자 환경이란?
---

먼저 안드로이드 권장 아키텍처를 본격적으로 살펴보기 전에 모바일 앱 사용자 환경에 대해 살짝 알아보자.

일반적인 Android 앱에는 Activity, Fragment, Service, Content provider, Broadcast receiver 등의 여러 App Component(앱 구성요소) 들이 포함되어 있다.

개발자는 manifest 파일에서 이러한 앱 구성요소 대부분을 선언하고, Android 운영체제가 이 파일을 사용하여 기기의 전반적인 사용자 환경에 앱을 통합하는 방법을 결정한다.

Android 앱을 사용하는 유저는 짧은 시간 내에 여러 앱을 실행할 때가 많으므로 앱이 사용자 중심의 다양한 workflow에 맞게 조정될 수 있어야 한다.

예를 들어, 사용자가 SNS 앱에서 사진을 업로드 하는 작업은 어떻게 실행될까?

제일 먼저 SNS 앱이 카메라 인텐트를 트리거할 것이다. 그러면 Android 운영체제에서 이 요청에 따라 카메라 앱을 실행시킨다.

이 순간에 사용자는 카메라 앱으로 이동함으로써 SNS 앱에서 나간 상황이지만 사용 환경은 끊김없이 연결되어 있는 상태라고 볼 수 있다.

그 다음, 카메라 앱에서 앨범을 보기 위해 앨범 인텐트를 트리거할 수도 있다.

이 후에 사용자가 다시 SNS 앱으로 돌아와서 앨범에서 선택한 사진이나 카메라 앱으로 직접 찍은 사진을 업로드할 것이다.

여기서 중요한 것은 이 과정에서 언제든지 전화나 알림에 의해 사용 환경의 흐름이 끊어질 수도 있다는 것이다.

사용자는 이러한 흐름 중단에 대응하고 난 후 다시 SNS 앱으로 돌아가 하던 작업을 이어서 계속 하길 원할 것이다.

모바일 환경에서는 이렇게 다양한 앱을 시도 때도 없이 바꾸면서 사용하기 때문에 앱에서 이러한 흐름을 올바르게 처리해야 한다.

또한 휴대폰은 하드웨어 자원(메모리 등)이 제한되어 있으므로 Android 운영체제에서 새로운 앱을 위한 공간을 확보하도록 언제든지 일부 앱 프로세스를 종료할 수도 있어야 한다.

이러한 모바일 환경 조건을 고려해 볼 때 App Component(앱 구성요소)는 개별적이고 비순차적으로 실행될 수 있으며 운영체제나 사용자가 언제든지 앱 구성요소를 제거할 수 있다.

따라서 개발자가 이러한 여러 상황을 직접 제어할 수 없기 때문에 __앱 구성요소에 앱 데이터나 상태를 저장해서는 안 되며 앱 구성요소가 서로 종속되어 있는 상태면 안된다!__

<br>

## 🔨 Android Architecture 기존 원칙
---

위에서 __"앱 구성요소에 앱 데이터와 상태를 저장하면 안된다"__ 라는 것을 깨닫게 되었을 것이다.

그렇다면 앱을 어떻게 설계해야 할까?

이 질문에 대한 대답으로 앱을 설계하는데 필수적인 몇 가지 원칙들을 알아보자.

<br>

__1️⃣ 첫 번째 원칙! "분리하라"__

이 원칙은 안드로이드 앱 아키텍처 원칙 중 가장 중요한 원칙이다.

안드로이드 초기 개발자들은 Activity나 Fragment에 모든 코드를 작성하는 "실수"를 범할 것이다.

Activity나 Fragment 클래스를 UI 기반 클래스라고 하는데 원칙적으로 __UI 기반 클래스는 UI 및 운영체제와 상호작용을 처리하는 로직만 포함해야 한다.__

UI 기반 클래스를 최대한 가볍게 유지하여야 많은 수명 주기 관련 문제를 피할 수 있을 것이다.

Activity와 Fragment 클래스는 Android 운영체제와 Application(앱) 사이의 계약을 나타내도록 이어주는 클래스일 뿐이다.

즉, 여러 수명 주기를 제어함으로써 사용자가 어떤 동작을 하더라도 그 흐름이 끊어지지 않도록 해주는 것이 UI 기반 클래스라고 생각하면 된다.

__따라서 UI 기반 클래스에서는 수명 주기에 따라 발생할 수 있는 상황에 대처하는 코드만 작성하는 것이 좋다.__

따라서 사용자 환경 흐름이 자연스러은 앱을 만들고자 한다면 UI 클래스에 대한 코드 의존성을 최소화하는 것이 좋다.

<br>

__2️⃣ 두 번째 원칙! "Model(모델)과 UI 분리하기"__

안드로이드 앱 아키텍처 원칙 중 두 번째로 중요한 원칙은 모델과 UI를 분리해야 한다는 것이다.

모델은 __앱의 데이터를 담당하는 구성요소__ 로, 앱의 View 객체 및 앱 구성요소와 독립되어 있는 존재이다.

따라서 모델은 앱의 수명 주기나 여러 사용자 환경 흐름에 영향을 받지 않는다.

<br>

## 🔨 권장하는 Android Architecture
---

위에서 알아본 두 가지 원칙을 어떤 방법으로 지켜야 하는지 조금 더 자세히 알아보자.

예를 들어, 사용자 프로필 UI를 제작한다고 상상해보자.

사용자 프로필 관련 정보는 REST API를 사용해서 서버를 통해 가져온다고 가정하자.

<br>

우리는 사용자 프로필 기능을 제작하기 위해 다음과 같은 아키텍처를 구성해야만 한다.

아래 구조가 안드로이드에서 권장하는 아키텍처이다.

![02](https://user-images.githubusercontent.com/31889335/83138586-fbe25f00-a125-11ea-9a82-be29de3fc4b0.PNG)

위 다이어그램을 보면 각 구성요소(Activity, ViewModel, Model, Repository, Remote Data Source)들이 딱 한 단계 아래의 구성요소에만 종속되어 있음을 알 수 있다.

예를 들어 Activity나 Fragment는 ViewModel에만 종속되어 있는 것이다.

하지만 특이하게도 Repository(저장소)는 여러 개의 다른 클래스에 종속되는 유일한 요소임을 알 수 있다.(위 그림에서 Repository는 Model과 Remote Data Source에 종속되어 있다.)

위와 같은 구조를 기억하고 본격적으로 사용자 프로필 기능을 구현해보자!

<br>

__1️⃣ 사용자 프로필 UI 레이아웃 및 UI 클래스 만들기__

사용자 프로필 UI를 위해 user_profile_layout.xml 이라는 xml 파일과 UserProfileFragment 라는 클래스를 만들었다고 가정하자.

또 UI 안에 들어갈 정보들을 Model의 데이터 요소로 만들었다고 하자.

마지막으로 프로필 UI 안에 들어갈 정보들은 아래와 같이 총 두 가지라고 가정해보자.

- __사용자 ID__ : 사용자 ID는 Android 운영체제에서 이 앱의 프로세스를 제거해도 이 정보가 계속 유지되어 앱을 다시 시작할 때 ID를 재사용할 수 있도록 하는 역할이다.

    따라서 사용자 ID는 fragment argument 사용하여 프래그먼트에 전달하는 것이 좋다.

- __사용자 Object__ : 사용자 프로필에 관한 세부 정보를 가지고 있는 데이터 클래스이다. (예를 들어, 이 데이터 클래스에는 사용자 닉네임, 나이, 사진 등이 포함될 수 있다.)

<br>

__2️⃣ ViewModel 클래스 만들기__

위에서 본 안드로이드 아키텍처 다이어그램을 보면 Activity/Fragment 아래 단계에 ViewModel이 있는 것을 알 수 있다.

그렇기 때문에 우리는 UI 를 구현하고 난 후 ViewModel 클래스를 생성해야 한다.

UserProfileViewModel이라는 이름으로 ViewModel 클래스를 만들자.

그렇다면 대체 [ViewModel](https://developer.android.com/topic/libraries/architecture/viewmodel)이 뭐냐!

__ViewModel은 UI 구성요소에 데이터를 제공하고 Model과 커뮤니케이션하기 위한 데이터 처리 비즈니스 로직__ 이라고 할 수 있다.

예를 들어, ViewModel은 데이터를 UI 구성요소에 반영하기 위해 ViewModel 하위 단계인 Repository를 호출하고 사용자 요청을 전달하여 데이터를 수정할 수 있다.

또한 __ViewModel은 UI 구성요소에 대해 전혀 알지 못하므로 activity나 fragment의 수명주기 변경에 영향을 받지 않는다.__

<br>

결과적으로

- user_profile_layout.xml : 프로필 화면의 레이아웃 정의 파일

- UserProfileFragment : 데이터를 표시하는 UI 기반 클래스

    ~~~kotlin
    class UserProfileFragment : Fragment() {
    // To use the viewModels() extension function, include
    // "androidx.fragment:fragment-ktx:latest-version" in your app
    // module's build.gradle file.
    private val viewModel: 
    // 여기서 ViewModel 인스턴스를 생성한다.
    UserProfileViewModel by viewModels()

        override fun onCreateView(
            inflater: LayoutInflater, container: ViewGroup?,
            savedInstanceState: Bundle?
        ): View {
            return inflater.inflate(R.layout.user_profile, container, false)
        }
    }
    ~~~

- UserProfileViewModel : UserProfileFragment 에서 띄워줄 데이터를 준비하거나 사용자 요청(서버의 데이터 값 수정 등)에 반응하는 클래스

    ~~~kotlin
    class UserProfileViewModel : ViewModel() {
        // TODO() 는 원하는 작업 코드를 작성하라는 의미.
       val userId : String = TODO()
       val user : User = TODO()
    }
    ~~~



이렇게 총 3개의 파일을 만들면 된다.

<br>

위 코드를 해석해보면 UI에 데이터를 세팅해주는 부분이 없다는 것을 알 수 있다.

먼저 UserProfileViewModel에 있는 userId를 가져오려면 ViewModel에서 프래그먼트 인자(fragment argument)에 엑세스해야 한다.

프래그먼트 인자를 전달할 수도 있고, 더 나은 방법으로 [SavedState 모듈](https://developer.android.com/topic/libraries/architecture/viewmodel-savedstate)을 사용해 ViewModel에서 직접 userId를 읽도록 할 수도 있다.

> Savedstate 모듈은 [여기](https://developer.android.com/topic/libraries/architecture/viewmodel-savedstate) 에서 자세히 알아볼 수 있다.

우리는 SavedState 모듈을 사용하는 걸로 해보자!

SavedState 모듈을 사용하기 위해 UserProfileViewModel 클래스를 다음과 같이 수정해보자.

~~~kotlin
class UserProfileViewModel(
    savedStateHandle: SavedStateHandle
) : ViewModel() {
    // userId 를 savedState 모듈에서 가져온다.
    val userId : String = savedStateHandle["uid"] ?:
            throw IllegalArgumentException("missing user id")
    val user : User = TODO()
}
~~~

추가로 UserProfileFragment에서 작성한 viewModel 인스턴스 생성 코드를 다음과 같이 수정해야 한다.

~~~kotlin
private val viewModel: UserProfileViewModel by viewModels(
       factoryProducer = { SavedStateVMFactory(this) }
)
~~~

여기까지하면 userId를 savedState 모듈을 사용하기 위한 작업을 완료한 것이다.

<br>

이제 User object에 있는 데이터를 Fragment의 구성요소에 세팅하는 코드를 작성해보자.

이 과정에서 Jetpack의 Architecture부분의 라이브러리 중 하나인 __[LiveData](https://developer.android.com/topic/libraries/architecture/livedata)__ 가 사용된다.

LiveData는 관찰 가능한 데이터 홀더라고 생각하면 된다. 즉, 데이터를 잡고 있는 홀더 클래스이다.

이 홀더를 사용하면 activity나 fragment에서 상호 간에 명시적이고 종속적인 경로를 만들지 않고도 User 데이터의 변경사항을 모니터링 할 수 있다.

또 LiveData 는 Activity, Fragment, Service 등과 같은 앱 구성요소의 생명 주기 상태를 자체적으로 고려하여 데이터 유출과 과도한 메모리 소비를 방지하기 위한 로직을 포함하고 있다.

<br>

LiveData를 사용하기 위해서는 UserProfileViewModel의 변수 user 부분을 다음과 같이 변경해야 한다.

~~~kotlin
class UserProfileViewModel(
       savedStateHandle: SavedStateHandle
    ) : ViewModel() {
    val userId : String = savedStateHandle["uid"] ?:
              throw IllegalArgumentException("missing user id")
    //User 라는 데이터 클래스는 LiveData로 홀드(잡기)한다.
    val user : LiveData<User> = TODO()
}
~~~

이렇게 하면 이제 User 데이터는 LiveData에 잡혀(홀드)있는 상황이 된다.

이 다음으로 이러한 LiveData 데이터를 ViewModel이 관찰하고 UI를 업데이트하도록 UserProfileFragment 클래스의 onViewCreated 함수 안에 다음과 같은 코드를 추가하자.

~~~kotlin
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
       super.onViewCreated(view, savedInstanceState)

       // Fragment 에서 생성한 ViewModel 인스턴스를 사용하여 ViewModel 안에서 정의된 User 데이터 클래스를 관찰할 것임을 정의하는 부분이다.
       viewModel.user.observe(viewLifecycleOwner) {
           // update UI
       }
}
~~~

위 코드로 인해 사용자 프로필 데이터가 업데이트될 때마다 onChanged() 콜백이 호출되고 UserProfilFragment와 연관된 UI에 변경된 데이터가 자동으로 세팅될 것이다.

LiveData는 수명 주기를 자동으로 관찰하고 있기 때문에 개발자가 onStop() 메소드 같은 수명주기 메소드에 따로 데이터를 재세팅하는  코드를 정의하지 않아도 된다.

왜냐하면 LiveData는 자체적으로 자신을 관찰하고 있는 Fragment나 Activity가 active 상태가 아니라면 onChanged() 콜백이 호출되지 않기 때문이다.

또 LiveData는 자신을 관찰하고 있는 Fragment나 Activity의 onDestroy() 메소드가 호출되면(사용자 흐름에서 화면이 제거되는 경우) 자동으로 관찰자를 삭제하기까지 한다.

이렇게 LiveData를 사용함으로써 ViewModel 안에서 직접 View 객체를 참조해 데이터를 세팅하지 않아도 된다. (즉, ViewModel은 View를 모르는 상황인 것이다!)

<br>

__3️⃣ 데이터 가져오기__

위에서 LiveData를 사용하여 UserProfileViewModel에 있는 User 객체를 UserProfileFragment에 연결하였다.

이제 어떻게 하면 사용자 프로필 정보인 User 객체의 데이터를 가져올 수 있을까?

맨 처음에 REST API를 사용하여 서버에서 프로필 정보가져온다고 가정하였다.

따라서 Retrofit 라이브러리와 같은 안드로이드 http 통신 라이브러리를 사용하여 서버에서 데이터를 가져오면 된다!

예를 들어 Retrofit 라이브러리를 사용하여 Webservice를 다음과 같은 interface 형태로 정의했다고 해보자.

~~~kotlin
interface Webservice {
       @GET("/users/{user}")
       fun getUser(@Path("user") userId: String): Call<User>
}
~~~

안드로이드 아키텍처에서 이러한 Webservice를 호출하는 방법은 두 가지로 나뉘어진다.

__첫 번째 방법__ 은 UserProfileViewModel 클래스 안에서 Webservice를 직접 호출하여 데이터를 가져오고 이 데이터를 LiveData 객체에 할당하는 것이다.

하지만 이 방법을 사용하면 앱의 규모가 커지게 될 때 ViewModel 클래스에 코드가 많아져 유지보수가 어려워질 수 있고, UserProfileViewModel에 너무 많은 책임을 부여하게 되어 __분리하기__ 원칙을 위반하게 된다.

<br>

따라서 __두 번째 방법__ 인 Webservice 호출을 위한 ViewModel이 아닌 새로운 저장소를 만드는 방법을 사용하자!

이 저장소를 __Repository__ 라고 하고, 맨 처음 보여준 Architecture 구조에서도 Repository가 ViewModel의 한 단계 아래에 존재하는 것을 알 수 있다.

Repository에서는 Webservice를 호출하여 데이터 가져오는 작업을 처리하도록 하면 된다.

<br>

UserRepository 클래스를 하나 생성하고 다음과 같은 코드를 작성해보자.

~~~kotlin
class UserRepository {
    // REST API interface 선언
    private val webservice: Webservice = TODO()

    // Webservice에서 정의해 준 getUser 함수 호출   
    fun getUser(userId: String): LiveData<User> {
        // This isn't an optimal implementation. We'll fix it later.
        val data = MutableLiveData<User>()
        webservice.getUser(userId).enqueue(object : Callback<User> {
            override fun onResponse(call: Call<User>, response: Response<User>) {
                data.value = response.body()
            }
            // Error case is left out for brevity.
            override fun onFailure(call: Call<User>, t: Throwable) {
                TODO()
            }
        })
        return data
    }
}
~~~

이렇게 함으로써 UserProfileViewModel은 데이터를 가져오는 방법(어떤 REST API 호출 함수를 호출해서 가져오는지 등)을 알지 못하기 때문에 여러 개의 서로 다른 API를 호출하여 얻은 데이터를 ViewModel에 제공할 수 있게 된다.

<br>

여기까지해서 UI - ViewModel - Repository 까지의 구조에 대해서 알아보고 이해하였다!

이 시점에서 __안드로이드 아키텍처의 구성요소간 종속성 관리__ 에 대해서 생각해볼 필요가 있다.

UserRepository 클래스에서 사용자의 데이터를 가져오기 위해 Webservice 의 인스턴스를 생성했었던 것을 생각해보자.

![03](https://user-images.githubusercontent.com/31889335/83407257-4a0ba100-a44b-11ea-9849-238ebd515768.PNG)

Webservice는 Retrofit 라이브러리를 공부하면 간단히 만들 수 있지만 Webservice를 만드는 것 외에도 Webservice의 종속성에 대해서 고려할 줄도 알아야한다.

여기서 말하는 Webservice의 종속성이란 다음과 같다.

Webservice의 인스턴스 생성이 필요한 각각의 Repository 클래스에서 Webservice 인스턴스 생성 코드를 각각 따로 따로 작성해주어야 한다.

하지만 클래스 별로 새로운 WebService 인스턴스를 생성하는 작업은 앱의 리소스(자원, 메모리 등) 소모량이 커질 가능성이 크다.

즉, Webservice가 Repository에 종속되어 있지만 어떻게 하면 좀 더 효율적으로 종속되어 있을 수 있도록 할 수 있는지 고민해봐야 한다는 것이다.

안드로이드 아키텍처에서는 이러한 고민을 해결하기 위해 다음과 같은 __디자인 패턴__ 을 사용하길 권장한다.

<br>

- [DI(Dependency Injection) 패턴](https://en.wikipedia.org/wiki/Dependency_injection) 사용하기(종속성 주입하기)

    DI 패턴을 사용하면 클래스 안에서 직접 종속성을 구성할 필요 없이 종속성을 정의할 수 있다.

    런타임시 다른 특정한 클래스가 클래스 별 종속성을 제공함으로써 각 클래스 내에서는 종속성을 따로 정의할 필요가 없는 것이다.

    <br>

    > 여기서 종속성을 정의한다는 것은 Webservice를 사용하기 위해 UserRepository에서 Webservice 인스턴스를 생성한 것, UserProfileFragment에서 ViewModel을 사용하기 위해 UserProfileViewModel 인스턴스를 생성한 것과 같이 안드로이드 아키텍처의 아래 단계에 존재하는 구성 요소를 사용하기 위해 해당 인스턴스를 선언하고 생성하는 것을 말하는 군!

    <br>

    이러한 DI 패턴을 구현하려면 [Dagger2](https://dagger.dev/) 라는 라이브러리를 사용하는 것이 좋다.

    Dagger2는 종속성 트리를 따라 이동하며 객체를 자동으로 구성하고 종속성을 위한 컴파일 시간을 보장해주는 라이브러리이다.

    <br>

- [Service Locator 패턴](https://en.wikipedia.org/wiki/Service_locator_pattern) 사용하기

    Service Locator 패턴은 클래스가 자신의 종속 항목을 직접 구성하는 대신 종속 항목을 가져올 수 있는 레지스트리를 제공하는 패턴이다.

    DI 패턴보다 Service Locator 패턴의 구현이 더 쉬우므로 DI에 익숙하지 않다면 Service Locator를 사용하는 것도 좋은 방법이다.

<br>

__4️⃣ ViewModel과 Repository 연결하기__

이제 UserRepository에서 가져온 데이터를 UserProfileViewModel에서 사용할 수 있도록 UserProfileViewModel 클래스의 user 부분 코드를 다음과 같이 수정해보자.

~~~kotlin
class UserProfileViewModel @Inject constructor(
       savedStateHandle: SavedStateHandle,
       userRepository: UserRepository
    ) : ViewModel() {
       val userId : String = savedStateHandle["uid"] ?:
              throw IllegalArgumentException("missing user id")
       val user : LiveData<User> = userRepository.getUser(userId)
}
~~~

<br>

여기까지 안드로이드 권장 아키텍처 패턴에 대해서 알아보았다!

<br>