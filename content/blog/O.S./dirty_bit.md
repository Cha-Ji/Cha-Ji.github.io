---
title: '[OS] Dirty Bit'
date: 2020-07-02 16:21:13
category: 'O.S.'
draft: false
---
> 가상기억 장치 시스템에서, 교체될 페이지의 내용이 변경되었는지를 표시하는 비트

### 페이징

앞선 글에서 페이징에 대해 언급했습니다.

페이징 기법은 데이터를 페이지 단위로 쪼개서 관리하는 기법입니다.

보조기억장치는 공간이 큰 대신 접근할 때 탐색이 어렵습니다.

때문에 자주 사용하는 데이터는 주기억장치에 두고 접근합니다.

### Dirty Bit

> write한 적 있는 data

page 단위로 처리하는 데이터는 주로 주기억장치에서 읽고 씁니다.

보조기억장치로 가기전에 주기억장치가 종료된다면 데이터는 사라지게 됩니다.

이러한 문제를 막고자 변경된 page를 수시로 보조기억장치에 저장하는 작업을 진행합니다.

Dirty Bit(Modify Bit)는 write한 데이터를 표시하기 위한 bit이며 데이터의 손실을 막아줍니다.

## References

-   [https://bluemoon-1st.tistory.com/21](https://bluemoon-1st.tistory.com/21)