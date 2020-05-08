---
layout: post
title:  "[안드로이드] 📰 Bottom Sheet 만드는 나만의 라이브러리"
date:   2020-05-02 18:34:10 +0700
categories: [안드로이드]
---

> [머터리얼 디자인 Sheets: bottom](https://material.io/components/sheets-bottom) 와 [Bottom Sheets Android](https://material.io/develop/android/components/bottom-sheet-behavior/) 을 참고한 포스팅!

<br>

## 📰 Bottom Sheet 가 뭘까~?
---

구글 머티리얼 디자인의 컴포넌트 중 하나인 BottomSheet 만드는 법을 알아보기 전에 BottomSheet가 무엇인지 알아보자!

![01](https://user-images.githubusercontent.com/31889335/80859165-8192fc00-8c99-11ea-8440-4e4b5c8d869a.PNG)

BottomSheet는 위와 같이 화면의 아래에 걸려있는 화면을 말한다.

이러한 BottomSheet를 사용하면 좋은 경우에는 3가지 경우가 있다.

1. __기본적인 Bottom Sheets 를 만들어야 할 경우__

    어떠한 화면의 보충 기능들을 Bottom Sheets에 담아둘 수 있다.

    다음 그림과 같이 유저가 bottom sheet 아래의 화면과 상호작용하는 동안에도 bottom sheet가 보이고 있게 할 수도 있다.

    ![02](https://user-images.githubusercontent.com/31889335/80859356-24984580-8c9b-11ea-994a-2873e49e2ec6.PNG)

    따라서 간단한 멀티테스킹 기능이 필요할 때 유용하다.

    위 그림에서는 음악 앨범들의 리스트를 볼 수 있음과 동시에 음악의 정지 및 시작 기능을 bottom sheet를 통해 조작할 수 있는 경우이다.

    <br>

2. __모달(Modal) bottom sheets 를 만들어야 할 경우__

    다음 그림과 같은 모달 bottom sheets는 메뉴 기능이나 간단한 다이얼로그 기능을 대체할 수 있는 화면이다.

    ![03](https://user-images.githubusercontent.com/31889335/80859430-f7986280-8c9b-11ea-80ce-5a61dcfda9e9.PNG)

    모달 역할로써 bottom sheets를 사용하는 경우에는 bottom sheets 화면이 원래 화면을 그림자 처리 함으로써 bloking(막음) 한다.
    
    따라서 반드시 bottom sheets를 통한 상호작용이 끝나면 화면에서 해산시켜야 한다. --> 원래 화면을 다시 사용할 수 있도록 bottom Sheets를 확실히 내려버려야 한다는 뜻.

    또한 만약 유저가 그림자 처리된 뒷 화면을 누르면 모달 Bottom Sheet는 사라지게 된다.

    <br>

3. __확장 Bottom Sheets 를 만들어야 할 경우__

    다음 그림과 같은 확장 Bottom Sheets 는 작지만 유저가 확장시킬 수 있는 화면이다.

    ![04](https://user-images.githubusercontent.com/31889335/80859601-2b27bc80-8c9d-11ea-89f2-63b8a507d44c.PNG)

    유저가 이 작은 확장 bottom sheet를 누르면 [이와 같이](https://kstatic.googleusercontent.com/files/90f33f120dc01c83ff66bc84035d370c56435a5ffc3687a601d222524b709ec62949d290e311a577a57cd025aaf0d26e426a3b06ec16e93c86c9ee0423aa8d10) 작동하게 된다.

    <br>

위와 같이 Bottom Sheet를 사용할 수 있는 3가지 경우에 대해서 알아보았다.

이제 3가지 경우 중 '모달 bottom sheet' 를 만드는 방법을 알아보자!

<br>

## 📰 모달 Bottom Sheet 만드는 나만의 라이브러뤼~
---

Modal Bottom Sheet는 __BottomSheetDialogFragment__ 라고도 부른다는 것을 알아두고 [Modal Bottom Sheets : Android](https://material.io/develop/android/components/bottom-sheet-dialog-fragment/) 와 [Modal Bottom Sheet Youtube Video](https://www.youtube.com/watch?v=IfpRL2K1hJk) 를 참고하여 모달 Bottom Sheet를 어떻게 만드는지 알아보자.

> BottomSheetDialogFragment class overview --> [여기](https://developer.android.com/reference/com/google/android/material/bottomsheet/BottomSheetDialogFragment)
>
> BottomSheetDialogFragment class definition --> [여기](https://github.com/material-components/material-components-android/blob/master/lib/java/com/google/android/material/bottomsheet/BottomSheetDialogFragment.java)

<br>

1. __머티리얼 디자인 종속성 추가하기__

    Bottom Sheet는 머티리얼 디자인에 속해있는 컴포넌트이므로 프로젝트에 머티리얼 디자인 종속성을 추가해야 한다.

    ~~~kotlin
    implementation 'com.google.android.material:material:1.2.0-alpha06'
    ~~~

    > 2020/05/02 기준으로 머티리얼 디자인의 최신 버전은 1.2.0-alpha06 이다.

    <br>

2. __Modal Bottom Sheet xml 코드 작성하기__

    ![05](https://user-images.githubusercontent.com/31889335/80860753-eb64d300-8ca4-11ea-815b-6eca5c070e5f.PNG)

    위와 같이 res 폴더의 layout 폴더 안에 modal bottom sheet가 될 layout.xml 파일을 만든다.

    ![06](https://user-images.githubusercontent.com/31889335/80860795-1bac7180-8ca5-11ea-8b3f-23149c076c35.PNG)

    이렇게 modal_bottom_sheet_layout.xml 을 작성하였다!

    <br>

3. __BottomSheetDialogFragment 클래스를 상속하는 클래스 생성하기__

    [BottomSheetDialogFragment class overview](https://developer.android.com/reference/com/google/android/material/bottomsheet/BottomSheetDialogFragment) 를 보면 다음과 같이 BottomSheetDialogFragment 라는 클래스의 기능과 어떤 멤버 함수들이 있는지를 간단히 볼 수 있다.

    ![07](https://user-images.githubusercontent.com/31889335/80860830-4f879700-8ca5-11ea-9d9a-a66d015f8dfe.PNG)

    Modal Bottom Sheet는 이 BottomSheetDialogFragment 라는 클래스를 상속하기 때문에 우리도 아래와 같이 BottomSheetDialogFragment를 상속하는 클래스를 만들어주면 된다.

    
    ![10](https://user-images.githubusercontent.com/31889335/80860963-595dca00-8ca6-11ea-8cde-1d88d6443779.PNG)

    ![08](https://user-images.githubusercontent.com/31889335/80860875-b016d400-8ca5-11ea-8706-776b834dc7a5.PNG)

    <br>

    그 다음, onCreateView() 함수를 오버라이딩하여 처음에 만든 modal_bottom_sheet_layout.xml 을 inflate 하도록 코드를 작성해줘야 한다.

    ![09](https://user-images.githubusercontent.com/31889335/80860941-3b906500-8ca6-11ea-811e-e847a74c63bb.PNG)

    <br>

3. __Modal bottom sheet 올라오게 하기__

    ![12](https://user-images.githubusercontent.com/31889335/80861073-341d8b80-8ca7-11ea-9a3d-9da9ff44be08.PNG)

    이제 위와 같이 미리 만들어놓은 MainAcitivity의 Button을 누르면 Modal Bottom Sheet가 위로 올라오도록 해보자.

    MainActivity.kt 의 onCreate 함수 안에 다음과 같은 클릭 리스너를 작성하면 된다.

    ![11](https://user-images.githubusercontent.com/31889335/81403244-efde2f80-916d-11ea-9a3e-6be91da1b2f2.PNG)

    이 때, 상속한 BottomSheetDialogFragment 클래스의 멤버 함수인 show() 를 사용하게 된다.

    여기까지 작성한 후, 프로젝트를 build 해보면

    ![13](https://user-images.githubusercontent.com/31889335/80861169-be65ef80-8ca7-11ea-863c-744c396f874f.jpg)

    '모달 바텀 시트야 올라와라!' 라는 버튼을 눌렀을 때 화면 아래에서 Modal Bottom Sheet가 올라오게 된다!

    <br>

4. __모달 Bottom Sheet에서 클릭한 버튼이 어떤 버튼인지에 대한 정보를 MainActivity에 적용시키기__

    MainActivity에서 BottomSheetDialog 클래스의 함수를 가져다 사용할 수 있도록 BottomSheetDialog 클래스에 public interface를 하나 만들자.

    ![13](https://user-images.githubusercontent.com/31889335/80862988-14d92b00-8cb4-11ea-9b61-950a81b6810f.PNG)

    위와 같이 이름이 BottomSheetButtonClickListener인 public interface를 BottomSheetDialog 클래스 안에 만든다.

    이 interface 에는 onButtonClicked() 라는 함수를 정의해준다!

    > 인터페이스란?
    >
    > 인터페이스는 추상 클래스로 구현된 것은 없고 밑그림만 그려져 있는 '기본 설계도' 라고 할 수 있다.
    > 
    > 다형성을 구현하기 위해 사용된다.

    <br>


    이 onButtonClicked() 함수를 나중에 MainActivity에서 가져다 사용할 것이다.

    그 다음 작업을 하기 위해 알아두어야 할 것이 있다!

    현재 BottomSheetDialog 라는 클래스는 BottomSheetDialogFragment 라는 클래스를 상속하고 있으므로 Fragment 성질을 갖는다.

    프래그먼트는 액티비티 위에 올라갈 때 프래그먼트 life cycle에 의해 onAttach 라는 메소드가 자동으로 호출된다.

    따라서 우리는 MainActivity에서 위에서 만든 interface를 실제로 구현할 것이므로 Fragment의 onAttach 함수안에서 액티비티 객체를 변수에 할당해야 한다.

    따라서 BottomSheetDialog 클래스에서 다음과 같이 onAttached 함수를 오버라이드 해야한다.

    ![14](https://user-images.githubusercontent.com/31889335/80863048-65e91f00-8cb4-11ea-8238-0b574d416a62.PNG)

    ![15](https://user-images.githubusercontent.com/31889335/80863207-af863980-8cb5-11ea-83a8-4217bc8a4bca.PNG)

    그 다음, BottomSheetDialog 클래스의 onActivityCreated() 함수를 호출하고

    ![16](https://user-images.githubusercontent.com/31889335/80863243-e0666e80-8cb5-11ea-866a-5d3db54a0710.PNG)

    이와 같이 모달 화면의 두 개의 버튼을 클릭했을 경우 해야하는 작업을 작성해준다.

    이제 마지막으로 MainActivity에서 onButtonClicked() 함수를 오버라이드 하여 이 함수 안에서 수행해야 하는 작업을 작성해주면 된다.

    ![17](https://user-images.githubusercontent.com/31889335/80863306-40f5ab80-8cb6-11ea-83a8-1dad4622fca2.PNG)

    이 때, MainActivity 클래스에서 BottomSheetDialog 클래스에서 정의한 interface를 구현함을 명시해줘야 한다!

    여기까지 코드를 작성하고 프로젝트를 build 해보면 

    ![BottomSheet](https://user-images.githubusercontent.com/31889335/80863405-f7f22700-8cb6-11ea-9c10-577dc6e3aa31.gif)

    이와 같이 모달 Bottom Sheet 에서 클릭한 버튼에 따라 Main 화면의 text가 바뀌는 것을 볼 수 있다~!

    <br>









    

    








    
