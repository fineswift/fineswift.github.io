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
최근에 일을 하면서 좌우 스크롤이 되는 리스트에서 아이템 선택 시 해당 데이터가 사라지는 기능을 작업했었는데요.  
콜렉션뷰와 스택뷰 2개를 가지고 테스트를 해봤었는데 공유해 보려고 합니다.

# CollectionView 구현
우선 SnapKit, Then라이브러리를 이용하여 구현했습니다.

```swift
/// 콜렉션뷰에 보여줄 아이템 리스트
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
콜렉션뷰 선언 부분인데요. 콜렉션 뷰에 넣어줄 데이터와 UICollectionViewFlowLayout()를 이용해 가로 스크롤과 아이템 사이즈를 정적으로 지정해 줬습니다.  

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
콜렉션뷰 델리게이트와 데이터소스를 사용하여 작업 하였고 didSelectItemAt 메소드에서 deleteItems메소드를 사용하였습니다.

셀 파일은 UILabel 하나만 있어 따로 첨부하지 않겠습니다.

## 삭제 동작
---
