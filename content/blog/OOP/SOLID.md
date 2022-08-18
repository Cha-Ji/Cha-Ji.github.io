---
title: '[객체지향] SOLID'
date: 2022-08-18 18:00:00
category: 'OOP'
draft: false
---

SOLID란 로버트 마틴이 명명한 객체지향 프로그래밍 및 설계의 다섯가지 기본 원칙을 말합니다.
이 글에서는 코드를 Kotlin 언어를 기준으로 작성합니다.

## 1. SRP: Single Responsibility Principle

> 단일 책임 원칙. 가장 기본이 되는 원칙이며 한 객체는 한 책임만 가져야 한다는 원칙입니다.

함수를 동사로 작성해보면 책임을 여러개 갖고 있는건 아닌지 생각해볼 수 있습니다.
동사 하나로 끝나지 않으면 여러 책임을 지고있는 것입니다.

고양이가 걸으면서 동시에 밥을 먹더라도 둘은 엄연히 다른 기능입니다.

```kotlin
class Cat {
    
    val leg = 4
    
    // 한꺼번에 호출
    fun walkAndEatAndPrintLeg() {
        print("뚜벅뚜벅")
        print("냠냠")
        print(leg)
    }
    
    // SRP를 고려 
    fun walk() {
        print("뚜벅뚜벅")
    }
    
    fun eat() {
        print("냠냠")
    }
    
    fun printLeg() {
        print(leg)
    }
}
```

고양이가 뚜벅뚜벅이 아닌 사뿐사뿐 걷는다면, SRP를 고려할 때에는 분명히 walk() 함수가 잘못된 것을 알 수 있습니다.
하지만 SRP를 고려하지 않고 먹을 때 입에서 사뿐사뿐 소리가 난다면 잘못된 줄도 모를 것입니다.

객체나 함수의 책임을 한 곳이 갖는다면 코드를 수정할 때 영향을 미치는 곳도 확실하고 에러가 발생했을 때 원인을 찾기도 수월해질 것입니다.

## 2. OCP: Open Closed Principle

> 개방 폐쇄 원칙. 확장에는 열려있고 수정에는 닫혀있어야 한다는 원칙입니다.

처음 이 원칙을 접했을 때 잘 설명할 수 없었습니다.
위 코드에서 Spider 클래스를 추가하겠습니다.
Cat과 Spider 중 다리가 더 많은 동물에게 간식을 줄 것입니다.

```kotlin
class Cat {
    
    val leg = 4
    
    fun eat() {
        print("냠냠")
    }
}

class Spider {
    
    val leg = 8
    
    fun eat() {
        print("슥슥")
    }
}

fun main() {
    val cat = Cat()
    val spider = Spider()
    
    game(cat, spider)
}

fun game(cat: Cat, spider: Spider) {
    if (cat.leg > spider.leg) cat.eat()
    else if (cat.leg < spider.leg) spider.eat()
    else return
}
```

패배한 cat 대신 dog가 참가한다면 코드가 어떻게 변할까요?
게임 함수가 변해야 합니다.

```kotlin
// fun game(cat: Cat, spider: Spider) {
fun game(dog: Dog, spider: Spider) {
    // 생략
}
```

참가자가 바뀌었는데 게임 자체가 변해버립니다. 이 행위는 cat을 dog로 **수정**한 행위입니다. 

그렇다면 **확장**하려면 어떻게 해야할까요?

```kotlin
interface Gamer {
    
}

fun game(gamer1: Gamer, gamer2: Gamer) {...}
```

위 예시에서는 인터페이스나 추상클래스처럼 상위 개념을 매개변수로 받아 확장을 고려할 수 있습니다.
요점은 기능을 추가할 때 기존 코드를 수정하는 것이 아닌 코드를 추가만 할 수 있도록 설계하는 원칙입니다.

## 3. LSP: Liskov Substitution Principle

> 리스코프 치환 원칙. 부모 객체를 호출하는 동작에서 자식 객체가 부모 객체를 완전히 대체할 수 있다는 원칙입니다.

