---
title: '[O.S.] Thread vs Process'
date: 2022-08-07 18:05:00
category: 'O.S.'
draft: false
---

## 프로세스

프로그램은 실행할 수 있는 파일을 말하며 코드 덩어리에 불과합니다.
프로세스는 실행되고 있는 프로그램의 인스턴스를 뜻합니다.

독립된 Code, Data, Stack, Heap 메모리 영역을 할당받으며 최소 1개의 스레드를 갖습니다.
다른 프로세스의 자원에 접근하기 위해서는 IPC를 사용하게 됩니다.

## 쓰레드

스레드는 프로세스가 할당받은 자원을 사용하는 실행단위를 뜻합니다.
프로세스 내의 Code, Data, Heap 영역은 공유하며 Stack 영역을 독립적으로 소유합니다.

## 멀티프로세스 vs 멀티스레드

멀티스레드 방식은 여러 장점이 존재합니다.
context switching을 할 때 stack 영역만 처리하므로 비용이 적습니다.
프로세스 간 통신에는 IPC가 필요하지만 스레드는 Stack을 제외한 영역을 공유하기 때문에 통신이 자유롭습니다.
하지만 그만큼 동기화 문제가 발생할 위험이 증가합니다. 때문에 Thread-safe한 구현이 필요할 수 있습니다.

### Thread-safe

멀티스레드 환경에서 공유자원에 동시에 접근할 때 의도한 대로 동작하는 것을 의미합니다.
이를 위해 상호배제를 통해 임계영역을 제어할 필요가 있습니다.
Mutex나 Semaphore 기법 등이 존재합니다.

### Reentrant

재진입성이라는 의미로 서브루틴에서 공유자원을 사용조차 하지않습니다.
Thread-safe함을 보장할 수 있습니다.

## Mutex Semaphore

### Mutex

임계영역을 가진 스레드의 Running time을 겹치지 않게하는 기술입니다.
synchronized 또는 lock을 사용해 두 스레드가 동시에 공유자원을 사용할 수 없도록 합니다.

### Semaphore

공유자원의 데이터를 접근하는 스레드의 개수를 한정짓습니다.
Semaphore는 시스템 범위에 걸쳐있고 파일 형태로 존재하는 반면 Mutex는 프로세스 범위를 갖고 프로세스가 종료될 때 Clean up 됩니다.

### References

- https://github.com/WeareSoft/tech-interview/blob/master/contents/os.md#프로세스와-스레드의-차이