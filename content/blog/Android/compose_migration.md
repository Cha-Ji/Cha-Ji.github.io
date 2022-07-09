---
title: '[Compose] Compose로 migration하기'
date: 2021-11-03 16:21:13
category: 'Android'
draft: false
---

> 이 글은 RecyclerView로 구성된 한 화면을 Compose로 변경해보며 느낀점과  
> compose의 기본 개념을 서술합니다.

## Compose의 특징

---

-   적은 수의 코드
    -   손쉬운 유지보수
-   실시간 미리보기 기능
    -   빠른 개발시간
-   선언형 프로그래밍
    -   화면 전체를 개념적으로 재생성한 후 변경사항만 적용하는 방식을 적용할 수 있었습니다.

## 적용 이유와 후기

---

-   기존 코드를 migration하며 차이를 느껴보고 싶었습니다.
    -   RecyclerView를 작성하는 과정에서 코드 수가 크게 감소했다.
-   compose는 성능이 좋지 않다는 이야기가 있습니다.  
    성능 이슈는 타인의 말을 들어서는 크게 와닿지 않았습니다.  
    직접 비교하기 위해 기존 코드를 compose로 변경했습니다.
    -   의도한 대로 성능의 차이를 느끼지는 못했다.
    -   하지만 성능을 생각하며 개발하다보니 View의 재사용에 대해 고민했고,  
        compose에서는 변경된 사항만 똑똑하게 재사용하고 있기 때문에  
        첫 생성을 제외하고는 충분히 좋은 성능을 가진다.

## LifeCycle

---

compose는 아주 간단한 생명 주기를 갖습니다.

조금 더 복잡하게 사용하고 싶다면 Side effects를 학습해야합니다.

-   composition
-   recomposition
-   Leave the composition

### Composition

-   호출될 때마다 Composition에 여러 인스턴스가 배치됩니다.
-   여기서 State 객체가 변경되면 Recomposition이 트리거됩니다.

### Recomposition

-   Composable은 호출될 때 call site를 기억하며 식별자로 갖는다고 생각할 수 있습니다.
    -   call site는 소스코드의 위치를 말합니다.

```kotlin
@Composable
fun LoginScreen(showError: Boolean) {
    if (showError) {
        LoginError()
    }
    LoginInput() // This call site affects where LoginInput is placed in Composition
}

@Composable
fun LoginInput() { /* ... */ }
```

-   showError를 변경시켜 Recomposition이 진행되면 관련된 LoginError만 새 인스턴스가 배치됩니다.

> 반복문을 사용하면 코드 위치가 같은데 다 똑같은 Composable인가?

-   이럴 때에는 인스턴스를 구분하기 위해 call site 외에 실행 순서가 적용되고 재사용이 되지 않습니다.
-   key 값을 지정해 인스턴스를 구분시켜준다면 재사용이 가능하게 됩니다.

> 그래서 어떻게 똑똑한 재활용을 하는거지?

-   먼저 Stable한 Composable은 안정적입니다. primitive한 속성은 모두 Stable합니다.
-   Stable하고 변경되지 않았다면 recomposition을 건너뛸 수 있습니다.
-   Stable하지 않다면 변경이 되었는지 equals를 확인합니다.
-   안정적인 것으로 간주하기 위해 @Stable 어노테이션을 사용할 수 있습니다.

## Resources

---

- [https://developer.android.com/jetpack/compose](https://developer.android.com/jetpack/compose/lifecycle)