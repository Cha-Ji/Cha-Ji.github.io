---
title: '[Kotlin] LiveData -> Coroutine Flow'
date: 2021-11-06 16:21:13
category: 'Android'
draft: false
---

MVVM 패턴을 적용하다보면 View와 ViewModel의 관심사 분리를 위해 노력하곤 합니다.  
그 중 이벤트 처리와 데이터를 다루는 객체로 LiveData를 사용했습니다.  
기존에 사용하던 LiveData에서 Coroutine Flow로 마이그레이션하며 개념을 정리하려 합니다.

## 짧은 사전 설명

xml에서 버튼을 클릭하면 Fragment에서는 원하는 화면으로 전환합니다.  
해당 동작을 dataBinding을 사용해 아래처럼 구현했습니다.

1.  Fragment에선 viewModel의 flow를 구독합니다.
2.  flow 데이터의 값을 수신하면 원하는 화면으로 전환합니다.
3.  xml에서 viewModel의 emit 함수를 실행시킵니다.
4.  viewModel에선 flow 값을 발행시킵니다.

## LiveData -> Flow

Domain Layer에서 반환값을 liveData로 사용하면 Android 패키지를 import하게 됩니다.  
하지만 Domain Layer는 kotlin으로만 이루어져 있어 Coroutine Flow를 사용하게 되었습니다.

## SharedFlow, StateFlow?

LiveData는 StateFlow와 유사하다는 얘기를 듣고 SharedFlow와 StateFlow의 차이가 궁금해 정리하게 되었습니다.

## Hot Stream (SharedFlow)

Hot Stream은 방송국과 같습니다.  
구독자가 없어도 값을 계속 방출한다는 특징이 있으며, 모든 구독자는 같은 데이터 스트림을 공유합니다.  
이는 pub-sub 패턴과 유사하다고 볼 수 있습니다.

```kotlin

fun testSharedFlow(data: Flow<String>) {
    val sharedFlow: SharedFlow<String> = data.sharedIn(
        scope = viewModelScope,
        started = SharingStarted.Eagerly,
        replay = 0
    )
}
```

일반 flow에 sharedIn 메서드를 붙여 SharedFlow로 사용할 수 있습니다.  
sharedIn 메서드에는 매개변수가 세개 필요합니다.

1.  어떤 코루틴 스코프에서 작동할지
2.  데이터 방출 시작방식
3.  각 새 collector로 재생할 항목의 수

여기서 2번째인 started 매개변수의 설명을 더하자면 세가지 방식이 존재합니다.

-   첫 구독자가 나타날 때 방출하는 Lazily 방식
-   선언과 함께 방출하는 Eagerly 방식
-   구독자가 존재하는 동안만 방출하는 등의 WhileSubscribed 방식

해당 SharingStarted 객체는 Cold Stream에서도 매개변수로 활용하게 됩니다.

## Cold Stream (StateFlow)

Cold Stream은 Lazy합니다. LiveData의 쓰임새를 그대로 사용하려면 StateFlow를 사용하게 됩니다.  
구독자가 생길 때마다 새 데이터 스트림을 생성한다는 특징이 있으며, 구독자가 생겨야 값을 방출하기 시작합니다.  
이는 observer 패턴과 유사하다고 볼 수 있습니다.

```kotlin

fun testStateFlow(data: Flow<String>) {
    val stateFlowExample: StateFlow<String> = data.stateIn(
        scope = viewModelScope, 
        started = SharingStarted.Eagerly,
        initialValue = ""
    )
}
```

일반 flow에 stateIn 메서드를 붙여 StateFlow로 사용할 수 있습니다.  
stateIn 메서드에는 매개변수가 세개 필요합니다.

1.  어떤 코루틴 스코프에서 작동할지
2.  데이터 방출 시작방식
3.  디폴트값

## References

-   [https://myungpyo.medium.com/콜드-플로우-효율적으로-사용하기-437a5edcd598](https://myungpyo.medium.com/%EC%BD%9C%EB%93%9C-%ED%94%8C%EB%A1%9C%EC%9A%B0-%ED%9A%A8%EC%9C%A8%EC%A0%81%EC%9C%BC%EB%A1%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-437a5edcd598)