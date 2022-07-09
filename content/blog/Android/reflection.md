---
title: '[Java] Reflection'
date: 2021-11-06 16:21:13
category: 'Android'
draft: false
---

## 리플렉션

-   객체를 통해 클래스의 정보를 분석하는 기법입니다.

### Class.class

```
Instances of the class Class represent classes and interfaces
 in a running Java application.  Every array also belongs to a class that is
 reflected as a Class object that is shared by all arrays with
 the same element type and number of dimensions.
```

-   ⇒ Class 클래스의 객체는 자바 프로그램에서 사용되는 클래스들과 인터페이스들을 나타낸다.
-   사용 가능한 클래스의 인스턴스가 없을 때 .class를 사용하곤 합니다.
-   인스턴스가 있다면 getClass() 메서드로 얻을 수 있습니다.
-   코드 _리플렉션_에 사용됩니다.
-   정규화 된 클래스 이름, 상수 목록, 공용 필드 목록 등과 같은 클래스의 메타 데이터를 수집 할 수 있습니다.

### 실사용

```kotlin
Method[] methods = testClass.getMethods();
//getMethod("메서드", 파라미터);
Method method = testClass.getMethod("testMethod");
Method method = testClass.getMethod("testMethod", null);
Method method = testClass.getMethod("testMethod", String.class);
Method method = testClass.getMethod("testMethod", new Class[]{String.class, Integer.class});
```

-   리플렉션을 사용하면 외부에서 클래스의 private 메서드도 꺼내볼 수 있습니다.

### References

[\[Java\] Class.class, Class 클래스.](https://devyongsik.tistory.com/292)

[\[Java\] 자바 기본 API - Class Class](https://palpit.tistory.com/entry/Java-%EC%9E%90%EB%B0%94-%EA%B8%B0%EB%B3%B8-API-Class-Class)