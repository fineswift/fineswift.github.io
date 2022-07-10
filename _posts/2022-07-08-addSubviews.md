---
title:  "코드로 View 작업 시 한번에 addSubview하기"
excerpt: "Swift 꿀팁"

categories:
  - iOS
tags:
  - iOS

date: 2020-07-07
---

안녕하세요. 이번엔 iOS 개발을 하면서 사용하기 좋은 꿀팁하나 가져와봤습니다.

저는 프로젝트 작업 시 필요하거나 공통으로 쓰이는 기능들을 extension해서 swift파일별로 만들어 작업하는 편입니다.

Ex)  
<img width="294" alt="Extension" src="https://user-images.githubusercontent.com/60169777/177739879-7a2a1163-7582-4d4e-9080-9fe6ead147e6.png">

프로젝트를 진행할 때 코드로 작업을 많이 하는데 View마다 addSubview를 사용하게 되면
중복되는 부분도 많아지고 전체적인 코드 길이도 늘어나면서 가독성이 떨어집니다.  
그래서 UIview를 estension해서 addSubviews라는 메소드를 만들어 놓고 사용합니다. 위에 보이는 UIView+ 파일 안에는
```swift
import UIKit

extension UIView {
    func addSubviews(_ views: [UIView]) {
        _ = views.map { self.addSubview($0) }
    }
}
```
이런식으로 작성이 되어 있습니다.

addSubviews를 사용하게되면 각 View별로 addSubview해주던 것을 한줄로 처리가 가능해 집니다.

```swift
let parentView = UIView()
let subView = UIView()
let subViewButton = UIButton()
let subLabel = UILabel()
let subImageView = UIImageView()
```
가 있다고 가정하면 기존에는
```swift
parentView.addSubview(subView)
parentView.addSubview(subViewButton)
parentView.addSubview(subLabel)
parentView.addSubview(subImageView)
```
이렇게 길어지는데 addSubviews를 사용하면
```swift
parentView.addSubviews([subView, subViewButton, subLabel, subImageview])
```

간단하게 표현할 수 있습니다.

추가로 필요한 extension기능이 있으면 추가로 작성해서 사용하면 됩니다.