---
title: '[Compose] Phases'
date: 2021-11-03 16:21:13
category: 'Android'
draft: false
---

## 프레임의 3가지 단계

---

-   Composition → Layout → Drawing

### 1. Composition

-   Composable이 생성되는 단계입니다.
-   생성이 되면 다음 단계를 수행합니다.

### 2. Layout

-   measurement, placement 두 단계가 존재합니다.
-   재배치를 하더라도 재측정을 하지는 않습니다.

### 3. Drawing

-   Canvas를 호출하거나 draw관련 메서드를 호출하면 실행합니다.

## State 읽기 최적화

---

```kotlin
Box {
    val listState = rememberLazyListState()

    Image(
        // Non-optimal implementation!
        Modifier.offset(
            with(LocalDensity.current) {
                // State read of firstVisibleItemScrollOffset in composition
                (listState.firstVisibleItemScrollOffset / 2).toDp()
            }
        )
    )

    LazyColumn(state = listState)
}
```

-   위 코드에서는 State객체인 firstVisibleItemScrollOffset이 수시로 변경됩니다.
-   Box 내부에서 호출되고 있으므로 Box 자체가 재호출됩니다.

```kotlin
Box {
    val listState = rememberLazyListState()

    Image(
        Modifier.offset {
            // State read of firstVisibleItemScrollOffset in Layout
            IntOffset(x = 0, y = listState.firstVisibleItemScrollOffset / 2)
        }
    )

    LazyColumn(state = listState)
}
```

-   위 코드에서는 변경되는 값을 람다로 전달합니다.
-   Modifier에게 람다로 전달하는 블록은 레이아웃의 배치단계에서 호출됩니다.

> 그 밖에도 단계를 거스르는 방식의 호출 방법은 불필요한 반복을 야기시킬 수 있습니다.