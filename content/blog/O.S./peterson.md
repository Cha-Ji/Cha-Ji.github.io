---
title: '[OS] 피터슨의 알고리즘'
date: 2020-07-07 16:21:13
category: 'O.S.'
draft: false
---

Critical Section 문제를 해결하기 위해서는 3가지 조건이 충족해야 합니다.

세 가지 조건을 충족한 고전적인 방법으로 피터슨의 알고리즘이 존재합니다.

### 기본 개념

프로세스가 2개일 때만 가능하고, 3개일 때는 아직 연구중

자원을 공유하는 2개의 프로세스 간에는 Flag와 turn변수가 공유됩니다.

turn은 PCB와 비슷한 개념입니다. 진입할 프로세스의 번호가 저장되어 있습니다.

flag는 프로세스가 임계구역에 들어가 실행할 준비가 되었는지 판별하는 Boolean입니다.

2개의 프로세스가 동시에 접근한다면, turn의 값이 덮어씌워지기 때문에 문제가 생기지 않습니다.

### Mutual Exclution

-   turn 변수는 다음에 올 프로세스의 번호입니다.
-   turn 변수는 0 또는 1입니다. 0과 1의 값을 동시에 갖지 못해 두 프로세스가 동시에 접근할 수 없습니다.

### Progress

-   임계영역에 어떤 스레드도 실행되지 않는다면 바로 접근이 되어야 합니다.
-   실행이 끝난 프로세스의 flag는 false가 되므로 성립합니다.

### Bounded Waiting

-   자원을 요청하는 프로세스가 영원히 대기하지 않아야 합니다.
-   flag 변수가 있기 때문에 둘 다 영원히 false일 순 없습니다.

### 문제점과 해결

-   현대 컴퓨터 구조가 load와 store 같은 기본적인 기계어를 수행하는 방식 때문에 올바른 실행을 보장할 수 없습니다.
-   소스코드의 순서가 바뀌는 것을 방지하면 해결됩니다.
    -   Volatile 키워드 사용 - 컴파일러 동작 순서 보장
    -   메모리 펜스 사용 - Out-of-Other을 방지
        -   CPU가 최적화를 통해 서로 의존성이 없다고 판단하면 코드의 순서를 마음대로 바꾸는 것을 의미
        -   turn = 1, flag\[0\] = true 두 코드의 순서가 바뀌게 되어 상호 배제가 끊어지게 되는 경우를 말한다.

### 의사코드

```
bool flag[2] // 불린 배열(Boolean array)
int turn // 정수형 변수

flag[0]   = false // false은 임계 구역 사용을 원하지 않음을 뜻함.
 flag[1]   = true
 turn      = 0 // 0 은 0번 프로세스를 가리킴, 1은 1번 프로세스를 가리킴

P0: flag[0] = true // 임계 구역 사용을 원함
     turn = 1 // 1번 프로세스에게 차례가 감
     while( flag[1] && turn == 1 )
     {
          // flag[1] 이 turn[1] 을 가지고 있으므로
          //현재 사용중임
          // 임계 구역이 사용 가능한지 계속 확인
     }// 임계 구역
     ...
    // 임계 구역의 끝
   flag[0] = false

P1: flag[1] = true
    turn = 0
    while( flag[0] && turn == 0 )
    {
         // 임계 구역이 사용 가능한지 계속 확인
    }// 임계 구역
    ...
    // 임계 구역의 끝

flag[1] = false
```

### 읽어볼만한 다른 기법

[상호배제 여러가지 기법들 : SW solution](https://yoongrammer.tistory.com/61)

[데커의 알고리즘 - 위키백과, 우리 모두의 백과사전](https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%BB%A4%EC%9D%98_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98?tableofcontents=1)

### References

[11011.피터슨의 알고리즘 : 고전적인 해결법!](https://haun25ne.tistory.com/58?category=718126)

[11100\. 피터슨 알고리즘 : 조건 3개에 맞는거야?](https://haun25ne.tistory.com/59)

[피터슨의 알고리즘 - 위키백과, 우리 모두의 백과사전](https://ko.wikipedia.org/wiki/%ED%94%BC%ED%84%B0%EC%8A%A8%EC%9D%98_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)