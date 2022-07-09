---
title: '[Android] 예외처리: run catching'
date: 2000-07-02 16:21:13
category: 'Android'
draft: false
---
# kotlin.runCatching

-   예외처리는 흔하게 try / catch문을 사용한다고 알고 있습니다.

```kotlin
fun test(): String? {
    val result
    try {
        result = getStr()
        return result
    }catch(e) {
        e.printStackTrace()
        return null
    }
}
```

-   개발자라면 가독성이 좋은 코드를 쫓기 마련입니다.

```kotlin
fun test(): String? = kotlin.runCatching {
    getStr()    
}
    .onFailure { e -> e.printStackTrace() }
    .getOrDefault(null)
```

-   저는 runCatching문이 더 예뻐보이지만 가독성은 집단마다 차이가 있기 때문에 선호하는대로 사용하면 될 것 같네요!