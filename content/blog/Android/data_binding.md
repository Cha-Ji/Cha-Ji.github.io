---
title: '[Android] Data Binding'
date: 2000-07-03 16:21:13
category: 'Android'
draft: false
---

# 데이터 바인딩

-   이전에 포스팅했던 View binding과 같이 뷰 속성을 참조하는 방식 중 하나입니다.
-   View binding보다 성능은 떨어질 수 있습니다.

## 사용 전

-   build.gradle에서 android 블록에 `dataBindinbg { enabled = true }` 를 추가해야 합니다.
-   plugin 블록에 \`id 'kotlin-kapt' 를 추가해야 합니다.
-   컴파일될 때 바인딩 클래스가 자동으로 구현됩니다.
-   layout.xml에선 최상위에 layout 블록이 놓여집니다.
-   그리고 data 블록 안에 바인딩할 데이터를 작성합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools">

    <data>
        <variable
                name="text"
                type="com.afsdf.asdf.TextSample"/>
    </data>

    <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent">
        <TextView
            android:id="@+id/textView_sample"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{text}"/>
    </LinearLayout>
</layout>
```

## 사용

-   Activity, Fragment에서 binding 변수를 만듭니다.
-   view에 해당하는 xml파일을 매칭시켜 주면서 binding 변수에 할당합니다. `DataBindingUtil.setContentView(this, R.layout.activity_main)`
-   View Binding을 사용할 때와 마찬가지로 Activity, Fragment 파일에서 binding.textViewSample을 참조할 수 있습니다.

## MV?에서의 적용

-   MVC 패턴에서 데이터 바인딩을 적용하게 되면, 화면은 Model을 참조하게 되고 의존하게 됩니다.
-   MVVM 패턴의 ViewModel을 따로 만들어 레이아웃과 위젯을 ViewModel이 바인딩하게 합니다.