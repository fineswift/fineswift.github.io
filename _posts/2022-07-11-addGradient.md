---
layout: single
title:  "View에 그라데이션 효과 주기"
excerpt: "Swift 꿀팁"

categories:
  - Extension
# tags: [Swift, Extension]

author_profile: true
sidebar:
  nav: "docs"
---

안녕하세요. 이번엔 UIView에 Gradient 적용하는 법을 알아보겠습니다.

작업을 할 때 그라데이션 배경 효과를 이미지로 넣을 수도 있지만 코드로 직접 추가하여 하는 경우도 생기는데요.

저 같은 경우 CAGradientLayer를 이용해 작업하고 있습니다.

## CAGradientLayer
- 배경색 위에 색상 그라데이션을 그려 레이어의 모양을 채우는 레이어

설명대로 레이어를 그라데이션으로 채울 수 있게 해주는 CALayer입니다. 특이사항으로는 둥근 모서리도 포함해서 그라데이션을 채워 준다는 점입니다.

그럼 먼저 코드를 보여드리겠습니다.

```swift
extension UIView {
  func addGradient(colors: [CGColor], locations: [NSNumber] = [0.0, 1.0], startPoint: CGPoint = CGPoint(x: 0.5, y: 0.0), endPoint: CGPoint = CGPoint(x: 0.5, y: 1.0)) {
    let gradient = CAGradientLayer()
    gradient.frame.size = self.frame.size
    gradient.colors = colors
    gradient.locations = locations
    gradient.startPoint = startPoint
    gradient.endPoint = endPoint
    self.layer.insertSublayer(gradient, at: 0)
    }
}
```
swift 파일을 만든 뒤 UIView를 extension 해서 메소드 형식으로 만들어놓고 사용합니다.

우선 간단하게 코드 내용을 설명하자면 CAGradientLayer인스턴스 생성 후 필요한 속성들을 정의해준 뒤 해당 View에 layer를 추가하는 방식입니다.

메소드의 파라미터로 받는 값들은
1. colors: 그라데이션 효과를 줄 색상 배열입니다.
2. locations: 각 색상이 정지하는 지점으로 0 ~ 1.0 범위를 가지고 있습니다.
    - 특징으로는 반드시 증가하는 형태로 값을 설정해야 한다는 점인데요 [1.0, 0.5, 0.0] 이렇게 하면 안 되고 [0.0, 0.5, 1.0] 이런 식으로 설정해 줘야 합니다.
    - 이 값을 주지 않을 수도 있는데 주지 않으면 색상이 균일하게 분포하게 됩니다.
3. startPoint: 그라데이션 효과의 시작 좌표로
4. endPoint: 그라데이션 효과의 끝 좌표

startPint와 endPoint는 각각 기본값이 CGPoint(x: 0.5, y: 0.0), CGPoint(x: 0.5, y: 1.0)로 설정되어 있어 건드리지 않아도 됩니다. 이럴 경우 그라데이션 효과가 위에서 아래로 효과가 적용되는데 다른 방향으로 주고 싶으시면 좌푯값을 따로 설정해 주시면 됩니다.
![image](https://user-images.githubusercontent.com/60169777/178217216-454dc58c-de85-4d74-8ec6-66d85cfe51d2.png)
- 출처: https://www.swiftdevcenter.com/how-to-create-gradient-color-using-cagradientlayer/

위 이미지를 보시면 화면에 대한 좌푯값이 설명되어 있는데 원하시는 방향으로 좌표를 설정해 주시면 됩니다.

```swift
/// 그라데이션 뷰 모서리 둥글게
lazy var gradientView = UIView().then {
  $0.layer.cornerRadius = 10
  $0.layer.masksToBounds = true
}

// 사용법
// locations, startPoint, endPoint 생략 가능
gradientView.addGradient(colors: [UIColor.red.cgColor, UIColor.blue.cgColor, UIColor.green.cgColor], locations: [0.0, 0.5, 1.0], startPoint: CGPoint(x: 0.0, y: 0.5), endPoint: CGPoint(x: 1.0, y: 0.5))
```

위 예제 코드는 빨강, 파랑, 초록색을 왼쪽에서 오른쪽으로 그라데이션을 준 코드인데요 빌드해서 확인해보면  
![스크린샷 2022-07-11 오후 5 42 53](https://user-images.githubusercontent.com/60169777/178224455-f6fcb733-0cb4-48e6-8c39-3977a944ef42.png)

이렇게 그라데이션이 적용된 걸 보실 수 있습니다.

마지막으로 중요 포인트가 있는데요 .addGradient를 호출하는 시점을 잘 생각하셔야 합니다.  
뷰가 생성되고 레이아웃이 완전히 자리 잡고 frame 크기가 정해지기 전에 호출하게 되면 그라데이션 효과가 적용되지 않습니다!!
예를 들면 UIView에서 사용하실 경우 override func layoutSubviews(),
뷰컨에서 사용하실 경우 override func viewDidLayoutSubviews()에서 호출해 주셔야 그라데이션 레이어가 정상적으로 추가된 모습을 보실 수 있습니다.