---
layout: post
title:  "[Kotlin] 💅 안드로이드에서 사용되는 코틀린 패턴"
date:   2020-07-22 18:34:10 +0700
categories: [Kotlin]
---

## 💅 안드로이드에서 사용되는 코틀린 패턴이 있다?
---

Kotlin으로 안드로이드 개발을 할 때 자주 사용되는 언어 패턴이 있다!

패턴이 생기게 된 이유는 코틀린이라는 언어를 통해 안드로이드 개발을 할 때 코틀린의 가장 유용한 점들을 적용하기 위해서이다.

* 상속할 때 사용되는 코틀린 패턴
* null 허용 여부 및 초기화에서 사용되는 코틀린 패턴
* 정적 변수를 선언할 때 사용되는 코틀린 패턴

<br>

## __상속__ 할 때 사용되는 코틀린 패턴
---

__어떤 패턴들이 있나?__

1. __상속할 때는 : 기호 사용하는 패턴__

    코틀린으로 안드로이드 개발을 하다보면 다른 클래스를 부모 클래스로 상속해야 하는 경우가 많다. 상속을 할 때는 __:__ 기호를 사용한다.

    ~~~kotlin
    class LoginFragment : Fragment() 
    ~~~

    <br>

2. __함수를 오버라이드하여 재정의할 때는 override 키워드 사용하는 패턴__

    이렇게 Fragment 클래스를 상속받은 후에는 Fragment 클래스에 정의되어 있는 여러 프래그먼트 수명 주기 함수들을 override 하여 재정의할 수 있다.

    함수를 재정의할 떄는 __override__ 키워드를 fun 키워드 앞에 붙이면 된다.

    ~~~kotlin
    override fun onCreateView(
            inflater: LayoutInflater,
            container: ViewGroup?,
            savedInstanceState: Bundle?
    ): View? {
        return inflater.inflate(R.layout.login_fragment, container, false)
    }
    ~~~

    <br>

3. __super 키워드를 사용하여 함수를 참조하는 패턴__

    부모 클래스에 포함되어 있는 멤버 함수를 호출하려면 __super__ 키워드를 사용한다.

    ~~~kotlin
    // onViewCreated를 그대로 호출하고 그 후에 다른 작업을 실행시키는 방식으로 재정의하려 super.~~를 사용한 모습
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        println("Kotlin")
    }
    ~~~

<br>

## __초기화__ 할 때 사용되는 코틀린 패턴
---

__어떤 패턴들이 있나?__

1. __lateinit 키워드를 통해 초기화를 지연시키는 패턴__

    Kotlin에서는 객체를 선언할 때 반드시 객체의 속성을 초기화해줘야 한다. 그래야 해당 클래스의 인스턴스를 가져올 때 클래스 내부 변수들을 즉시 참고할 수 있기 때문이다.

    > --> 클래스의 인스턴스를 가져온다?
    >
    > 예를 들어 A라는 클래스가 존재한다고 할 때 다른 곳에서 A라는 클래스를 사용하려면 가장 먼저 A라는 클래스의 인스턴스를 생성해야 한다. 즉, 클래스의 생성자 함수를 호출하여야 한다는 것이다. 이것을 클래스의 인스턴스를 가져온다라고 하는 것!

    하지만 Fragment 클래스의 인스턴스를 가져와야 하는 경우 Fragment의 View객체는 Fragment의 생명 주기 함수인 __onCreateView__ 를 호출하기 전까지는 준비가 되어있지 않게 된다. 즉, View의 속성 초기화를 Fragment 클래스의 인스턴스를 가져오는 순간으로부터 미뤄야 하는 경우가 발생한다.

    ~~~kotlin
    // onCreateView 함수 예시
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        val view = inflater.inflate(R.layout.fragment_main, container, false);
        return view
    }
    ~~~

    위 코드와 같이 Fragment는 생명 주기 함수 중 onCreateView 함수가 호출되서야 inflater를 사용해 fragment xml 파일을 메모리에 올리기 때문이다.

    따라서 아래 코드와 같이

    ~~~kotlin
    class LoginFragment : Fragment() {

    // 이런 식으로 바로 초기화 할 수 없다. onCreateView함수가 호출되기 전에는 View들이 메모리에 올라와있지 않아 해당 뷰 객체를 찾을 수 없기 때문이다.
    // 컴파일 에러남
    private var usernameEditText: EditText = findViewById(R.id.~~)
    private var passwordEditText: EditText = findViewById(R.id.~~)
    private var loginButton: Button = findViewById(R.id.~~)
    private var statusTextView: TextView = findViewById(R.id.~~)

    ...
    }
    ~~~

    <br>

    이러한 경우를 대비하여 코틀린에서는 __lateinit__ 이라는 키워드를 제공하여 객체의 초기화를 미룰 수 있게 한다!

    하지만 lateinit 으로 객체의 초기화를 미룰 경우에는 최대한 빨리 초기화 시켜줘야 한다.

    ~~~kotlin
    class LoginFragment : Fragment() {

    // lateinit 키워드를 사용하여 초기화를 미뤘다
    private lateinit var usernameEditText: EditText
    private lateinit var passwordEditText: EditText
    private lateinit var loginButton: Button
    private lateinit var statusTextView: TextView

    // onCreateView 함수가 호출되고 난 바로 다음에 호출되는 onViewCreated 함수 안에서 초기화시켜줌으로써 최대한 빨리 초기화 시켜주는 모습!
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        usernameEditText = view.findViewById(R.id.username_edit_text)
        passwordEditText = view.findViewById(R.id.password_edit_text)
        loginButton = view.findViewById(R.id.login_button)
        statusTextView = view.findViewById(R.id.status_text_view)
    }

    ...
    }
    ~~~

<br>

## __정적 변수__ 를 선언할 때 사용되는 코틀린 패턴
---

__어떤 패턴들이 있나?__

1. __companion object를 사용하여 정적 변수를 선언하는 패턴__

    자바에서는 __static__ 이라는 키워드로 절대 변할 수 없는 변수를 선언하게 되어있다.

    이와 같은 기능을 코틀린에서는 __companion object(동반자 객체)__ 라는 키워드로 제공하고 있다.

    ~~~kotlin
    class LoginFragment : Fragment() {

        ...

        companion object {
            // const라는 키워드도 붙인다
            private const val TAG = "LoginFragment"
        }
    }
    ~~~

    위 코드는 정적 변수인 TAG 를 companion object 블록 안에서 선언한 코드이다!

    정적 변수를 companion object 키워드를 사용하지 않고 LoginFragment 파일의 가장 최상위에 작성할 수도 있지만 파일 최상위에는 정적 변수 이외에도 여러 객체 변수들이 선언되어 있을 수 있기 때문에 companion object 블럭으로 정적 변수들만 분리시켜놓는 것이 좋다.

<br>