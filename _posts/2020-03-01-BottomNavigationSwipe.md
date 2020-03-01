---
layout: post
title:  "[안드로이드] 🍏 Bottom Navigation Swipe되도록 만드는 나만의 라이브러리"
date:   2020-03-01 18:34:10 +0700
categories: [안드로이드]
---

> [여기](https://droidmentor.com/bottomnavigationview-with-viewpager-android/)를 참고하여 작성한 포스팅~

<br>

# 🍏 Bottom Navigation Swipe되도록 만드는 나만의 라이브러뤼~

일단 Bottom Navigation이 Swipe 된다는 것이 무슨 말일까?

![bottomNavigationWithViewPager](https://user-images.githubusercontent.com/31889335/75622205-3a2cb880-5be1-11ea-9fb6-fa2a74e0b1f6.gif)

이렇게 아래 탭을 직접 클릭해서도 화면이 바뀌고, 화면을 슬라이드해서도 탭간 이동이 가능한 뷰를 말한다!

그렇다면 이것을 만드려면 어떻게 구현해야할까?

__일단, Bottom Navigation Bar와 ViewPager를 만들고 이 둘을 연결해야 한다!__

Bottom Navigation Bar와 ViewPager 만드는 방법은 [Bottom Navigation Bar 만들기](https://choheeis.github.io/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C/2020/03/01/BottomNavigation.html) 와 [ViewPager 만들기](https://choheeis.github.io/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C/2020/03/01/ViewPager.html)를 보고 만들어보자.

나는 Bottom Navigation Bar와 ViewPager를 activity_main.xml 에 배치하였고,

~~~xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">


    <androidx.viewpager.widget.ViewPager
        android:id="@+id/viewPager"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toTopOf="@+id/bottomNavigationBar"
        android:layout_width="0dp"
        android:layout_height="0dp" />

    <com.google.android.material.bottomnavigation.BottomNavigationView
        android:id="@+id/bottomNavigationBar"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom"
        app:menu="@menu/bottom_navigation_items"/>

    
</androidx.constraintlayout.widget.ConstraintLayout>
~~~

이렇게 ConstraintLayout을 이용하여 BottomNavigationView 위에 ViewPager를 배치하였다!

[ViewPager 만들기](https://choheeis.github.io/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C/2020/03/01/ViewPager.html) 를 따라 만들면 MainActivity.kt 에 Adapter가 inner class로 들어가있는 형태로 만들어지는데 여기서 추가로 onCreate에 다음과 같은 2개의 리스너를 작성해야 한다.

~~~kotlin
override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        myPager = findViewById(R.id.viewPager)

        val pagerAdapter: PagerAdapter = ViewPagerAdpater(supportFragmentManager)
        myPager.adapter = pagerAdapter

        // 1. BottomNavigation에 각 탭을 클릭했을 때 반응하는 리스너 설정
        // --> BottomNavigation의 각 탭을 클릭하면 ViewPager로 화면이 슬라이드 되도록 하기 위함!
        bottomNavigationBar.setOnNavigationItemSelectedListener(
            object : BottomNavigationView.OnNavigationItemSelectedListener{
                override fun onNavigationItemSelected(item: MenuItem): Boolean {
                    when(item.itemId){
                        R.id.item_calls -> {
                            // ViewPager의 현재 item에 첫 번째 화면을 강제 대입
                            myPager.currentItem = 0
                            return true
                        }
                        R.id.item_chats -> {
                            // ViewPager의 현재 item에 두 번째 화면을 강제 대입
                            myPager.currentItem = 1
                            return true
                        }
                        R.id.item_contacts -> {
                            // ViewPager의 현재 item에 세 번째 화면을 강제 대입
                            myPager.currentItem = 2
                            return true
                        }
                        else -> {
                            return false
                        }
                    }
                }
            }
        )

        // 2. ViewPager의 page가 변하면 반응하는 리스너 설정
        // --> 슬라이드로 해당 page가 선택되면 BottomNavigation의 해당 탭을 활성화하기 위함!
        viewPager.addOnPageChangeListener(
            object : ViewPager.OnPageChangeListener{
                override fun onPageScrolled(
                    position: Int,
                    positionOffset: Float,
                    positionOffsetPixels: Int
                ) {

                }

                // 슬라이드로 해당 page가 선택되면 BottomNavigation의 해당 탭을 활성화하기
                override fun onPageSelected(position: Int) {
                    bottomNavigationBar.menu.getItem(position).isChecked = true
                }

                override fun onPageScrollStateChanged(state: Int) {
                }

            }
        )

    }
~~~

첫 번째 __setOnNavigationItemSelectedListener__ 는 Bottom Navigation Bar 입장에서 각 탭을 클릭했을 때 화면이 슬라이드 되어 넘어가도록 ViewPager를 조작하는 역할이다.

두 번째 __addOnPageChangeListener__ 는 ViewPager 입장에서 슬라이드를 통해 화면을 넘기면 해당 탭을 활성화하는 역할이다.

이렇게 하면 Bottom Navigation bar와 ViewPager를 연결 완료이다~!



