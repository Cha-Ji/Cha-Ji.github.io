---
title: '[DI] Android에서의 DI 첫걸음'
date: 2021-11-04 16:21:13
category: 'Android'
draft: false
---

> DI는 Dependency Injection의 준말로, 의존성 주입을 뜻합니다.

## DI

Android에서 데이터를 관리할 때 ViewModel을 사용하곤 합니다.  
Repository 패턴도 함께 사용할 때가 많은데, 각설하고 코드로 예시를 보이겠습니다.

```kotlin

class ViewModel: ViewModel() {

    fun fetch() {
        repository.fetch()
    }
}
```

위 코드에서 ViewModel의 fetch 함수는 repository라는 변수가 필요합니다.  
이 코드를 실행시키려면 다양한 방법이 있습니다.

1.  내부에서 객체 생성  
    `private val repository = Repository()`
2.  외부에서 주입  
    `class ViewModel(private val repository: Repository): ViewModel()`
3.  외부에서 객체 가져오기  
    `fun fetch() { Factory.getRepository().fetch() }`

## 재사용

내부에서 객체를 생성한다면 재사용에 좋지 않습니다.

```kotlin
interface Repository {
    fun fetch()
}

class UserRepository : Repository
class AdminRepository : Repository
```

User, Admin이 각각 필요한 ViewModel을 만들어야 할 때,  
내부에서 객체를 생성한다면 각자의 ViewModel을 만들어야 합니다.

```kotlin

class ViewModel: ViewModel() {

    private val repository = UserRepository()

    fun fetch() {
        repository.fetch()
    }
}

class ViewModel2: ViewModel() {

    private val repository = AdminRepository()

    fun fetch() {
        repository.fetch()
    }
}

fun main() {
    val userViewModel = ViewModel()
    val adminViewModel = ViewModel2()
}
```

하지만 외부에서 주입한다면?

```kotlin

class ViewModel(private val repository: Repository): ViewModel() {

    fun fetch() {
        repository.fetch()
    }
}

fun main() {
    val userRepo: Repository = UserRepository()
    val adminRepo: Repository = AdminRepository()

    val userViewModel = ViewModel(userRepo)
    val adminViewModel = ViewModel(adminRepo)


}
```

인터페이스를 활용해 ViewModel을 재사용할 수 있습니다.

## 테스트

내부에서 객체를 생성한다면 테스트하기 어렵습니다.  
ViewModel을 테스트하고자 할 때 Repository와 독립적이라고 할 수 없습니다.  
Repository에 문제가 있다면 ViewModel.fetch()가 정상적으로 실행되지 않습니다.

하지만 외부에서 주입한다면 테스트 더블을 활용해 항상 성공하는 Repository, 항상 실패하는 Repository를 가정하여 테스트를 진행할 수 있습니다.

## 끝으로

Android에서 DI Library는 대표적으로 Dagger, Kolin, Hilt가 존재합니다.  
이 중 Dagger 기반의 Hilt 라이브러리를 권장하고 있습니다.