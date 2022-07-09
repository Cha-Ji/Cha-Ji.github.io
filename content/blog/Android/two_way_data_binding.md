---
title: '[Android] Two-way Data Binding'
date: 2000-07-03 16:21:13
category: 'Android'
draft: false
---
## Data Binding

-   먼저 데이터 바인딩에 대해 간략히 이야기하자면, xml위에서 코딩하는 방식입니다.
-   선언적 형식으로 앱의 데이터 소스와 UI의 구성요소를 결합합니다.

```kotlin
class ViewModel {
    var text = "123"
}
```

```xml
<variable
    name="vm"
    type="com.ex.viewModel"/>

<TextView
    android:text="@{vm.text}"/>
```

-   위와 같은 두 코드가 있을 때, Activity나 Fragment에서 따로 코드를 작성하지 않아도, viewModel에 있는 text 123을 xml의 TextView에 띄울 수 있습니다.

## Two-way Data binding

> UI에서 데이터 소스를 조작, 데이터 소스를 UI에 반영하는 두 방식을 동시에

-   xml에서 특정 함수를 실행시키거나 특정 반환값을 사용하는 것은 단방향 데이터 바인딩에서도 할 수 있습니다.
-   View의 변경사항을 ViewModel에 저장하고, 동시에 View에도 보이게 하려면 어떻게 해야할까요?
-   checkBox로 예시를 들자면, ViewModel은 항상 체크여부를 알아야하며 checkBox 역시 클릭될 때마다 체크박스가 반전됩니다.

```xml
<CheckBox
    android:checked="@{vm.checked}"
    android:onCheckChanged="@{vm.check}"/>
```

-   위 코드처럼 체크여부를 저장하고, 체크를 갱신해야합니다.
-   하지만 양방향 데이터 바인딩 방식을 사용한다면?

```xml
<CheckBox
    android:checked="@={vm.checked}"/>
```

-   체크할 때마다 데이터가 업데이트되며 데이터가 다시 View에 적용되기 까지 합니다.
-   양방향 데이터 바인딩 방식은 기존 방식의 `@` 기호 이후에 `=` 기호를 붙여주면 됩니다.

## 끝으로

-   양방향 데이터 바인딩은 말그대로 양쪽에서 조작할 수 있기 때문에 무한루프가 끝나지 않는 것을 조심해야 합니다.