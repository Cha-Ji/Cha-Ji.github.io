---
title: '[자료구조] Heap'
date: 2022-08-07 15:00:00
category: 'D.S.'
draft: false
---

# Heap

완전 이진트리이며, 최솟값이나 최댓값을 빠르게 찾을 수 있는 자료구조입니다.
이진탐색트리와 달리 중복된 값을 허용합니다.

## 구현

배열로 트리와 유사하게 만듭니다.
왼쪽자식은 부모인덱스 * 2, 오른쪽 자식은 부모인덱스 * 2 + 1로 계산합니다.

### 삽입

1. 노드를 만들어 힙의 마지막노드 뒤에 삽입합니다.
2. 부모를 제쳐가며 본인의 위치를 찾아갑니다.

```kotlin
class MaxHeap {
    private val storage = mutableListOf<Int>()
    
    fun insert(x: Int) {
        storage.add(x)
        
        var i = storage.size
        while (x > storage[i / 2]) {
            storage[i] = storage[i / 2]
            i /= 2
        }
    }
}
```

### 삭제

max heap의 삭제연산은 최댓값을 삭제하는 것이며 최댓값은 항상 root에 존재합니다.
root 노드를 삭제한 뒤, 자식노드로 빈 칸을 채우며 마지막 노드를 비우고 배열의 크기를 하나 줄이는 것으로 완성됩니다.

```kotlin
class MaxHeap {
    private val storage = mutableListOf<Int>()
    
    fun delete(): Int? {
        if (storage.isEmpty()) return null
        
        var i = 1
        while (storage[i] < storage[i * 2] || storage[i] < storage[i * 2 + 1]) {
            val temp = storage[i]
            storage[i] = storage[i * 2]
            storage[i * 2] = storage[i]
            i *= 2
        }
    }
}
```

### References

- https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html