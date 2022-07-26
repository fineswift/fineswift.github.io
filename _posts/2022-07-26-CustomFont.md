---
layout: single
title:  "커스텀 폰트 적용하기"
excerpt: "Noto Sans 폰트를 적용해 보겠습니다."

categories:
  - iOS-devalop
# tags: [iOS-devalop, UIFont]

author_profile: true
sidebar:
  nav: "docs"
toc: true
toc_sticky: true
toc_label: "목차"
---
안녕하세요. 이번에는 커스텀 폰트인 Noto Sans 폰트를  
프로젝트에 적용해보고 사용하는 법에 대해 알아보겠습니다.  

# 폰트 다운로드
---
우선 폰트 파일을 다운로드받으셔야 하는데요. [이곳](https://fonts.google.com/noto/specimen/Noto+Sans+KR) 에서 다운로드가 가능합니다.  

근데 제가 이번에 적용할 건 **NotoSansKR-Hestia**라는 폰트로  
자주 사용되는 글자만 뽑아서 용량을 줄였다고 합니다.  

**NotoSansKR-Hestia**는 [깃허브](https://github.com/theeluwin/NotoSansKR-Hestia) 주소로 가시면 보실 수 있을 겁니다.  
저는 여러 폰트 중에 Regular, Medium, Bold 3가지 폰트를 프로젝트에 적용해 보겠습니다.
<br><br>

# 프로젝트 적용 과정
## 폰트 파일 추가
1. 폰트를 다운로드받으신 후 프로젝트 디렉터리에 넣어줍니다.  
![Ag](https://user-images.githubusercontent.com/60169777/180959372-19bfd324-531c-4088-b1b7-8923798c8b64.png)

2. 폰트 파일들을 드래그해서 프로젝트에 추가해주면 보이는 화면입니다. Add to targets에서 적용할 프로젝트인 CustomFont를 체크를 해줍니다.
![Choose options for adding these files](https://user-images.githubusercontent.com/60169777/180960713-177cc692-f9f0-4db7-95f4-3e1bffb06530.png)

3. 프로젝트에 파일이 추가되고 오른쪽 Target Membership에서 프로젝트 체크되어 있는지 확인해 주세요.
![ABCDEFGHIJKLM](https://user-images.githubusercontent.com/60169777/180960862-b5dfe1d2-add8-42cf-9bc0-b1907b35b07a.png)

4. 그리고 Copy Bundle Resources에 폰트가 추가되어 있는지 확인해 주세요.
![스크린샷 2022-07-26 오전 10 53 29](https://user-images.githubusercontent.com/60169777/180960978-94c8cb23-db4f-4663-80f5-93e7b408c3e4.png)

## info.plist 설정
파일을 추가한 후 info 파일에 설정을 추가해 줘야 합니다.
![CustomFontTests](https://user-images.githubusercontent.com/60169777/180964119-d1c4fa20-f5a1-4111-9584-baa331c9a74b.png)

1. info 파일에서 Information Property List에 커서를 가져가면 +버튼이 생기는데 누르면
![Information Property List](https://user-images.githubusercontent.com/60169777/180964280-4fa20491-4eee-47e8-84e5-589c215dcb75.png)  
이렇게 목록이 추가되면서 목록들이 나옵니다.

2. 목록 중에 **Fonts provided by application**을 선택한 후 추가한 폰트의 이름을 추가하시면 됩니다.
![formation Property List](https://user-images.githubusercontent.com/60169777/180964302-bfe345a4-3ce8-4b01-bca5-71440c9461d9.png)  
주의할 점은 파일의 확장자 이름까지 써주셔야 합니다!


# 사용 예제
---
기존에 있는 시스템 폰트와 커스텀 폰트의 차이를 보기 위해 6개의 UILabel을 스토리보드로 추가한 후 viewDidLoad에서 코드로 폰트를 적용해 주었습니다.

```swift
class ViewController: UIViewController {
  @IBOutlet weak var customFontRegular: UILabel!
  @IBOutlet weak var customFontMedium: UILabel!
  @IBOutlet weak var customFontBold: UILabel!
    
  override func viewDidLoad() {
    super.viewDidLoad()
    customFontRegular.text = "커스텀 폰트 레귤러"
    customFontRegular.font = UIFont(name: "NotoSansKR-Regular", size: 20)

    customFontMedium.text = "커스텀 폰트 미디엄"
    customFontMedium.font = UIFont(name: "NotoSansKR-Medium", size: 20)

    customFontBold.text = "커스텀 폰트 볼드"
    customFontBold.font = UIFont(name: "NotoSansKR-Bold", size: 20)
  }
}
```
보시면 오른쪽에 폰트가 적용된 걸 보실 수 있습니다.

<img width="457" alt="스크린샷 2022-07-26 오전 11 36 57" src="https://user-images.githubusercontent.com/60169777/180964325-cd227a8e-cfc9-4c26-829c-0ee4e5bc6451.png">

**그런데 폰트를 적용할 때마다 UIFont(name: ,size:)를 해줘야 하면 불편하기 때문에**  
**저 같은 경우 extension을 이용해서 작업을 합니다.**  

# 커스텀 폰트 적용 팁
---
swift 파일을 따로 만들어줍니다.
![스크린샷 2022-07-26 오전 11 40 25](https://user-images.githubusercontent.com/60169777/180964344-b70a5212-10c5-4a78-8a6e-c668fc0d62fd.png)

<br>

그리고 파일 안에 아래와 같이 코드를 작성합니다.  
```swift
import UIKit

extension UIFont {
  enum Style: String {
    case regular = "NotoSansKR-Regular"
    case medium = "NotoSansKR-Medium"
    case bold = "NotoSansKR-Bold"
  }
    
  static func notoSans(_ style: Style = .regular, size: CGFloat) -> UIFont {
    return UIFont(name: style.rawValue, size: size)!
  }
}
```
- enum을 이용해 폰트 이름을 저장합니다.
- 메소드를 구현하는데 인자값으로 사용할 폰트 스타일과 사이즈를 입력받아 폰트를 적용하는 메소드입니다.
이후 기존에 폰트를 적용했던 코드를 다음과 같이 작성할 수 있습니다.

```swift
customFontRegular.text = "커스텀 폰트 레귤러"
// style 초기값이 .regular이기 때문에 생략 가능
customFontRegular.font = .notoSans(size: 20)

customFontMedium.text = "커스텀 폰트 미디엄"
// UIFont 생략 가능
customFontMedium.font = .notoSans(.medium, size: 20)

customFontBold.text = "커스텀 폰트 볼드"
customFontBold.font = UIFont.notoSans(.bold, size: 20)
```
이렇게 3가지 방법으로 폰트 적용이 가능합니다.

# 마무리
---
enum으로 정의한 폰트 이름을 작성하실 때 파일 이름이 아닌 PostScript 이름으로 적어줘야 하는데요.  
FontFamily 또는 Mac 서체 관리자에서 확인이 가능합니다.  
지금 당장 사용하실 일이 없더라도 저 같은 경우 실제 프로젝트를 진행하면서 기본 폰트를 이용한 적이 없었습니다.  
떄문에 폰트 적용하는 법에 대해 알고 계시면 도움이 많이 될 거 같아서 글을 작성해 봤습니다.