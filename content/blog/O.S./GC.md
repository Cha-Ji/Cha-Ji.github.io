---
title: '[OS] Garbage Collector'
date: 2020-07-02 16:21:13
category: 'O.S.'
draft: false
---

# 메모리 관리 시뮬레이션

## Heap allocation Algorithm

[Buddy memory allocation - Wikipedia](https://en.wikipedia.org/wiki/Buddy_memory_allocation)

-   Heap 자료구조는 주로 동적으로 할당되며 블록의 크기가 일정하지 않습니다.
-   따라서 외부 단편화가 생기는 경우가 많으며 이를 방지하기 위해 빈 공간을 찾아 집어넣는 과정이 필요합니다.
-   때문에 연결리스트나 해쉬맵 자료구조가 어울립니다.

\*단편화: 선형적인 자료구조를 사용한다면 가변적인 크기의 블록을 해제하고 채워가는 과정에서 중간에 padding이 많아지고 빈 공간이 많이 생기게 됩니다.

## 프로세스 메모리

---

-   메모리 계층은 CPU와의 거리에 따라 분류됩니다.
-   레지스터 -> 캐시 -> 메인메모리 -> 보조기억장치 -> 외부기억장치 순입니다.
-   메모리의 구조는 크게 Stack, Heap, Data, Code 네 영역으로 나뉩니다.

### Heap

-   메모리 구조에서의 heap은 자료구조에서의 heap과는 다릅니다.
-   동적할당에 주로 사용되는 메모리이며 일반적으로 stack보다 큰 용량을 차지합니다.
-   컴퓨터는 컴파일 시간동안 코드를 번역하고 런타임 동안 실행하는데, Heap은 이 때 크기가 결정됩니다.
-   번역할 때가 아니라 실행할 때 사용되기 때문에 컴퓨터가 아닌 사용자가 관리해야합니다.

### Stack

-   Stack 은 자료구조에서의 Stack 과 흡사합니다.
-   LIFO 방식에 따라 동작하며 높은 주소에서 낮은 주소의 방향으로 할당됩니다.

### data 영역

-   전역변수, 정적변수, 배열, 구조체 등이 저장됩니다.
-   초기화되지 않은 데이터는 BSS에 저장되며 static 변수는 공간만 할당되고 함수가 실행될 때 마저 초기화됩니다.

### Code 영역

-   코드 자체를 구성합니다. 기계어로 제어됩니다.

[출처 - Recorda (Tistory블로그)](https://recorda.tistory.com/entry/20160503%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B5%AC%EC%A1%B0)

## JVM 에서의 GC

---

-   stop the world 와 mark and sweep 의 단계를 거쳐 동작합니다.

### stop-the-world

-   stop-the-world 는 GC 이외의 모든 쓰레드 작업을 멈추는 것입니다.
-   이 stop-the-world 시간을 줄이는 것이 GC의 효율을 높입니다.

### mark-and-sweep

-   사용되는 메모리와 사용되지 않는 메모리를 식별하고
-   식별된 메모리를 해제하는 작업입니다.

### Young 영역

-   매우 많은 객체가 들렸다 가는 곳입니다. Minor GC 가 발생하면서 객체가 사라집니다.

### Old 영역

-   Young 에서 살아남은 객체가 여기로 복사됩니다. 용량도 크게 할당하며 Major GC가 발생하면서 객체가 사라집니다.

### Minor GC

-   Eden: 새로 생성된 객체가 할당되는 영역
-   Survivor: 살아남은 객체
-   새 객체가 Eden 영역에 할당되고 Eden이 꽉차면 Minor GC가 실행됩니다.
-   해제가 진행되다가 2개의 Survivor 영역이 꽉차면 Old 영역으로 이동하게 됩니다.

### Major GC

-   Old 영역의 메모리가 부족해지면 작동합니다. Young 영역보다 크기도 크고 시간도 10배 이상 오래걸립니다.

[출처 - 망나니개발자 (Tistory블로그)](https://mangkyu.tistory.com/118)
