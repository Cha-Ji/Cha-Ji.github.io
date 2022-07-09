---
title: '[Android] Single Live Event & Event wrapper'
date: 2021-11-04 16:21:13
category: 'Android'
draft: false
---

# Single Live Event & Event wrapper

> Data를 다루는 LiveData객체를 활용해 이벤트 발생을 처리하는 방법.

## 간단한 구현

-   특정 이벤트가 발생할 때마다 실행시켜야 하는 함수가 있다면 다음과 같이 구현할 수 있습니다.

```kotlin
class ExampleViewModel: ViewModel() {

    private var _event: MutableLiveData<Unit> = MutableLiveData()
    val event: LiveData<Unit> = _event

    fun callEvent() {
        _event.setValue(Unit)
    }
}

class ExampleActivity: Activity() {
    fun main() {
        button.ClickListener {
            viewModel.callEvent()
        }

        viewModel.event.observe {
            showClickCount()
        }
    }
}
```

1.  button이 클릭될 때마다 event객체에 Unit값을 set 해줍니다.
2.  event객체를 observe해서 위 이벤트가 발생할 때마다 함수를 실행시킵니다.

## 문제점

-   위처럼 구현하면 문제가 발생합니다.
-   화면 회전과 같이 생명주기가 바뀌는 상황에서 이벤트가 발행된다는 점입니다.
-   클릭하지 않았는데 클릭 횟수가 오르는 상황이 생길 수 있습니다.
-   이 문제들은 LiveData가 InActive한 상태에서 Active한 상태로 변경되기 때문에 콜백이 호출되어 발생합니다.

## Single Live Event

-   다음은 [아키텍처 샘플](https://github.com/android/architecture-samples) 에서 구현한 방식을 축소한 코드입니다.

```kotlin
class SingleLiveEvent<T> : MutableLiveData<T>() {

    private val pending = AtomicBoolean(false)

    @MainThread
    override fun observe(owner: LifecycleOwner, observer: Observer<T>) {

        // Observe the internal MutableLiveData
        super.observe(owner, Observer<T> { t ->
            if (pending.compareAndSet(true, false)) {
                observer.onChanged(t)
            }
        })
    }

    @MainThread
    override fun setValue(t: T?) {
        pending.set(true)
        super.setValue(t)
    }

    @MainThread
    fun call() {
        value = null
    }
}
```

-   위처럼 구현하면 불필요한 이벤트가 발생하진 않습니다.
-   하지만 아직 문제는 남아있습니다.
-   Single이라는 이름에 걸맞게 이벤트 발생을 여러 곳에서 구독할 수 없고 구독자 중 한 명만 소식을 받아볼 수 있습니다.

## Event Wrapper

-   다음 역시 [아키텍처 샘플](https://github.com/android/architecture-samples) 에서 구현한 방식을 축소한 코드입니다.

```kotlin
open class Event<out T>(private val content: T) {

    var hasBeenHandled = false

    fun getContentIfNotHandled(): T? {
        return if (hasBeenHandled) {
            null
        } else {
            hasBeenHandled = true
            content
        }
    }

    fun peekContent(): T = content
}
```

-   get Content If Not Handled 함수를 Single Live Event처럼 사용할 수 있습니다.
-   peekContent 함수를 이벤트 처리 여부와 관계 없이 사용할 수 있습니다.

## 끝으로

-   이외에도 coroutine flow의 StateFlow, SharedFlow로 이벤트를 발행할 수 있습니다.