---
title: '[Android] LiveData'
date: 2000-07-02 16:21:13
category: 'Android'
draft: false
---

## LiveData

- LiveData는 관찰자 패턴을 활용한 Data Holder로 볼 수 있습니다.
- 간단하게 말해서 value를 set, post하는 과정을 Observe 합니다.
- MVVM을 예시로 들면, ViewModel에서 post한 데이터를 Activity에서 observe해 사용합니다.
- 추가로, postValue가 여러번 호출된다면 최신의 값이 적용되기 때문에 변경된 값을 읽지 못하는 경우가 생겨 setValue 방식을 권장합니다.

```kotlin
val liveData = MutableLiveData<String>("") 

val test = "" 
val setTest = "a" 

// observe 
liveData.observe(this, Observer{ test = it }) 
        
// set value 
liveData.value = setTest 
        
// post value 
liveData.postValue(setTest)
```

## 장점

-   LifeCycle의 변화를 인식하기 때문에 필요하지 않을 때 메모리를 절약하는 방법이 될 수 있습니다. (Destroy에 자동 메모리 해제)
-   또한 LifeCycle에 대응하지 못해 발생하는 오류도 없습니다. (사용자의 갑작스런 조작으로 인한 pause, destroy 등을 대응)

## 단점

- lifeCycle에 대응하기 때문에 Test를 진행할 때 Destroy 상태에도 관찰되도록 설정해줘야 합니다.
- java 코드를 가져다 쓰기 때문에 기본적으로 null이 허용되는 자료구조를 갖고 있고 kotlin에서는 null 예외처리가 필요합니다.
- 이에 대한 대안으로 flow를 사용하기도 합니다.