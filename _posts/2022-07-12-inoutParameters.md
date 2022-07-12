---
layout: single
title:  "inout 키워드에 대해"
excerpt: "함수 인자값중 하나인 inout 파라미터에 대해 알아보겠습니다."

categories:
  - Extension
# tags: [Extension, Swift]

author_profile: true
sidebar:
  nav: "docs"
---
안녕하세요. 이번엔 inout 키워드에 대해 알아보겠습니다.

Swift 함수의 파라미터 종류 중 하나입니다.  
Swift에서 함수의 파라미터는 상수로 파라미터의 값을 함수 내에서 변경하려고 한다면 에러가 발생하는데요.

만약 정수값을 받아서 + 10을 해주는 함수가 있다고 한다면
```swift
func addTen(_ value: Int) -> Int {
  return value + 10
}
```
이렇게 반환 값이 있는 함수로 작성하는데요.<br><br>

새 값을 반환하지 않고 값을 직접 변환하고 싶어서 
```swift
func addTen(_ value: Int) {
  // Cannot assign to value: 'value' is a 'let' constant
  value += 10
}
```
이렇게 해봤지만 주석에 쓰여진 에러가 발생하게 됩니다.<br><br>

함수 파라미터의 값을 직접 변경하고 싶다면 inout키워드를 사용하면 됩니다.  
파라미터 타입 앞부분에 inout 키워드를 넣어주면 되는데요.
```swift
func addTen(_ value: inout Int) {
  value += 10
}
```
이런 식으로 함수를 작성하시면 됩니다.  
이렇게 되면 새 값을 만들지 않고 인자값을 직접 변경하고 변경 사항이 저장되게 되는데요.
```swift
var num = 10

func addTen(_ value: inout Int) {
  value += 10
  print(value)
}

addTem(&num)
// 20
```


이렇게 사용하게 되면 num의 값이 20으로 변경됩니다.  
inout 파라미터를 사용할 때는 함수 외부의 값이 변경되므로 상수나 리터럴 값을 사용할 수 없습니다.  
그리고 함수 호출 시 인자값이 함수에 의해 수정될 수 있음을 나타내기 위해 &(앰퍼샌드)를 붙여줘야 합니다.

### inout 원리
1. 함수가 호출되면 인수 값이 전달됩니다.
2. 함수 본문에서 복사본이 수정됩니다.
3. 함수가 반환되면 복사본의 값이 원래 인수에 할당됩니다.
<br><br>

이 동작을 copy-in copy-out 또는 call by value result라고 불리는데
값을 복사해서 수정하고 다시 원래 값에 복사한다고 볼 수 있습니다.  
또 특징으로는 최적화를 위해 인자 값이 메모리의 물리적 주소에 저장된 값인경우 함수 내부와 외부에서 동일한 메모리 위치가 사용된다고 합니다.
> 출처: <https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#ID545>

이 부분은 이해가 어렴풋이 되어 글로 정리 하려니 힘드네요.. 아직 많이 부족한거 같습니다.  
혹시 잘 알고 있으신 분은 피드백 주시면 감사하겠습니다:)
