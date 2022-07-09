---
title: '[Android] MVVM 패턴'
date: 2000-07-01 16:21:13
category: 'Android'
draft: false
---
# MV? 패턴

-   디자인 패턴은 코드의 수정과 이해를 돕습니다.
-   의존성을 덜어내 테스트도 수월해지게 됩니다.
-   안드로이드에서 MV? 패턴에는 MVC - MVP - MVVM - MVI 패턴 등 여러가지가 존재합니다.
-   그 중 MVVM 패턴은 Model, View, ViewModel로 나뉘어져 있습니다.

## View

-   MVVM패턴에서 Activity, Fragment와 같이 UI 로직을 다루는 구성요소를 View라고 부릅니다.
-   View에 포함된 비즈니스 로직과 View와 Model과의 의존성은 MV? 패턴의 고민입니다.
-   해당 고민들을 해결해 나가며 여러가지 패턴이 등장했고, ViewModel 역시 View의 프레젠테이션 로직을 담당합니다.
-   본문에는 서술하지 않지만 LiveData로 Model과 View의 의존성을 덜어줄 수 있습니다.

## Model

-   Model은 데이터와 관련된 모든 행위를 담당합니다.
-   비즈니스 로직을 포함하고 있습니다.

## ViewModel

-   Model의 데이터를 가공해 View에 전달합니다.
-   binder를 갖고 View와 View 속성을 자동으로 연결합니다.
-   AAC ViewModel은 lifecycle을 알고있는 객체를 생성할 때 사용하며 ViewModel과 동일하게 이해해선 안됩니다.