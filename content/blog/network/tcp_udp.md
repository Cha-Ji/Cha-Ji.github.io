---
title: '[Network] TCP와 UDP'
date: 2022-08-08 11:00:00
category: 'network'
draft: false
---

TCP와 UDP는 OSI 계층 중 4번째 계층은 전송 계층에서 사용하는 프로토콜입니다.

## TCP

IP가 데이터를 전송하고 TCP는 패킷을 추적 및 관리합니다.
3-way handshaking, 4-way handshaking을 통해 신뢰성 있는 연결을 보장합니다.
full-duplex, point to point 방식이며 양방향 전송이 가능하고 각 연결이 정확히 2개의 종단점을 갖고 있다는 의미입니다.

### 흐름제어

데이터를 송신하는 곳과 수신하는 곳의 데이터 처리속도를 조절하여 수신자의 버퍼 오버플로우를 방지합니다.
송신하는 곳에서 너무 많은 데이터를 보내서 생기는 문제를 방지합니다.

### 혼잡제어

네트워크 내의 패킷수가 넘치지 않도록 방지합니다.
정보의 소통량이 과대할 때 억제합니다.

## UDP

데이터그램 단위로 데이터를 처리하며 비연결형 서비스입니다.
헤더의 CheckSum 필드를 통해 최소한의 오류만을 검출합니다.
신뢰성이 낮지만 빠른 속도를 보장합니다. 주로 실시간 스트리밍에 사용됩니다.

### References

- https://github.com/WeareSoft/tech-interview/blob/master/contents/network.md