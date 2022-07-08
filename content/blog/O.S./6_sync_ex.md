---
title: '[공룡책] 동기화 예제'
date: 2020-07-02 16:21:13
category: 'O.S.'
draft: false
---

[Operating System - 동기화]()

이전 글에서는 발생할 수 있는 문제에 대해 많이 언급했습니다.

또한 임계구역 문제, 경쟁조건이 없는 프로그램을 설계할 때 발생하는 문제, 교착상태, 라이브니스 위험 등을 언급했습니다.

이 글에서는 제시된 도구를 고전적인 동기화 문제에 적용합니다.

## 유한 버퍼 문제

먼저 유한 버퍼 문제가 존재합니다.

```
// n개의 버퍼로 구성된 pool이 존재하며 각 버퍼는 한 item을 저장
val n: Int
var mutext: Semaphore = 1 // 상호 배제 기능을 제공
var empty: Semaphore = n  // 비어있는 버퍼 수
var full: Semaphore = 0   // 사용중인 버퍼 수

//================================================
// 생산자
while (true) {
  // produce
  wait(empty)
    wait(mutext)

    // next produce
    signal(mutex)
    signal(full)
}

//================================================
// 소비자
while (true) {
    wait(full)
    wait(mutex)
    // remove item

    signal(mutex)
    signal(empty)
    //consume
}
```

### Readers-Writers 문제

> 병행 프로세스들이 한 DB를 공유한다면 배타적 접근 권한이 필요합니다.

1.  첫 번째 문제

-   공유 객체를 사용할 수 있는 허가를 얻지 못했다면, 다른 reader들이 끝날 때까지 기다리는 reader가 있으면 안된다.
-   → writer의 starvation

1.  두 번째 문제

-   writer가 접근중이라면, 새 reader들은 읽기를 시작할 수 없다.
-   → reader의 starvation

### Reader-wirter lock

> 해당 문제는 두 가지모드를 지원하는 락을 둬서 해결합니다.

-   읽기모드의 락, 쓰기모드의 락이 존재합니다.
-   reader 락은 여러 프로세스가 동시에 획득하는 것이 가능합니다.
-   writer 락은 하나의 프로세스만 획득할 수 있습니다.

다음과 같은 상황에서 유용합니다.

-   공유데이터를 읽기만하는 프로세스와 쓰기만하는 스레드를 식별하기 쉬운 응용
-   Wirter보다 reader의 개수가 많은 응용
    -   reader-writer lock을 설정하는 오버헤드는 semaphore나 mutex를 설정할 때보다 큽니다.
    -   reader들의 병행성을 높여 상쇄합니다.

## 식사하는 철학자

> 이전 글에서 언급한 식사하는 철학자 문제의 해결안을 살펴봅니다.

```
1. 일정 시간 생각을 한다.
2. 왼쪽 포크가 사용 가능해질 때까지 대기한다. 만약 사용 가능하다면 집어든다.
3. 오른쪽 포크가 사용 가능해질 때까지 대기한다. 만약 사용 가능하다면 집어든다.
4. 양쪽의 포크를 잡으면 일정 시간만큼 식사를 한다.
5. 오른쪽 포크를 내려놓는다.
6. 왼쪽 포크를 내려놓는다.
7. 다시 1번으로 돌아간다
```

-   두 포크를 모두 집을 수 있는지 판단한 뒤에 집습니다.
    -   임계구역 안에서만 포크를 집어야합니다.
-   비대칭 해결안을 활용합니다.
    -   예를 들어, 홀수번호는 왼쪽 포크부터 집고 짝수번호는 오른쪽 포크부터 집습니다.
-   우선순위로 해결합니다.
    -   포크에 우선순위를 두고 양쪽 포크 중 우선순위가 높은 포크부터 집습니다.
    -   한 명은 하나의 포크도 집을 수 없게 되고 한명이 식사를 마치면 두명씩 식사를 할 수 있습니다.
-   시간제한으로 해결합니다.
    -   사용하지 않는 포크에 시간제한을 둡니다.
    -   동시에 내려놓지 않는다고 가정하면 한 명이 포크를 내려놓는 순간 옆자리 한명이 식사를 시작하게 됩니다.

해당 해결안들은 효율성이 고려되지 않았습니다.

데드락이 없는 해결안이 반드시 기아의 가능성도 제거하지는 않습니다.

## 모니터

> 앞서 언급한 식사하는 철학자의 해결안을 제시하며 모니터의 개념을 설명합니다.

양쪽 포크를 모두 집을 수 있을 때에만 포크를 집을 수 있도록 강제합니다.

```
enum class State {
    THINKING,
    HUNGRY,
    EATING
}

class DiningPhilosophers : Monitor {

    val state = mutableListOf<State>()

    // 원하는 포크를 집을 수 없을 때 집기를 미룰 수 있게 한다.    
    val self = mutableListOf<Condition>() 

    init {
        (0..5).forEach { i ->
            state.append(THINKING)
        }
    }

    // 식사 전
    fun pickUp(i: Int) {
        state[i] = HUNGRY
        eat(i)

        if (state[i] != EATING) self[i].wait()    
  }

  // 식사 후
    fun putDown(i: Int) {
        state[i] = THINKING
        eat(left(i))
        eat(right(i))
    }

    private fun left(i: Int) = (i + 4) % 5

    private fun right(i: Int) = (i + 1) % 5

    fun eat(i: Int) {
        if (canEat(i)) {
                state[i] = EATING
                self[i].signal()
        }
    }

    private fun canEat(i: Int) = state[left(i)] != EATING && 
        state[i] == HUNGRY && 
        state[right(i)] != EATING
}
```

-   해당 코드는 공룡책 그림7.7의 코드를 kotlin으로 작성했으며, 가독성을 위해 코드를 세분화한 코드입니다.

데드락을 해결한 코드이지만 기아 상태를 해결하진 못했습니다.

### References

-   Abraham Silberschatz, Peter B. Galvin, Greg Gagne의 『Operating System Concept 10th』