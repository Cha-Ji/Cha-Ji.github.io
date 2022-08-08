---
title: '[Android] Activity Lifecycle'
date: 2022-08-08 11:30:13
category: 'Android'
draft: false
---

## onCreate()

액티비티가 최초 실행될 때 한 번 호출됩니다.
필요한 리소스를 초기화하며 이전 상태가 저장되어 있을 경우 번들 객체를 참조해 복원가능합니다.

## onStart()

액티비티가 화면에 보이기 직전에 호출됩니다.
빠르게 끝나고 onResume() 단계로 넘어갑니다.

## onResume()

액티비티가 사용자와 상호작용하기 직전에 호출됩니다.
바로 액티비티가 사용자에게 보여지며 사용자에게 focus를 잡은 상태입니다.

## onRestart()

액티비티가 중지된 이후에 호출되는 메서드이며 다시시작되기 직전에 호출됩니다.

## onPause()

focus를 잃었을 때 호출됩니다. 다른 액티비티가 호출되기 직전에 실행되기 때문에 많은 일을 처리하는 것을 권장하지 않습니다.

## onStop()

액티비티가 완전히 가려질 때 호출되는 함수입니다.
이 상태에서 액티비티가 다시 불려지면 onRestart()가 호출됩니다.

## onDestroy()

액티비티가 완전히 스택에서 없어질 때 호출됩니다.

### References

- https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Android/Activity%20Lifecycle.md