---
title: '[공룡책] 입출력 시스템'
date: 2022-07-11 09:05:00
category: 'O.S.'
draft: false
---

컴퓨터의 두 가지 주요 작업은 계산과 입출력 작업입니다.
이 중 입출력 작업이 더 중요한 경우가 많습니다.
이 글에서는 운영체제의 입출력 서브시스템의 구조를 살펴봅니다.

## 인터럽트

CPU는 명령어를 끝내고 다음 명령을 수행하기 전 항상 이 인터럽트 요청 라인을 검사합니다.

예기치 않은 상황을 만났을 때, 현재보다 더 중요한 일이 발생했을 때 인터럽트가 발생합니다.

외부 인터럽트는 전원 이상, 기계착오, 외부신호, 입출력 등 외부적인 요인으로 발생합니다.

내부 인터럽트는 Trap이라고 부르며, 0나누기, 오버플로우 등 잘못된 명령이나 데이터를 사용할 때 발생합니다.

소프트웨어 인터럽트는 요청에 의해 발생합니다. (SVC 인터럽트)

### 처리 과정

1. 입출력 하드웨어 컨트롤러가 신호를 보낸다.
2. CPU가 알아차리고 각종 레지스터 값과 상태 정보를 저장한다.
3. 메모리 상 인터럽트 핸들러 루틴으로 이동한다.

### 풀링

풀링은 사용자가 명령어를 사용해 입력핀의 값을 계속읽어 변화를 알아내는 방식입니다.

인터럽트 요청 플래그를 차례로 비교하며 인터럽트 핸들러 루틴을 수행합니다.

### 인터럽트

인터럽트 방식은 MCU 자체가 하드웨어적 변화를 체크하여 변화 시에만 일정한 동작을 합니다.

하드웨어로 지원을 받아야한다는 제약이 있지만 신속한 대응이 가능합니다.

- Vector Interrupt - 프로그램 루틴없이 하드웨어적으로 판별하며 회로가 복잡하고 추가 하드웨어가 필요합니다. 직렬과 병령 우선순위 부여 방식이 있습니다.
- Daisy Chain - 인터럽트가 발생하는 모든 장치를 우선순위에 따라 한 회선에 직렬로 연결합니다. 병렬 부여 방식은 장치마다 별개의 회선으로 연결합니다.

## 캐싱

자주 사용될 자료의 복사본을 저장하는 빠른 메모리 영역입니다.

복사를 한다는 점에서 버퍼와의 차이가 있습니다.

### 캐시의 지역성

자주 사용될 자료를 예측하기 위해, Hit rate를 높이기 위해 지역성 원리를 사용합니다.

최근 참조된 주소의 내용은 다시 참조되는 특성을 시간 지역성이라 합니다.

- 재귀함수, 스택의 예시가 있습니다.

참조된 주소와 인접한 주소의 내용이 다시 참조되는 특성을 공간 지역성이라 합니다.

- 배열을 순회할 때 인접한 주소를 순회하는 예시가 있습니다.

### 캐싱 라인

캐시의 데이터를 빠르게 참조하기 위해 태그를 붙입니다. 이 때 태그의 묶음을 캐싱라인이라 합니다.

종류로는 대표적으로 Fully Associative, Set Associative, Direct Map 세 가지가 있습니다.

참고: [https://gofo-coding.tistory.com/entry/5-Set-Associative-Mapping](https://gofo-coding.tistory.com/entry/5-Set-Associative-Mapping)

### References

- [https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer Science/Operating System/Interrupt.md](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Operating%20System/Interrupt.md)

- [https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/OS#캐시의-지역성](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/OS#%EC%BA%90%EC%8B%9C%EC%9D%98-%EC%A7%80%EC%97%AD%EC%84%B1)
- Abraham Silberschatz, Peter B. Galvin, Greg Gagne의 『Operating System Concept 10th』