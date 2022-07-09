---
title: '[Android] ViewModel'
date: 2000-07-02 16:21:13
category: 'Android'
draft: false
---
## ViewModel

viewModel은 Android Jetpack의 구성요소이며 UI 관련 데이터를 관리합니다.

MVC 패턴에서 Controller에 과도한 책임이 할당되는 문제를 어느정도 해결해줍니다.

## 특징

-   UI 컨트롤러의 수명 주기를 관리합니다.
-   컨트롤러 로직에서 뷰 데이터 소유권을 분리하는 목적으로 사용합니다.
-   클래스를 분리하기 때문에 다른 화면에서 재사용이 가능합니다.

## 구현

- 아주 간략한 코드입니다.

```kotlin
class TempViewModel : ViewModel {
    private val _temp = MutableLiveData<Item> by lazy {
        MutableLiveData<Item>().also {
            test()
        }
    }
    
    val temp: LiveData<Item> = _temp

    fun test() { }
}
```

```kotlin
class MainActivity : AppcompatActivity {

    val model: TempViewModel by viewModels()
    
    override fun onCreate() {
        model.temp.observe(this, Observer<Item> {

        })    
    }
}
```

-   ViewModel 클래스를 생성해 관찰할 LiveData에 관한 함수를 작성합니다.
-   Activity, Fragment에서는 by viewModels() 키워드를 사용해 선언할 수 있고, 관찰하거나 작성한 함수를 사용합니다.

### References
- [Android 공식 사이트](https://developer.android.com/topic/libraries/architecture/viewmodel?hl=ko)