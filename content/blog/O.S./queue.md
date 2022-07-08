---
title: '[OS] 다단계 큐와 다단계 피드백 큐'
date: 2020-07-02 16:21:13
category: 'O.S.'
draft: false
---

### Multilevel Queue

> 여러개의 큐에 여러 스케줄링을 적용하는 기법

다단계 큐를 이해하기 앞서 다른 스케줄링 기법을 알 필요가 있습니다.

여기서 언급하는 기법은 FCFS, RR, Priority 세 기법입니다.

작업이 분류되고, Ready Queue에서는 우선순위에 맞게 작업이 스케줄링 됩니다.

시스템 작업, 대화형 작업, 일괄처리 작업 등으로 분류하는 것을 말합니다.

분류된 작업이 실행되며 Round Robin 기법에 따라 프로세스가 실행됩니다.

여기서 foreground queue와 background queue를 나눠서 생각합니다.

foreground는 interactive, background는 batch 식입니다.

foreground에서는 사용자가 느끼기에 끊김없이 진행되어야 하기 때문에 RR 방식을 사용하게 됩니다.

상대적으로 time quantum이 긴 프로세스는 background queue에 할당될 것입니다.

또한 context switch를 줄여 최소한의 시간에 작업을 마치는 것을 원하기 때문에 FCFS에 근접하게 됩니다.

Android에서 예를 들면 메인 스레드에서는 UI 작업을 RR 스케줄링 기법에 의해 실행합니다.

DB에 insert하고 read하는 과정은 time quantum이 길 수 있으며 foreground queue에 할당하면 에러가 발생합니다.

때문에 background queue에 할당되며 사용자가 UI를 보는 동안 background에서 긴 time quantum을 수행하게 됩니다.

단점도 존재합니다.

우선순위 스케줄링의 고질병인 starvation이 발생할 수 있습니다.

또한 스케줄링 오버헤드가 낮은 대신 inflexible 합니다. 프로세스가 적재된 큐에서 다른 큐로 이동할 수 없습니다.

그에 대한 대안으로 Feedback 큐 기법이 있습니다.

### Multilevel Feedback Queue

> time quantum으로 interactive한 작업인지를 판단하는 feedback을 주는 스케줄링 기법입니다.

우선 큐 하나에 모든 프로세스가 적재됩니다.

RR 방식으로 CPU를 할당합니다.

time quantum 내에 끝나지 않은 프로세스는 다음 큐로 넘깁니다.

최종적으로는 FCFS 방식이 됩니다.

I/O burst가 빈번하게 일어나는 일은 우선순위가 높다고 판단됩니다.

해당 작업은 I/O 작업과 CPU 처리가 번갈아 일어나므로 CPU burst도 자연스레 작습니다.

그러므로 interactive한 작업은 time quantum 내에 끝나게 되며 우선순위가 높은 채로 남습니다.

starvation은 그대로 남아있습니다. 해당 문제는 aging으로 해결합니다.

flexible하기 때문에 높은 우선순위 큐로 프로세스를 이동하기 수월합니다.

### References

[\[운영체제\]Multilevel Queue 다단계큐, 멀티레벨큐](https://jhnyang.tistory.com/28)

[https://jhnyang.tistory.com/156](https://jhnyang.tistory.com/156)