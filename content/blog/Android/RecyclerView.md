---
title: '[Android] RecyclerView'
date: 2000-07-02 16:21:13
category: 'Android'
draft: false
---

## Recycler View

- 영어 뜻 그대로 재활용하는 뷰를 의미합니다.
- 스크롤을 자동으로 지원하며 화면에서 사라지거나 생겨나는 뷰를 재사용합니다.

## 구성

뷰의 구성요소를 참조할 때에는 `findViewById()` 메서드를 사용하게 됩니다.
뷰를 재사용할 때마다 해당 메서드를 호출하는 것은 비용이 커질 수 있습니다.
이 때 각 뷰 객체를 뷰홀더에 보관함으로써 반복적 호출 메서드를 줄여 속도를 개선하는 패턴이 ViewHolder 패턴입니다.

또한 호환되지 않는 두 객체를 연결하는 패턴이 Adapter 패턴입니다.
여기서는 `ViewHolder`와 `xml`상의 데이터를 연결해줍니다.

- `onCreateViewHolder()`
  - `ViewHolder`를 생성할 때 호출되는 메서드입니다. `BaseViewHolder` 클래스를 만들어두고 제네릭과 매개변수를 활용해 재사용할 수 있습니다.
  - 이 때, 데이터바인딩을 적용해 `bind()` 함수의 코드를 `xml`상에서 구현해야 같은 `ViewHolder`를 재사용하기 수월합니다.
- `onBindViewHolder()`
  - `ViewHolder`와 Data를 연결할 때 호출되는 메서드입니다.

## 구현

기본적인 `RecyclerView`는 아래 과정을 거쳐 구현할 수 있습니다.

- 사전에 라이브러리를 추가합니다.
- 재사용할 view를 xml파일로 만듭니다.
- Linear, Grid 등의 Layout을 설정합니다.
- 세 함수를 override 해서 작성합니다.

`onCreateViewHolder() // viewHolder를 만들 때 호출`  
`onBindViewHolder() // ViewHolder와 data를 bind 할 때 호출`  
`getItemCount() // 데이터 세트 크기를 가져올 때 호출`

이 때 `ListAdapter`와 같은 선택지가 존재하며 `diffUtil`을 추가적으로 구현해 재활용 성능을 높일 수 있습니다. 

### References
- https://jcchu.medium.com/viewholder-pattern에-대해-알아보기-9838e66ea9a5