---
title: '[Android] Collection'
date: 2000-07-02 16:21:13
category: 'Android'
draft: false
---

### Collection 함수의 생성

-   Collection에는 listOf, setOf, mapOf와 같은 함수가 존재합니다.
-   ```kotlin
    val array = listOf(1, 2, 3)
    ```

-   여기서 선언한 array의 원소는 변경할 수 없고, 변경하려면 mutable한 collection 함수로 생성해야 합니다.

### Mutable vs Immutable

직역하면 변할 수 있는, 변할 수 없는 이라는 뜻입니다.

Kotlin에서 변수를 선언할 때 var, val 형으로 선언하곤 합니다.

val로 지정된 변수는 immutable하며 초기화만 가능합니다.

var로 지정된 변수는 mutable하며 값을 바꿀 수 있습니다.

또한, Collection 함수는 대체로 immutable하며 가변적으로 만들고 싶다면 mutable한 collection 함수로 선언해야 합니다.

```kotlin
var array = mutableListOf(1, 2, 3)
```

-   여기서 선언한 array는 원소를 변경할 수 있습니다.

### for문과 array

배열을 선언했다면 모든 원소를 탐색할 수 있어야 합니다.

Kotlin은 함수형 프로그래밍을 좋아하기 때문에 Collection 함의 원소를 탐색하는 여러 함수가 존재하지만,

반복문을 사용해도 첫 원소부터 마지막 원소까지 훑을 수 있습니다.

저는 코딩테스트를 연습하며 Python 언어에 익숙했고, Kotlin에서의 for문은 생소했습니다.

Kotlin에서의 for문은 in을 주로 사용합니다.

```kotlin
// i는 0부터 array의 size까지
for (i in 0 until array.size){
    println(array[i])
}

for (i in array.indices){
    println(array[i])
}
```

-   두 for문은 array의 원소를 처음부터 끝까지 훑는 반복문입니다.

### when

배열을 순회하는 목적 중 하나는 원하는 원소를 찾고 싶을 때 입니다.

제어문을 사용하면 원하는 조건에 부합할 때만 작동하는 명령을 만들 수 있습니다.

Kotlin의 제어문에는 대표적으로 if문과 when문이 있습니다.

if문을 여러개 사용할 때 간단하게 when 문으로 대체할 수 있습니다.

```kotlin
if (a == 1) print(1)
else if(a == 2) print(3)
else if(a == 3) print(8)


when{
    a == 1 -> print(1)
    a == 2 -> print(3)
  a == 3 -> print(8)
}
```

-   위 if 문과 when 문은 똑같이 작동합니다. 정답은 없지만 저에게는 when문이 더 가독성이 좋게 느껴지네요ㅎ

## 2\. 제네릭

---

> "제네릭 프로그래밍은 데이터 형식에 의존하지 않고 하나의 값이 여러 다른 데이터 타입들을 가질 수 있는 기술에 중점을 두어 재사용성을 높일 수 있는 프로그래밍이다." -[https://ko.wikipedia.org/wiki/제네릭\_프로그래밍](https://ko.wikipedia.org/wiki/%EC%A0%9C%EB%84%A4%EB%A6%AD_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)

### 왜 쓸까?

코틀린에서는 Int를 매개변수로 받는 함수가 있으면

그 함수에 String을 넘겨줄 때 오류가 발생합니다.

일반 자료형은 toInt()와 toString()과 같은 함수를 사용해 타입 캐스팅을 해주면 되지만,

배열의 자료형을 변경하려면 모든 원소에 함수를 사용해줘야 하며, List로 선언된 배열에 string을 넣어줄 수 없어 새 배열을 만들어야 합니다.

### 사용

함수를 선언할 때 fun 바로 뒤에 <T: Comparable>를 붙여주고

자료형에 T를 사용합니다.

```kotlin
fun <T: Comparable<T>>walk(leg: List<T>){ 
      print(leg)
  }
```

-   위와 같이 함수를 만들면 leg에 {'left leg', 'right leg'}가 들어와도 {2, 4, 6, 8}이 들어와도 자료형에 구애받지 않고 에러 없이 출력할 수 있습니다.

### 주의할 점

-   객체를 생성할 때에는 제네릭 타입을 사용할 수 없습니다.
-   자료형을 확정짓지 않는만큼 코드의 가독성이 떨어질 수 있습니다.

따로 자료형을 지정해주지 않아도 되는 편리함을 가졌지만,

코드를 볼 때 자료형을 확인할 수 없고 복잡하다는 단점도 생각하면서 써야겠습니다.