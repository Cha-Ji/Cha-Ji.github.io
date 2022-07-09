---
title: '[Compose] Side Effects'
date: 2021-11-03 16:21:13
category: 'Android'
draft: false
---

## Side effects

---

일반적인 수명주기에서 더 복잡한 작업을 수행할 때 Side Effects를 필요로 합니다.

### LaunchedEffect

> Composable 내에서 suspend 함수를 호출할 때 사용

-   매개변수 중 하나가 변경되면 재호출
-   ex) 스낵바 호출, 스크롤 등의 일반적인 동작

### rememberCoroutineScope

> Composable 외부에서 코루틴 실행할 때 사용

-   ex) 다른 composable을 취소시키는 비동기 동작

### rememberUpdatedState

> 값이 변경되더라도 recomposition되지 않는 효과에서 값 참조

```
@Composable
fun LandingScreen(onTimeout: () -> Unit) {

    // This will always refer to the latest onTimeout function that
    // LandingScreen was recomposed with
    val currentOnTimeout by rememberUpdatedState(onTimeout)

    // Create an effect that matches the lifecycle of LandingScreen.
    // If LandingScreen recomposes, the delay shouldn't start again.
    LaunchedEffect(true) {
        delay(SplashWaitTimeMillis)
        currentOnTimeout()
    }

    /* Landing screen content */
}
```

-   LaunchedEffect를 재사용하지 않기 위해 onTimeout을 기억

### DisposableEffect

Composition을 종료한 뒤 clear해야하는 부수 효과에 적용합니다.

```
DisposableEffect(lifecycleOwner) {
        // Create an observer that triggers our remembered callbacks
        // for sending analytics events
        val observer = LifecycleEventObserver { _, event ->
            if (event == Lifecycle.Event.ON_START) {
                currentOnStart()
            } else if (event == Lifecycle.Event.ON_STOP) {
                currentOnStop()
            }
        }

        // Add the observer to the lifecycle
        lifecycleOwner.lifecycle.addObserver(observer)

        // When the effect leaves the Composition, remove the observer
        onDispose {
            lifecycleOwner.lifecycle.removeObserver(observer)
        }
    }
```

-   lifecyclerOwner가 변경되면 DisposableEffect 절을 재호출합니다.
-   onDispose절을 포함시키지 않으면 오류가 발생합니다.

### SideEffect

> Compose State를 비 Compose 코드에서 구독

-   비 Compose와 혼용할 때 사용합니다.
-   recomposition 될 때마다 호출합니다.
-   SideEffect 절에서 일반함수를 호출하려면 SideEffect 블럭 안에서 호출하면 됩니다.

### produceState

> 비 Compose 를 Compose State로 변환

-   비 Compose의 속성을 Compose의 State로 사용할 수 있습니다.

```
@Composable
fun loadNetworkImage(
    url: String,
    imageRepository: ImageRepository
): State<Result<Image>> {

    // Creates a State<T> with Result.Loading as initial value
    // If either `url` or `imageRepository` changes, the running producer
    // will cancel and will be re-launched with the new keys.
    return produceState(initialValue = Result.Loading, url, imageRepository) {

        // In a coroutine, can make suspend calls
        val image = imageRepository.load(url)

        // Update State with either an Error or Success result.
        // This will trigger a recomposition where this State is read
        value = if (image == null) {
            Result.Error
        } else {
            Result.Success(image)
        }
    }
}
```

-   return 값이 있는 Composable은 소문자로 시작합니다.

### derivedStateOf

> State 객체를 다른 State 객체에서 사용할 때

```
    val todoTasks = remember { mutableStateListOf<String>() }

    // Calculate high priority tasks only when the todoTasks or
    // highPriorityKeywords change, not on every recomposition
    val highPriorityTasks by remember(todoTasks, highPriorityKeywords) {
        derivedStateOf {
            todoTasks.filter { it.containsWord(highPriorityKeywords) }
        }
    }
```

### snapshotFlow

> State를 Flow로 변환

-   snapshotFlow가 collect될 때 State 객체의 결과를 내보냅니다.
-   State 객체가 변화되면 다시 값을 내보냅니다.

## Resources

---

- [https://developer.android.com/jetpack/compose](https://developer.android.com/jetpack/compose)