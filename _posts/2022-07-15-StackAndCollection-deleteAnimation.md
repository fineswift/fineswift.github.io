---
layout: single
title:  "CollectionView와 StackView 데이터 삭제"
excerpt: "CollectionView와 StackView의 삭제 동작을 비교해 보겠습니다."

categories:
  - iOS-devalop
# tags: [iOS-devalop, StackView, CollectionView]

author_profile: true
sidebar:
  nav: "docs"
toc: true
toc_sticky: true
toc_label: "목차"
---
안녕하세요. 이번엔 UICollectionView와 StackView의 데이터 삭제 동작을 비교해 보려고 합니다.  
최근에 일하면서 좌우 스크롤이 되는 리스트에서 아이템 선택 시 해당 데이터가 사라지는 기능을 작업했었는데요.  
컬렉션뷰와 스택뷰 2개를 가지고 테스트를 해봤었는데 공유해 보려고 합니다.
<br><br>
# CollectionView 구현
우선 SnapKit, Then라이브러리를 이용하여 구현했습니다.

```swift
/// 컬렉션뷰에 보여줄 아이템 리스트
var collectionTextList = ["item 1", "item 2", "item 3", "item 4", "item 5", "item 6", "item 7", "item 8", "item 9", "item 10"]

lazy var collectionView = UICollectionView(frame: .zero, collectionViewLayout: UICollectionViewFlowLayout().then {
  $0.scrollDirection = .horizontal
  $0.itemSize = CGSize(width: 68, height: 100)
  }).then {
    $0.register(CollectionCell.self, forCellWithReuseIdentifier: CollectionCell.reuseIdentifier())
    $0.showsHorizontalScrollIndicator = false
    $0.delegate = self
    $0.dataSource = self
}
```
컬렉션뷰 선언 부분인데요. 컬렉션뷰에 넣어줄 데이터와 UICollectionViewFlowLayout()를 이용해 가로 스크롤과 아이템 사이즈를 정적으로 지정해 줬습니다.  

```swift
extension ViewController: UICollectionViewDelegate, UICollectionViewDataSource {
  func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
    /// 보여줄 아이템 갯수
    return collectionTextList.count
  }
  
  func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
    /// 셀 생성 및 데이터 삽입
    if let cell = collectionView.dequeueReusableCell(withReuseIdentifier: CollectionCell.reuseIdentifier(), for: indexPath) as? CollectionCell {
      cell.configure(text: collectionTextList[indexPath.row])
      return cell
    }
    return UICollectionViewCell()
  }
    
  func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
    /// 선택한 셀 삭제 및 아이템리스트 삭제
    collectionTextList.remove(at: indexPath.item)
    collectionView.deleteItems(at: [IndexPath(item: indexPath.row, section: 0)])
  }
}
```
컬렉션뷰 델리게이트와 데이터소스를 사용하여 작업 하였고 didSelectItemAt 메소드에서 deleteItems메소드를 사용하였습니다.

셀 파일은 UILabel 하나만 있어 따로 첨부하지 않겠습니다.
<br><br>

## CollectionView 삭제 동작
---
![ezgif com-gif-maker](https://user-images.githubusercontent.com/60169777/179165496-01b1544b-d3ac-461c-b32d-75506c51efc8.gif)

컬렉션뷰의 deleteItems이용해 셀을 삭제할 경우 기본적으로 애니메이션 효과가 들어가 있는 걸 보실 수 있습니다.
<br><br>
# StackView 구현
스택뷰의 경우 좌우 스크롤 기능이 없기 때문에 스크롤뷰로 한번 감싸서 구현했습니다.

```swift
/// 스택뷰에 넣어줄 아이템 리스트
var stackTextList = ["item 1", "item 2", "item 3", "item 4", "item 5", "item 6", "item 7", "item 8", "item 9", "item 10"]

let container = UIView()

lazy var scrollView = UIScrollView().then {
  $0.alwaysBounceVertical = false
  $0.alwaysBounceHorizontal = false
  $0.showsHorizontalScrollIndicator = false
  $0.isScrollEnabled = true
}

lazy var stackView = UIStackView().then {
  $0.axis = .horizontal
  $0.alignment = .center
  $0.distribution = .fillProportionally
  $0.spacing = 5
}

/// setupLayout with SnapKit
view.addSubview(container)
container.snp.makeConstraints {
  $0.top.equalTo(stackLabel.snp.bottom).offset(5)
  $0.leading.trailing.equalToSuperview()
  $0.height.equalTo(100)
}

container.addSubview(scrollView)
scrollView.snp.makeConstraints {
  $0.edges.equalToSuperview()
}

scrollView.addSubview(stackView)
stackView.snp.makeConstraints {
  $0.edges.equalToSuperview()
}
```
- containerView( scrollView( stackView ) ) 구조로 레이아웃 배치
- 스택뷰의 아이템 개수가 많아 가로 길이가 길 경우 스크롤뷰가 스크롤 되면서 좌우 스크롤
- 개수가 적으면 스크롤 X

이렇게 레이아웃 작업까지 끝난 후 스택뷰에 아이템을 넣어줘야 합니다. 컬렉션뷰의 경우 델리게이트를 이용했지만 스택뷰의 경우 저는 addArrangedSubview를 이용하여 구현했습니다.

```swift
/// setupLayout 후 호출
func setupStackView() {
  // 초기에 설정한 아이템 갯수만큼 반복
  stackTextList.forEach { text in
    // 스택뷰에 넣어줄 뷰(안에 버튼 있음) 생성
    let view = StackItemView()
    
    view.itemButton.setTitle(text, for: .normal)
    view.setupConstraints()

    // 스택뷰에 아이템 세팅
    stackView.addArrangedSubview(view)
    view.itemButton.addTarget(self, action: #selector(buttonTap), for: .touchUpInside)
  }
}

/// 스택뷰 아이템 삭제 이벤트
@objc func buttonTap(sender: UIButton) {
  guard let selectText = sender.titleLabel?.text else { return }
  for (index, item) in stackTextList.enumerated() {
    if selectText == item {
      // index로 접근해서 스택뷰 아이템 삭제, 애니메이션 효과 추가
      UIView.animate(withDuration: 0.3, animations: {
        self.stackView.arrangedSubviews[index].removeFromSuperview()
        self.view.layoutIfNeeded()
      })
      stackTextList.remove(at: index)
    }
  }
}
```
- 반복문을 이용하여 데이터 개수만큼 뷰를 생성 후 버튼의 타이틀 및 TargetAction 이벤트 등록
- 초기 데이터배열의 값과 선택된 버튼 타이틀을 비교하여 index 값으로 스택 서브뷰 삭제
- 애니메이션 효과를 주기 위해 UIView.animate 사용
<br><br>

## StackView 삭제 동작
---
![ezgif com-gif-maker (1)](https://user-images.githubusercontent.com/60169777/179180587-d3ded4b1-1c4f-4aab-a587-235856222eb9.gif)

애니메이션 효과 차이 말고는 컬렉션뷰와 동일하게 동작합니다.

# 마무리
컬렉션뷰와 스택뷰를 가로 스크롤 하는 법과 아이템 삭제에 대해서 알아봤는데요.  
컬렉션뷰의 경우 셀을 따로 만들어야 하고, 레이아웃 잡아주고, 델리게이트 및 데이터소스 채택하고...
개인적으로 간단한 기능을 위해 만들 때는 스택뷰로 구현하는 게 편한 거 같습니다.