객체의 상속이 일어날 때 자식 객체는 부모 객체의 특성을 그대로 갖습니다. 하지만 이 과정에서 무리하거나 의도와 어긋나는 확장으로 인해 잘못된 상속이 일어나기도 합니다.

유명한 예시로 직사각형과 정사각형이 있습니다.
정사각형은 직사각형이므로 상속받을 수 있다고 가정합니다.

```kotlin
// 직사각형
class Rect {
    val width: Int = 10
    val height: Int = 10
    
    fun getArea() = width * height
}

// 정사각형
class Square : Rect {
    
    fun setWidth(width: Int) {
        this.width = width
        this.height = width
    }
    
    fun setHeight(height: Int) {
        this.height = height
        this.width = height
    }
}
```

정사각형의 가로 또는 세로를 변경할 때 나머지 변도 길이를 함께 변경해야 합니다. 상속으로 간편하게 구현할 수 있습니다.
작성한 클래스를 메인 함수에서 활용해 보겠습니다.

```kotlin
fun main() {
    val rect = Rectangle()
    rect.setWidth(10)
    rect.setHeight(5)
    
    print(rect.getArea())
}
```

위 직사각형은 넓이 50을 출력할 것입니다.
리스코프 치환 원칙에 의하면 자식 객체는 부모 객체를 완전히 대체할 수 있습니다.

```kotlin
fun main() {
    val rect = Square()
    rect.setWidth(10)
    rect.setHeight(5)
    
    print(rect.getArea())
}
```

선언할 때에만 자식 클래스로 변경했습니다.
결과는 같을까요? 아닙니다. 아래 정사각형의 넓이는 5 * 5인 25가 나옵니다.

OCP의 예시에서 보였던 추상화, 이처럼 리스코프 치환 원칙에 맞지 않는다면 원치 않는 결과가 나올 수 있습니다.
직사각형과 정사각형이 "사각형"을 상속받으면 해결됩니다.

## 4. ISP: Interface Segregation Principle

> 인터페이스 분리 원칙. 특정 클라이언트를 위한 인터페이스 여러개가 범용 인터페이스 하나보다 낫다는 원칙입니다.

클라이언트는 사용하지 않는 인터페이스에 강제로 의존해서는 안됩니다.

걷는 동물도 있고 나는 동물도 있습니다.

```kotlin
interface Animal {

    fun fly() {}

    fun walk() {}
}

class Cat : Animal {

    override fun fly() {}

    override fun walk() { }
}
```

동물 인터페이스를 구현하는 Cat은 fly와 walk를 무조건 구현해야 합니다.
하지만 고양이는 날 수 없기 때문에 ISP를 위반하게 됩니다.

```kotlin
interface Flyable {
    fun fly()
}

interface Walkable {
    fun walk()
}

class Animal {}

class Cat : Animal, Walkable {
    fun walk() {}
}
```

위처럼 인터페이스를 분리해야할 의무가 있습니다.

## 5. DIP: Dependency Inversion Principle

> 의존 역전 원칙. 고수준 모듈은 저수준 모듈의 구현에 의존해서는 안된다.

앞서 살펴봤듯이 객체지향 설계는 일상적인 예시를 들어볼 수 있습니다.
여기서는 스마트폰 충전을 예시로 들겠습니다.
아이폰을 배제하고 안드로이드 폰만 생각해 보겠습니다.

과거 휴대전화에는 전용 충전기가 있었습니다. 전용 충전기로만 충전할 수 있다면 전용 충전기에 강한 의존성을 갖고 있다고 말할 수 있습니다. 
하지만 최근처럼 USB-C 타입 단자를 사용하는 스마트폰의 경우 전용 충전기가 아닌 다른 제조사의 충전기를 사용해도 잘 동작합니다.

이 경우 제조사의 충전기가 USB-C타입 이라는 인터페이스에 의존하게 됩니다.
기기가 충전기에 의존하던 것을 C타입 단자라는 인터페이스를 둬 의존성을 역전시켰다고 볼 수 있습니다.

### References

- https://blog.itcode.dev/posts/2021/08/15/liskov-subsitution-principle
- https://steady-coding.tistory.com/388
- 옥수환. _아키텍쳐를 알아야 앱이 보인다_
 