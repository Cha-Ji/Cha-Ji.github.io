---
title: '[자료구조] Red-Black-Tree'
date: 2022-08-06 23:00:00
category: 'D.S.'
draft: false
---

## Red Black Tree

BST는 삽입, 탐색, 삭제 연산이 O(h)의 시간복잡도를 갖습니다. (h는 트리의 높이)
트리의 밸런스가 무너진다면 트리의 높이는 N이 될 수 있습니다.
기존 BST 자료구조에서 트리의 균형을 잡아 최악의 경우를 없애 O(logN)을 유지하기 위해 Red-Black Tree를 사용합니다.

## 규칙

### Color

1. Red의 자식은 Black 입니다.
2. 루트노드와 리프노드(NIL노드)는 Black 입니다.
3. 모든 노드에 대해서 그 노드로부터 자손인 리프노드에 이르는 모든 경로에는 동일한 개수의 Black이 존재합니다.
 
### NIL 노드

균형을 맞추기 위해 필요한 더미노드입니다.

새로 삽입되는 노드가 Black이라면 3번 규칙을 어기게 됩니다. 
때문에 Red를 삽입하면 2번 규칙을 어기게 됩니다.
이 때, Red를 삽입하고 항상 자식노드로 Black이 존재한다면 1,2,3번 규칙 모두 지켜낼 수 있습니다.
이를 위해 추가된 노드가 NIL 노드입니다. NIL 노드는 항상 모두의 자식으로써 Black으로 존재합니다.

## 높이

레드블랙트리의 핵심은 트리의 균형을 잡아 높이를 logN으로 줄여 삽입, 탐색, 삭제연산의 시간복잡도를 최악으로 향하지 않게 하는 것입니다.
높이는 리프노드까지의 경로에 포함된 엣지의 개수입니다.

`1. 높이가 h인 노드의 bh는 bh >= h/2를 만족합니다.`
`2. 노드 x를 루트로하는 임의의 서브트리는 적어도 2^bh(x) - 1개의 내부노드를 포함합니다.`
`(단, bh는 블랙 높이를 뜻하며 내부노드는 리프노드를 제외한 노드의 개수를 뜻합니다.)`

### 높이에 대한 증명

첫번째는 정의를 통해 유추할 수 있습니다.
Red노드는 Black노드와 달리 연속될 수 없으므로 연속될 수 있으며 루트와 리프노드인 Black 노드의 개수는 항상 Red 노드의 개수보다 크거나 같습니다.

두번째는 귀납법으로 증명합니다.
```
h(x)는 x노드의 높이, n(x)는 x노드의 서브트리 노드의 개수라 가정합니다.

i) h(x) = 0일 때, 
높이가 0인 x는 리프노드입니다. 이 때, 내부노드의 개수는 0입니다.
따라서, 2^0 - 1 = 0이 성립합니다.

ii) h(x) = k가 성립한다고 가정할 때, h(x) = k + 1인 경우
x의 바로 밑에 있는 두 자식노드를 각각 a, b라고 가정합니다.
n(x) >= n(a) + n(b) + 1 이고,

내부노드의 개수는 적어도 높이보다 크거나 같으므로 본인 노드를 제외한 개수보다 크거나 같습니다.
n(x) >= 2bh(a) - 1 + 2bh(b) - 1 + 1 = 2bh(a) + 2bh(b) - 1

a, b노드가 각각 black일 때를 고려해
bh(a) >= bh(x) - 1 이므로 
n(x) >= 2bh(a) - 1 + 2bh(b) - 1 + 1 = 2bh(a) + 2bh(b) - 1 >= 2bh(x) - 1 + 2bh(x) - 1 - 1
따라서, n(x) >= 4bh(x) - 3 >= 2bh(x) - 1 가 성립합니다.
```

위 증명을 통해 시간복잡도 역시 증명할 수 있습니다.
```
2^bh <= n
2^bh <= n + 1
bh <= log2(n + 1)
h <= 2log2(n + 1)
```

따라서, h = O(logn)이 성립합니다.

## 회전

x의 right 노드를 y라 할 때, y의 left 노드를 x노드로 만드는 것을 left rotate라 합니다.

```kotlin
/** condition
y != right[x] != NIL
parent[root] == NIL
*/

fun leftRotation(x: Node) {
    val y = right[x]
    right[x] = left[y]
    parent[left[y]] = x
    parent[y] = parent[x]

    when {
        parent[x] == NIL -> root = y
        x == left[parent[x]] -> left[parent[x]] = y
        else -> right[parent[x]] = y
    }

    left[y] = x
    parent[x] = y
}
```

### References

- https://github.com/namjunemy/TIL/blob/master/Algorithm/red_black_tree_01.md