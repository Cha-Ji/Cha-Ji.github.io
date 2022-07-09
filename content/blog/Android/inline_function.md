---
title: '[Kotlin] inline function'
date: 2021-11-05 16:21:13
category: 'Android'
draft: false
---

# inline

-   inline 키워드는 함수 선언 키워드인 fun 앞에 붙습니다.
-   inline 키워드는 함수 구현 자체를 코드에 넣어 오버헤드를 줄이는 키워드입니다.

## 코드 예시

### 고차함수

```kotlin
fun setOnClickListener(onClick: () -> Unit) {
    onClick()
}

fun main() {
    setOnClickListener(print("clicked"))
}
```

해당 코드를 자바코드로 디컴파일하면 아래처럼 동작합니다.  
(가독성을 위해 코틀린 코드로 동작원리만 비슷하게 작성했습니다.)

```kotlin
fun main() {
    setOnclickListner(
        fun invoke() {
            print("clicked")
        }
    )
}
```

### inline 키워드 사용

같은 코드를 inline으로 작성하면 아래와 같습니다.

```kotlin
inline fun setOnClickListner(onClick: () -> Unit) {
    onClick()
}

fun main() {
    setOnclickListner { print("clicked") }
}
```

해당 코드는 아래처럼 동작합니다.

```kotlin
fun main() {
    print("clicked")
}
```

-   힙에 저장된 함수를 호출하는 것이 아닌, 함수에 있는 코드를 그대로 가져오는 것을 inline 키워드가 맡습니다.
-   불필요한 과정을 생략하게 되므로 성능의 이점을 가져올 수 있습니다.

## inline은 만능?

단지 키워드를 붙이는 것만으로 성능의 이점을 가져온다면, 모든 함수는 inline이 붙어야 합니다.  
inline 키워드는 코드를 그대로 붙여넣는 키워드이기 때문에 바이트코드가 늘어난다는 단점이 있습니다.

## 추가 키워드

### noinline

앞서 언급했듯이 inline에게도 단점이 있습니다.  
noinline 키워드를 매개변수에 붙여 선택적으로 inline 키워드를 활용할 수도 있습니다.

```kotlin
inline fun setOnClickListner(onClick: () -> Unit, noinline onDoubleClick: () -> Unit) {

}
```

처럼 사용한다면 noinline 키워드가 붙은 매개변수는 일반 매개변수처럼 동작합니다.

### crossline

inline 키워드가 속한 함수가 아닌 다른 곳에서 매개변수를 소비하고 싶을 때 사용한다.  
전달받은 매개변수를 다른 dispatcher에서 사용하고자 할 때를 예로 들 수 있다.

```kotlin
inline fun setOnClickListener(crossline onClick: () -> Unit) {
    coroutineScope.launch {
        onClick()
    }
}
```