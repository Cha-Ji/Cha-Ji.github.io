---
title: '[공룡책] 프로세스'
date: 2020-07-02 16:21:13
category: 'O.S.'
draft: false
---

> 앞서 작성한 운영체제에 이어서 프로세스에 대해 간단히 이야기합니다.

## 프로세스

> 초기 컴퓨터 시스템은 한 번에 하나의 프로그램만을 실행시켰습니다.  
> 반면 오늘날의 컴퓨터 시스템들은 메모리에 다수의 프로그램이 적재되어 병행 실행되는 것을 허용합니다.

프로세스란 실행중인 프로그램을 말합니다. 프로세스가 무엇이고 어떻게 표현되며 어떻게 동작하는지 알아보겠습니다.

### 프로세스의 특징

-   병렬 실행이 가능합니다.
-   독립된 메모리 영역과 주소 공간을 운영체제로부터 할당받습니다.

### 프로세스 메모리 배치

-   Code 영역, Data 영역, Heap 영역, Stack 영역이 존재합니다.
-   Code 영역, Data 영역은 Run time 동안 고정된 크기를 갖습니다. 하지만 Stack 영역과 Heap 영역은 동적으로 크기가 조정되며 서로의 방향으로 확장됩니다.

### PCB

-   PCB는 프로세스 제어 블록을 의미합니다.
-   프로세스 상태, 프로세스 번호, Program Counter, Register, 메모리 제한, 오픈 파일 리스트를 포함합니다.
-   프로세스 상태는 new, ready, running, waiting, halted가 존재합니다.
-   Program Counter는 다음에 실행할 명령어의 주소를 가리킵니다.

### Context Switching

-   인터럽트가 발생할 때 현재 진행하고 있는 Task의 상태를 저장하고 다음 진행할 Task의 상태 값을 읽어 적용하는 과정입니다.
-   커널은 앞서 언급한 PCB에 문맥을 저장하고, 새 프로세스의 저장된 context를 복구합니다.

### Android Process Hierarchy

-   제한된 시스템 자원을 회수하기 위해 기존 프로세스를 종료해야 할 수도 있습니다.
-   중요도 계층을 식별합니다.
-   forground process -> visible process -> service process -> background process -> empty process

### IPC

-   InterProcess Communication의 준말로 프로세스 간 통신을 의미합니다.
-   information sharing, computation speedup, modularity
-   IPC에는 shared memory, message passing 두 모델이 존재합니다.
    -   shared memory model
        -   협력 프로세스들에 의해 공유되는 메모리의 영역이 구축됩니다.
        -   공유 메모리 영역을 구축할 때만 시스템 콜이 필요합니다.
        -   모든 접근이 일반적인 메모리 접근으로 취급되기 때문에 커널의 도움이 필요 없습니다.
    -   message passing model
        -   협력 프로세스들 사이에 교환되는 메시지를 통해 통신이 이루어집니다.
        -   충돌을 회피할 필요가 없기 때문에 적은 양의 데이터를 교환하기 적합합니다.
-   많은 시스템들이 두 모델을 모두 구현합니다.

### socket

-   socket은 통신의 endpoint를 뜻합니다. ip주소와 포트번호를 접합하여 구별합니다.

### RPC

-   Remote Procedure Calls의 준말입니다. 또한 IPC 기반 위에 만들어집니다.

### Anroid RPC

-   RPC는 동일 시스템에서 실행되는 프로세스 간 IPC의 형태로 사용될 수도 있습니다.
-   Android OS는 바인더 프레임워크에 포함된 풍부한 IPC 기법의 집합을 갖는데, 이 중 RPC는 프로세스가 다른 프로세스의 서비스를 요청할 수 있게 합니다.
-   이러한 응용 프로그램의 구성요소 중 하나는 서비스가 있습니다.
    -   클라이언트 앱이 bindService()를 호출하면 해당 서비스가 bound 되어 메시지 전달 또는 RPC를 사용해 통신을 제공합니다.
    -   Messenger 서비스는 단방향 통신만 가능합니다. 서비스와 클라이언트 모두 Messanger 서비스를 제공해야 서로 통신이 가능합니다.
-   onBind() 메소드는 상호작용을 위해 원격 오브젝트 안에 메소드를 정의하는 인터페이스를 반환해야 합니다.
    -   일반 java 구문으로 작성되며 (Android Interface Definition Language)를 사용해 스텁 파일을 생성합니다. 또한 (RemoteService.aidl)로 저장됩니다.
-   bindService() -> onBind() -> RemoteService 객체의 스텁이 클라이언트에 반환 -> remoteMethod()
-   매개변수 정돈을 처리 -> 프로세스 사이에 정돈된 매개변수를 전송 -> 필요한 서비스 구현을 호출 -> 반환값을 클라이언트 프로세스에 전송

```
interface RemoteService {
  boolean remoteMethod(int x, double y);
}
```

### References

-   Abraham Silberschatz, Peter B. Galvin, Greg Gagne의 『Operating System Concept 10th』