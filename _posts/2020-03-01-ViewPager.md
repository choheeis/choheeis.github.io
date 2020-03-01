---
layout: post
title:  "[안드로이드] 🍎 ViewPager 만드는 나만의 라이브러리"
date:   2020-03-01 18:34:10 +0700
categories: [안드로이드]
---

> [ViewPager 공식 도큐먼트](https://developer.android.com/training/animation/screen-slide?hl=ko)를 참고하여 공부한 내용!

# 🍎 ViewPager 만드는 나만의 라이브러뤼~

ViewPager에 대해서 찾아보니 [ViewPager 공식 도큐먼트](https://developer.android.com/training/animation/screen-slide?hl=ko) 가 있어서 이걸 보고 따라해보았다!

ViewPager는 화면 간의 슬라이드를 할 수 있게 해주는 것이고 android.support 라이브러리에서 지원하는 뷰이다.

__그렇기 때문에 프로젝트에 android.support 라이브러리를 implement 해야한다.__

android.support 라이브러리를 프로젝트에 추가하는 방법은 [Bottom Navigation Bar 만드는 법에 대한 포스팅](https://choheeis.github.io/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C/2020/03/01/BottomNavigation.html)을 보고 따라하면 된다!

그 다음으로는 ViewPager로 넘겨질 화면들의 xml를 작성해준다.

각 화면에는 각 화면을 구별할 수 있도록 텍스트뷰를 배치하였고 각각 다른 텍스트를 넣어놓았다!

![01](https://user-images.githubusercontent.com/31889335/75620476-d5fff980-5bcc-11ea-8242-30cfafd45e47.PNG)

요렇게 3개의 xml을 만들었다.

이렇게 각 화면의 xml을 만들었다면 이 xml들을 반환하는 Fragment 클래스를 각각 만들어줘야한다!

![02](https://user-images.githubusercontent.com/31889335/75620517-1a8b9500-5bcd-11ea-8467-9eeef303a09c.PNG)

이렇게 새로운 코틀린 클래스를 생성한 후, 이 클래스가 Fragment() 클래스를 상속받도록 설정해준다.

![03](https://user-images.githubusercontent.com/31889335/75620551-80781c80-5bcd-11ea-8b53-dcf0c9293775.PNG)

그 다음 onCreateView 함수를 오버라이드 해준 후 다음과 같이 inflate시켜 반환할 layout의 이름을 세팅해준다.

![04](https://user-images.githubusercontent.com/31889335/75620574-b7e6c900-5bcd-11ea-8106-0b6d38d5abcc.PNG)

위와 같은 클래스를 Fragment2, Fragment3 이라는 이름의 클래스로 2개 더 만들어준다.

이제 ViewPager가 동작할 화면에 ViewPager를 추가시켜 주어야 한다.

ViewPager는 페이지 전환을 위한 스와이프 동작이 내장되어 있어 기본적으로 슬라이드 애니메이션을 지원하므로 개발자가 자체 슬라이드 애니메이션을 만들 필요가 없다.

또, ViewPater는 새 페이지를 표시해주는 요소로서 PagerAdapter를 사용하기 때문에 PagerAdapter만 만들어 주면 된다.

일단 ViewPager가 동작할 화면에 ViewPager를 추가시켜줘보자.

나는 메인 화면에서 화면 슬라이드가 되도록 할 것이기 때문에 아래와 같은 코드를 activity_main.xml 에 배치하였다.

![05](https://user-images.githubusercontent.com/31889335/75620644-86223200-5bce-11ea-9b12-e675117e3307.PNG)

그 다음 MainAcitivity.kt 를 ViewPager Adater 클래스를 inner 클래스로 갖도록 만들어줘야 한다.

~~~kotlin
package com.example.viewpager

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentManager
import androidx.fragment.app.FragmentStatePagerAdapter
import androidx.viewpager.widget.PagerAdapter
import androidx.viewpager.widget.ViewPager

// 1. 슬라이드에 참여하는 화면 수를 전역변수로 설정
private const val NUM_PAGES = 3

class MainActivity : AppCompatActivity() {

    // 2. ViewPager 변수를 나중에(onCreate함수 내에서) 초기화하겠다고 선언
    private lateinit var myPager: ViewPager

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // 3. myPager 변수에 xml에 작성한 viewPager 대입
        myPager = findViewById(R.id.viewPager)

        // 5. pagerAdapter 변수에 Adapter 클래스 대입
        val pagerAdapter: PagerAdapter = ViewPagerAdapter(supportFragmentManager)
        myPager.adapter = pagerAdapter

    }

    // 6. 뒤로가기 버튼 클릭시 해줄 것들 추가 세팅
    override fun onBackPressed() {
        if(myPager.currentItem == 0){
            // 만약 유저가 슬라이드 화면 중 제일 첫 화면을 보고 있을 경우에 Back버튼을 누르면 어플이 finish 됨
            super.onBackPressed()
        }else{
            // 아닐경우 Back 버튼 누르면 이전 화면으로 슬라이드되면서 이동
            myPager.currentItem = myPager.currentItem - 1
        }
    }


    // 4. ViewPager가 참조할 ViewPagerAdapter 클래스를 inner 클래스로 생성
    private inner class ViewPagerAdapter(fm: FragmentManager) : FragmentStatePagerAdapter(fm){
        override fun getItem(position: Int): Fragment {
            // 슬라이드 position 마다 어떤 화면을 띄워줄지를 결정
            return when (position) {
                0 -> return Fragment1()
                1 -> return Fragment2()
                2 -> return Fragment3()
                else -> Fragment1()
            }
        }
        override fun getCount(): Int = NUM_PAGES

    }
}

~~~


이렇게 만들어준다!

그럼 이렇게 결과적으로 3개의 화면이 슬라이드되는 ViewPager를 볼 수 있다!

![viewpager](https://user-images.githubusercontent.com/31889335/75621057-3b56e900-5bd3-11ea-8224-516ebafa85c2.gif)

<br>

# 🍎 ViewPager 만드는 순서 정리

### 1. android.support 라이브러리 implement 하기

### 2. 각 화면의 xml 작성하기 (뷰만들기)

### 3. 각 화면을 반환하는 Fragment 클래스 만들기

### 4. ViewPagerAdapter를 만들어 연결해주기

<br>

