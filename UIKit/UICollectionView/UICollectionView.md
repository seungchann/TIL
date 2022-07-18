# UICollectionView  
> 정렬된 data의 collection을 관리하고 customizable layout을 사용해서 collection을 보여주는 object  

## A. Declaration  
```swift
class UICollectionView : UIScrollView
```

## B. Overview 

### dataSource  
* Collection view를 위한 data를 제공하는 object  
* Collection view data source는 `UICollectionViewDataSource`를 따라야만 한다.  
* 커스텀한 data source를 사용하고 싶다면, `UICollectionView`의 다른 method들을 사용해서 data를 add, delete, rearrange 할 수 있다.

### [UICollectionViewDataSource](https://developer.apple.com/documentation/uikit/uicollectionviewdatasource)
* 이미 이 protocol을 따르고 있는 [UICollectionViewDiffableDataSource](https://developer.apple.com/documentation/uikit/uicollectionviewdiffabledatasource)를 data source object로 사용해도 좋다.
* 하지만 data source object를 custom하고 싶다면 최소한 2개의 method는 반드시 구현해야한다.  
  * `collectionView(_:numberOfItemsInSection:)`: collection view의 item의 개수  
  * `collectionView(_:cellForItemAt:)` : 주어진 item의 cell을 creating, configuring and returning할 때 사용  

```swift
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        collectionView.delegate = self
        collectionView.datasource = self
    }
}

extension viewController: UICollectionViewDataSource {

    // 필수 구현 메서드
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int

    // 필수 구현 메서드
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell

    ...
}
```

#### [collectionView(_:cellForItemAt:)](https://developer.apple.com/documentation/uikit/uicollectionviewdatasource/1618029-collectionview)  
* cell의 reuse identifier를 parameter로 사용한다면 `dequeueReusableCell(withReuseIdentifier:for:)`도 사용할 수 있다.  

```swift
func collectionView(_ collectionView: UICollectionView, 
     cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
     let cell = collectionView.dequeueReusableCell(withReuseIdentifier: CustomCollectionViewCell.reuseID, for: indexPath)
          
     return cell
}
```

```swift
func dequeueReusableCell(withReuseIdentifier identifier: String, 
                     for indexPath: IndexPath) -> UICollectionViewCell{
    // register의 내용에따라 init(frame:)으로 새로운 셀을 생성하거나
    
    // 재사용가능한 셀이 있을경우에는 prepareForReuse를 호출한다.
    // prepareForReuse는 셀의 프로퍼티를 모두 초기화하고 재사용할 준비를 시키는 메서드다.
    
    // 그후 셀을 반환해준다
}
```

#### `UICollectionViewDataSource`의 다른 method들
```swift
// # 아이템과 섹션 기준에 관한 메서드

// * 필수
// 섹션의 아이템 수를 결정한다.
func collectionView(_ collectionView: UICollectionView, 
numberOfItemsInSection section: Int) -> Int

// collection view의 section의 개수를 결정한다.
optional func numberOfSections(in collectionView: UICollectionView) -> Int

// # 아이템의 뷰에 관한 메서드

// * 필수
// 아이템과 각각 매칭되는 셀의 configure를 위한 메서드
func collectionView(_ collectionView: UICollectionView, 
      cellForItemAt indexPath: IndexPath) -> UICollectionViewCell

// supplementary view를 제공하기 위한 메서드
optional func collectionView(_ collectionView: UICollectionView, 
viewForSupplementaryElementOfKind kind: String, 
                          at indexPath: IndexPath) -> UICollectionReusableView

// # 아이템 재배치에 관한 메서드

// 특정 아이템이 collection view의 다른 위치로 이동할 수 있는지에 관한 메서드
optional func collectionView(_ collectionView: UICollectionView, 
               canMoveItemAt indexPath: IndexPath) -> Bool

// 특정 아이템을 새로운 위치로 움직인다고 알려주는 메서드
optional func collectionView(_ collectionView: UICollectionView, 
                  moveItemAt sourceIndexPath: IndexPath, 
                          to destinationIndexPath: IndexPath)

// # 인덱스 configuring에 관한 메서드

// Index item을 위한 title을 return한다. 아래 메서드와 함께 쓰인다.
optional func indexTitles(for collectionView: UICollectionView) -> [String]?

// Index entry와 매칭되는 index path를 return하는 메서드
optional func collectionView(_ collectionView: UICollectionView, 
      indexPathForIndexTitle title: String, 
                          at index: Int) -> IndexPath

// 전화번호부 앱 오른쪽에서 보이는 그 빠른스크롤링을 위한 title indicator를 사용하고 싶다면,
// 두 메서드를 모두 사용해야 한다.

```
***
### [UICollectionViewDiffableDataSource](https://developer.apple.com/documentation/uikit/uicollectionviewdiffabledatasource)  

* Declaration
```swift
class UICollectionViewDiffableDataSource<SectionIdentifierType, ItemIdentifierType> : NSObject where SectionIdentifierType : Hashable, ItemIdentifierType : Hashable
```
* `UICollectionViewDataSource` 프로토콜을 채택해 기존의 메서드도 모두 가지고 있으며, 유동적인 업데이트를 굉장히 간편하게 만들어준다.  
* Fancy한 애니메이션도 제공한다.  
* Collection view에 data를 채우기 위한 방법은 다음과 같다.  
  1. Collection view와 diffable data source를 연결한다.  
  2. Collection view cell을 configure하기 위해서 cell provider를 구현해라.  
  3. 현재 상태의 data를 만들어라.  
  4. Data를 UI에 보여줘라.
> 1번부터 자세히 살펴보면, 이제 굳이 UICollectionViewDataSource 프로토콜을 내 컨트롤뷰에 (확장) 채택해 컬렉션뷰의 dataSource에 연결해 줄 필요가 없다. 대신에 diffableDataSource를 생성할 때 인자로 컬렉션뷰를 넘겨주면 자동으로 dataSource가 연결된다. 그 후 2번과정은 diffableDtaSource의 클로저 내부에서 일어나게 된다.

* Diffable data source를 만들기 위해서는 `init(collectionView:cellProvider:)` 메서드를 사용해라.  
* Cell을 configure할 수 있는 cell provider는 다음과 같이 만들 수 있다.  
```swift
dataSource = UICollectionViewDiffableDataSource<Int, UUID>(collectionView: collectionView) {
    (collectionView: UICollectionView, indexPath: IndexPath, itemIdentifier: UUID) -> UICollectionViewCell? in
    // 셀을 configure하고 return 하는 부분이 이곳으로 들어왔다.
    // 기존처럼 cell을 디큐잉하고 configure한 뒤에 return 해주면 된다.
}
```

> 이제 업데이트 과정인 3,4번을 살펴보면 아래와 같다. `NSDiffableDataSoourceSnapshot`을 이용해 현재 데이터를 기록할 스냅샷을 생성하고, 여기에다가 표현하려는 데이터를 넣어준 후, 데이터소스에 적용하기만 하면된다. 업데이트가 굉장히 간단해졌다. 원한다면 빈 스냅샷 대신에 dataSource.snapshot으로 데이터소스의 스냅샷을 써도 무방하다.

```swift
// 섹션과 현재 데이터를 기록 할 빈 스냅샷을 생성한다.
var snapshot = NSDiffableDataSourceSnapshot<Int, UUID>()        

// 스냅샷에 표시하려는 섹션과 아이템들을 넣어준다.
snapshot.appendSections([0])
snapshot.appendItems([UUID(), UUID(), UUID()])

// 데이터 소스에 만들어진 스냅샷을 적용한다.
dataSource.apply(snapshot, animatingDifferences: true)
```
***
### delegate  
* Collection view의 delegate로 행동하는 object  
* Delegate는 `UICollectionViewDelegate`를 따라야만 한다.  

### [UICollectionViewDelegate](https://developer.apple.com/documentation/uikit/uicollectionviewdelegate)
* Delegate는 각 아이템의 item의 selection behavior나 상호작용을 관리할 때 사용한다.  
* 이 protocol의 메서드는 모두 optional이다.  
* 컬렉션 뷰 안에는 델리게이트 프로퍼티가 선언되어 있다.  
```swift
weak var delegate: UICollectionViewDelegate? { get set }
```

#### `UICollectionViewDelegate`의 다른 method들

    // # 선택된 셀들에 대한 관리
> User에게 cell의 상태와, 각 상태 사이의 transition에 관한 시각적 피드백을 제공한다.  
> {} [Changing the Appearance of Selected and Highlighted Cells](https://developer.apple.com/documentation/uikit/uicollectionviewdelegate/changing_the_appearance_of_selected_and_highlighted_cells)  

> Table에서 multiselect gesture를 사용해서 여러개의 아이템을 선택할 수 있게 해준다.  
> {} [Selecting Multiple Items with a Two-Finger Pan Gesture](https://developer.apple.com/documentation/uikit/uitableviewdelegate/selecting_multiple_items_with_a_two-finger_pan_gesture)  

```swift
// User가 컬렉션 뷰의 아이템을 선택하려고 할 때 이 메서드가 호출된다.
// 설정하지 않으면 디폴트로 모든 아이템은 선택가능하도록 true로 설정된다.
optional func collectionView(_ collectionView: UICollectionView, 
          shouldSelectItemAt indexPath: IndexPath) -> Bool

// 특정한 index path의 아이템이 선택되었다고 알려준다.
// 유저가 성공적으로 아이템을 선택했을때 호출되는 메서드다. 메서드를 구현할 때 아이템이 선택 될 경우의 동작에 대해 처리 할수 있다!
optional func collectionView(_ collectionView: UICollectionView, 
             didSelectItemAt indexPath: IndexPath)

// User가 컬렉션 뷰의 아이템을 선택해제 하려고 시도할 때 호출되는 메서드이다.
// 따로 구현하지 않으면 기본적으로 모든 아이템은 선택해제가 가능하게 true로 설정된다.
optional func collectionView(_ collectionView: UICollectionView, 
        shouldDeselectItemAt indexPath: IndexPath) -> Bool

// User가 성공적으로 컬렉션 뷰의 아이템을 선택해제 했을 때 호출되는 메서드이다.
optional func collectionView(_ collectionView: UICollectionView, 
           didDeselectItemAt indexPath: IndexPath)

// User가 two-finger pan gesture를 사용해서 여러 아이템을 선택할 수 있는지 묻는 메서드이다.
// 시스템이 two-finger pan gesture를 감지하면, 컬렉션뷰의 'isEditing'을 true로 만들기 전에 이 메서드를 호출한다.
// 이 메서드를 구현하지 않을경우, 시스템은 `allowsMultipleSelectionDuringEditing`의 값을 통해서
// User가 pan gesture를 이용해 여러 아이템을 선택할 수 있는지 결정한다.
optional func collectionView(_ collectionView: UICollectionView, 
shouldBeginMultipleSelectionInteractionAt indexPath: IndexPath) -> Bool\

// 이 메서드를 구현하면, 앱의 interface에서 user가 여러 아이템을 선택하고 있다는 걸 나타낼 수 있다.
// 예를 들어, Edit이나 Select button을 Done 버튼으로 바꿀 수 있다.
optional func collectionView(_ collectionView: UICollectionView, 
didBeginMultipleSelectionInteractionAt indexPath: IndexPath)

/* 예시코드
func collectionView(_ collectionView: UICollectionView, 
didBeginMultipleSelectionInteractionAt indexPath: IndexPath) {
    // Replace the Select button with Done, and put the 
    // collection view into editing mode.
    setEditing(true, animated: true)
}
*/

// User가 기기에서 그들의 손가락을 뗄 떼 이 메서드를 호출한다.
optional func collectionViewDidEndMultipleSelectionInteraction(_ collectionView: UICollectionView)

// # Cell Highlighting 관리

// 터치 이벤트가 실행되었을 때, 컬렉션 뷰에서 유저가 선택하려는 아이템(cell)을 highlight할지에 관한 메서드이다.
// programmatically 설정된 것이 아닌, 실제 user 터치에서만 이 메서드가 호출된다.
// 만약 false로 설정되었다면, 시스템은 collectionView(_:shouldSelectItemAt:)이나 selection 관련된 다른 메서드를 호출하지 않는다.
// True라면, isHighlighted가 true로 설정되며, collectionView(_:didHighlightItemAt:)이 호출되면서 선택 process를 시작한다.
// 메서드를 구현하지 않은 경우, default return value는 true이다.
optional func collectionView(_ collectionView: UICollectionView, 
       shouldHighlightItemAt indexPath: IndexPath) -> Bool

// 특정 인덱스 path의 cell이 하이라이트 된 상태일 때 호출된다. 구현에서 행동을 정해줄 수 있다.
optional func collectionView(_ collectionView: UICollectionView, 
          didHighlightItemAt indexPath: IndexPath)

// 특정 인덱스 path에서 하이라이트가 제거되었음을 알려주는 메서드이다.
optional func collectionView(_ collectionView: UICollectionView, 
        didUnhighlightItemAt indexPath: IndexPath)

// # View의 추가와 삭제를 tracking 하는 메서드

// 컬렉션 뷰가 cell을 컨텐츠에 추가하기 전에 이 메서드를 호출한다. Cell 추가를 감지할 때 사용할 수 있다.
optional func collectionView(_ collectionView: UICollectionView, 
                 willDisplay cell: UICollectionViewCell, 
                   forItemAt indexPath: IndexPath)

// 컬렉션 뷰가 supplementary를 컨텐츠에 추가하기 전에 이 메서드를 호출한다. View 추가를 감지할 때 사용할 수 있다.
optional func collectionView(_ collectionView: UICollectionView, 
willDisplaySupplementaryView view: UICollectionReusableView, 
              forElementKind elementKind: String, 
                          at indexPath: IndexPath)

// Cell이 컬렉션 뷰에서 삭제될 때를 감지하기 위해서 이 메서드를 사용한다.
optional func collectionView(_ collectionView: UICollectionView, 
            didEndDisplaying cell: UICollectionViewCell, 
                   forItemAt indexPath: IndexPath)

// 특정 supplementary view가 컬렉션 뷰에서 삭제될 때 이 메서드를 사용한다.
optional func collectionView(_ collectionView: UICollectionView, 
didEndDisplayingSupplementaryView view: UICollectionReusableView, 
            forElementOfKind elementKind: String, 
                          at indexPath: IndexPath)

// # Layout 변화를 다루는 메서드

// Transition 중에 커스텀 UICollectionViewTransitionLayout을 사용하고 싶을 때 사용하는 메서드이다.
// 기본적으로는, direct하게 위치를 이동시키는 animation을 가지고 있다.
// 커스텀하게 되면, non linear path, timing algorithm, incoming touch event에 따른 이동 등을 가능하게 해준다.
optional func collectionView(_ collectionView: UICollectionView, 
transitionLayoutForOldLayout fromLayout: UICollectionViewLayout, 
                   newLayout toLayout: UICollectionViewLayout) -> UICollectionViewTransitionLayout

// Layout 업데이트 중이나 layout 사이의 transition 중에, animation 끝에서 호출된 content의 offset을 변경할 수 있게 해주는 메서드이다.
// Layout object의 targetContentOffset(forProposedContentOffset:) 메서드 다음에 이 메서드가 호출된다.
// 컨텐츠 offset을 수정하기 위해서 너의 layout object를 하위로 두고 싶지 않은 경우에 이 메서드를 사용해라.
optional func collectionView(_ collectionView: UICollectionView, 
targetContentOffsetForProposedContentOffset proposedContentOffset: CGPoint) -> CGPoint

// User가 item을 옮기는 동안에, 원래의 index path가 아닌 다른 index path를 제공하고 싶다면 이 메서드를 사용해라.
// 너는 user가 옳지 않은 장소에 item을 drop하는 걸 막기 위해서 이 메서드를 사용할 수 있다.
optional func collectionView(_ collectionView: UICollectionView, 
targetIndexPathForMoveFromItemAt originalIndexPath: IndexPath, 
         toProposedIndexPath proposedIndexPath: IndexPath) -> IndexPath
```

    // # Context Menu들을 관리하는 메서드  
> iOS app에 context menu들을 추가해서 유용한 action에 빠르게 접근할 수 있도록 해준다.  
> {} [Adding Context Menus in Your App](https://developer.apple.com/documentation/uikit/uicontrol/adding_context_menus_in_your_app)  

```swift
// 특정 point의 아이템의 context menu configuration을 반환하는 메서드이다.
optional func collectionView(_ collectionView: UICollectionView, 
contextMenuConfigurationForItemAt indexPath: IndexPath, 
                       point: CGPoint) -> UIContextMenuConfiguration?

// Context menu가 사라졌을 때 목적지 뷰를 return한다.
// 사라지는 애니메이션을 customize할 때 이 메서드를 사용해라.
optional func collectionView(_ collectionView: UICollectionView, 
previewForDismissingContextMenuWithConfiguration configuration: UIContextMenuConfiguration) -> UITargetedPreview?

// 생성된 collection view의 default preview를 덮어쓰는 view를 return 한다.
optional func collectionView(_ collectionView: UICollectionView, 
previewForHighlightingContextMenuWithConfiguration configuration: UIContextMenuConfiguration) -> UITargetedPreview? 

// Context menu가 나타날 때 호출되는 메서드이다.
optional func collectionView(_ collectionView: UICollectionView, 
      willDisplayContextMenu configuration: UIContextMenuConfiguration, 
                    animator: UIContextMenuInteractionAnimating?)

// Context menu가 사라질 때 호출되는 메서드이다.
optional func collectionView(_ collectionView: UICollectionView, 
willEndContextMenuInteraction configuration: UIContextMenuConfiguration, 
                    animator: UIContextMenuInteractionAnimating?)

// User가 프리뷰를 tap해서 커밋을 trigger했을 때 호출되는 메서드이다.
optional func collectionView(_ collectionView: UICollectionView, 
willPerformPreviewActionForMenuWith configuration: UIContextMenuConfiguration, 
                    animator: UIContextMenuInteractionCommitAnimating)

// # Collection View의 Focus를 관리하는 메서드

// 해당 index path의 아이템이 포커스 될 수 있는지를 결정한다.
// 이 메서드나, cell의 canBecomeFocused 메서드를 사용해서 어떤 item이 focus될지를 결정할 수 있다.
// Focus engine은 canBecomeFocused 메서드를 먼저 호출한다.
// 이 메서드를 구현하지 않을경우, 아이템이 선택가능한지 아닌지에 따라 focus 할 수 있는지가 결정된다.
optional func collectionView(_ collectionView: UICollectionView, 
              canFocusItemAt indexPath: IndexPath) -> Bool

// 포커스가 컬렉션 뷰로 맞춰지려고 할 때, 컬렉션 뷰는 어떤 subview가 포커스를 받아야할 지 반드시 정해야 한다.
// remembersLastFocusedIndexPath 프로퍼티 값이 true일 경우, 컬렉션 뷰는 마지막으로 포커스 됐던 cell의 인덱스 path를 return 한다.
// false일 경우, 전에 포커스 된 cell이 없으므로, 이 메서드를 이용해서 구체적으로 포커스 받을 cell을 지정해주어야 한다.
// 컬렉션 뷰를 subclass했다면, preferredFocusEnvironments 프로퍼티를 overriding해서 구현할수도 있다.
optional func indexPathForPreferredFocusedView(in collectionView: UICollectionView) -> IndexPath?

// 포커스가 다른 아이템으로 변하기 전에, 포커스가 변경될 수 있는 체크하는 메서드이다.
// 구현하지 않으면 컬렉션 뷰는 true라고 가정한다.
// 컬렉션 뷰를 subclass했다면, shouldUpdateFocus(in:) 메서드를 overriding해서 구현할 수도 있다.
optional func collectionView(_ collectionView: UICollectionView, 
         shouldUpdateFocusIn context: UICollectionViewFocusUpdateContext) -> Bool

// 포커스와 관련된 변화가 일어났을 때 호출되는 메서드이다.
// 앱 상태를 업데이트 하거나, 앱의 비주얼적인 면에서 변화를 animate할 때 이 메서드를 사용할 수 있다.
// 컬렉션 뷰를 subclass했다면, didUpdateFocus(in:with:) 메서드를 overriding해서 구현할 수도 있다.
optional func collectionView(_ collectionView: UICollectionView, 
            didUpdateFocusIn context: UICollectionViewFocusUpdateContext, 
                        with coordinator: UIFocusAnimationCoordinator)

// # 아이템 수정과 관련된 메서드

// 아이템이 수정가능한 아이템인지 결정하는 메서드이다. Default 값은 true이다.
optional func collectionView(_ collectionView: UICollectionView, 
               canEditItemAt indexPath: IndexPath) -> Bool

// # Spring-Loading Behavior 컨트롤과 관련된 메서드

// 특정 아이템에서 spring-loading 상호작용 효과를 보여줄지를 결정하는 메서드이다.
// 이 메서드를 구현하지 않는다면, 컬렉션뷰는 true라고 가정한다.
optional func collectionView(_ collectionView: UICollectionView, 
      shouldSpringLoadItemAt indexPath: IndexPath, 
                        with context: UISpringLoadedInteractionContext) -> Bool
```
***
### Layout
> Collection view에서 layout은 content의 정렬 방식을 정한다.  
* Layout object는 [UICollectionViewLayout](https://developer.apple.com/documentation/uikit/uicollectionviewlayout)의 subclass이다.  
* Layout object는 모든 cell과 supplementary view의 구성과 위치를 정한다.  
* Collection view에서 layout 정보를 관련된 view에 적용한다.  
* 그 이유는 cell과 supplementary view를 만들 때 collection view와 data source object 사이의 관계를 포함하기 때문이다.  
* Layout object는 [collectionViewLayout](https://developer.apple.com/documentation/uikit/uicollectionview/1618047-collectionviewlayout) 프로퍼티에 저장되어 있다.  
* 이 프로퍼티를 설정하면, 애니메이션 없이 바로 layout을 업데이트 한다.  
* 만약 애니메이션이 필요하다면 [setCollectionViewLayout(_:animated:completion:)](https://developer.apple.com/documentation/uikit/uicollectionview/1618017-setcollectionviewlayout) 메서드를 사용해라.  
* 제스쳐나 터치로 작동되는 interactive transition을 만들고 싶다면 [startInteractiveTransition(to:completion:)](https://developer.apple.com/documentation/uikit/uicollectionview/1618098-startinteractivetransition) 메서드를 사용해서 layout object를 변경해라.  
* 이 메서드는 중간 layout object를 생성해서, 제스쳐나 이벤트 handling 코드와 함께 transition의 진행을 따라간다.  
* Event-handling code에서 transition이 끝났다는 것을 감지하면, [finishInteractiveTransition()](https://developer.apple.com/documentation/uikit/uicollectionview/1618080-finishinteractivetransition)이나 [cancelInteractiveTransition()](https://developer.apple.com/documentation/uikit/uicollectionview/1618075-cancelinteractivetransition) 메서드를 사용해서 중간 layout object를 삭제하고, intended target layout object를 만든다.  
***
### Cell과 Supplementary Views  
* Collection view의 data source object는 아이템을 위한 컨텐츠와 그 컨텐츠를 보여주는데 사용되는 view를 둘 다 제공한다.  
* Collection view는 먼저 content를 로딩한 다음에, data source에게 아이템을 보여 줄 view를 제공하게 만든다.  
* 이 때 Collection view는 data source가 reuse를 위해서 mark해 둔 view object의 queue나 list를 유지한다.  
* 계속 새로운 view를 만들지 않고, 항상 dequeue view를 사용한다.  
* Dequeueing view를 위해 두 가지 메서드가 있다.  
  1. [dequeueReusableCell(withReuseIdentifier:for:)](https://developer.apple.com/documentation/uikit/uicollectionview/1618063-dequeuereusablecell)를 사용해서 item을 위한 cell 만들기  

```swift
func dequeueReusableCell(withReuseIdentifier identifier: String, 
                     for indexPath: IndexPath) -> UICollectionViewCell
```

  2. [dequeueReusableSupplementaryView(ofKind:withReuseIdentifier:for:)](https://developer.apple.com/documentation/uikit/uicollectionview/1618068-dequeuereusablesupplementaryview)를 사용해서 supplementary view 만들기  
> 추가적인 뷰, 즉 섹션의 헤더나 푸터같은 것들도 똑같다. 위에서 언급했던 옵셔널 메서드를 이용해서 안에서 dequeueing을 진행, configure 후 return 해준다. Cell의 재사용식별자 대신에 추가적인 뷰에 대한 종류를 인자로 받는다.  

```swift
func collectionView(_ collectionView: UICollectionView, 
    viewForSupplementaryElementOfKind kind: String, 
                            at indexPath: IndexPath) -> UICollectionReusableView{
            // 여기서 추가적인 뷰에대한 디큐잉이 일어나고
            // 뷰를 configure해준후
            // return해준다.
    }
```

```swift
func dequeueReusableSupplementaryView(ofKind elementKind: String, 
                  withReuseIdentifier identifier: String, 
                                  for indexPath: IndexPath) -> UICollectionReusableView {

    // register의 내용에따라 init(frame:)으로 새로운 추가뷰를 생성하고
    // 재사용가능한 뷰가 있을경우에는 prepareForReuse를 호출해 해당 뷰를 초기화 시켜 재사용할 준비를한다.
    // 그후 뷰를 반환한다.
}
}
```

* 이러한 메서드를 불러오기 전에, 관련된 view가 존재하지 않을 때 collection view에서 그 view를 만드는 방법을 알려줘야 한다.  
* 이를 위해서, collection view에 class나 nib file을 등록해야만 한다.  

```swift
// class를 이용해 뷰를 등록하는 방법
func register(_ cellClass: AnyClass?, 
forCellWithReuseIdentifier identifier: String)

// nib 파일을 이용해 뷰를 등록하는 방법
func register(_ nib: UINib?, 
forCellWithReuseIdentifier identifier: String)
```

* 등록 과정에서, 너는 view의 목적을 나타낼만한 reuse identifier를 지정해주어야 한다.  
* 나중에 view를 dequeueing할 때도 그 identifier가 사용된다.  
* Data source 메서드에서 적절한 view를 dequeueing한 후에, 컨텐츠를 configure하고 컬렉션 뷰로 return 시켜라.  
* Layout object로부터 layout 정보를 얻은 후에, collection view는 view에 적용시키고 나타낼 것이다.  
***

### Data Prefetching  
> Collection view는 반응성을 높이기 위해서 두가지 prefetching 기술을 제공한다.  
* Cell prefetching: 미리 cell을 준비해놓는다. Collection view가 동시에 많은 양의 cell을 요구하면, 화면에 나타나기 전에 미리 cell을 준비하기 때문에 화면이 부드럽게 스크롤 될 수 있다. (Default)  
* Data prefetching: Cell에 표시할 데이터를 미리 준비해놓는다. Cell의 컨텐츠가 네트워크 request 같이 데이터 로딩이 오래걸리는 프로세스에 의존하는 경우에 유용하다. [UICollectionViewDataSourcePrefetching](https://developer.apple.com/documentation/uikit/uicollectionviewdatasourceprefetching) 프로토콜을 채택하는 object를 [prefetchDataSource](https://developer.apple.com/documentation/uikit/uicollectionview/1771768-prefetchdatasource)프로퍼티에 할당시켜서, cell의 데이터가 언제 prefetch 될지 알림을 받을 수 있다.  
***

### Reordering Items Interactively  
> 컬렉션 뷰는 user interaction에 따라 아이템을 옮길 수 있다. 원래 아이템의 순서는 data source에 의해서 정해진다.  
> User들이 아이템 순서를 바꿀 수 있게 만들기 위해, user interaction을 track하는 gesture recognizer를 설정해서, 아이템을 업데이트 할 수 있다.  

* Interactive한 아이템 재배치를 위해서, [beginInteractiveMovementForItem(at:)](https://developer.apple.com/documentation/uikit/uicollectionview/1618019-begininteractivemovementforitem) 메서드를 호출해라.  
* Gesture recognizer가 터치 이벤트를 트래킹하는동안, touch location의 변화를 report하기 위해서 [updateInteractiveMovementTargetPosition(_:)](https://developer.apple.com/documentation/uikit/uicollectionview/1618079-updateinteractivemovementtargetp) 메서드를 호출해라.  
* 제스처 트래킹이 끝나면, [endInteractiveMovement()](https://developer.apple.com/documentation/uikit/uicollectionview/1618082-endinteractivemovement)나 [cancelInteractiveMovement()](https://developer.apple.com/documentation/uikit/uicollectionview/1618076-cancelinteractivemovement) 메서드를 이용해서 interaction을 끝내고, 컬렉션 뷰를 업데이트해라.  
* User interaction 동안에, 컬렉션 뷰는 자기 layout을 무효화하고, dynamic하게 현재 아이템의 위치를 반영한다.  
* 아무 행동도 하지 않는다면, default layout behavior가 아이템 위치를 되돌려놓겠지만, layout animation을 원할 때 커스텀할 수 있다.  
* Interaction이 끝나면, collection view는 data source object를 새로운 아이템 위치로 업데이트한다.  
* [UICollectionViewController](https://developer.apple.com/documentation/uikit/uicollectionviewcontroller) 클래스에서 default gesture recognizer를 제공하며, install하기 위해서는 [installsStandardGestureForInteractiveMovement](https://developer.apple.com/documentation/uikit/uicollectionviewcontroller/1623979-installsstandardgestureforintera) 프로퍼티를 true로 바꿔라.  

> 하나씩 살펴보면 각각 아래와 같다.  
```swift
class UICollectionView : UIScrollView {
    ...
    // 특정 인덱스 패스의 아이템에 대한 상호작용을 초기화한다.
    // 같은 컬렉션뷰내에서 아이템을 새로운 위치로 옮기는 상호작용을 시작할때 호출한다. 
    // delegate로 가서 아이템을 움직이는게 가능한지 확인받고 만일 옮기기가 불가능하면 false를 가능하면 true를 반환한다. 
    // 호출하면 반드시 endIntractiveMovement()나 cancelInteractiveMovement로 컬렉션뷰에게 움직임이 끝남음을 알려줘야한다.
    func beginInteractiveMovementForItem(at indexPath: IndexPath) -> Bool
    
    // 컬렉션 뷰내에 존재하는 아이탬의 위치를 업데이트한다.????
    // 제스처인식기가 현재 손의 위치를 알려주면 매순간 아래 메서드를 이용해 아이템의 위치를 업데이트 해줘야한다.
    // 컬렉션뷰는 delegate를 통해 위치 업데이트가 문제가 없는지 또 레이아웃의 업데이트가 필요한지를 체크한다
    // 포지션이 변할때마다 컬렉션뷰는 변화를 
    // collectionView(_:targetIndexPathForMoveFromItemAt:toProposedIndexPath:)를 호출해 delegate에게 보고한다. 
    func updateInteractiveMovementTargetPosition(_ targetPosition: CGPoint)
    
    // 사용자가 성공적으로 움직임을 끝냈다고 생각했을때 호출해야하는 메서드이다.
    // 아래 메서드를 호출하면 컬렉션뷰는 트래킹을 종료하고 아이템을 새로운 위치로 영구적으로 움직인다. 
    // 컬렉션뷰가 움직임을 영속시키기 위해 dataSource의 collectionView(_:moveItemAt:to:)를 호출해 
    // dataSource의 아이템순서를 바꿔버린다.
    func endInteractiveMovement()
    
    // 사용자가 상호작용을 취소했을떄 호출해아하는 메서드이다.
    // 호출되면 움직임 트래킹을 취소하고 아이탬을 원래의 자리에 돌려놓는다. 
    // 만일 제스처 인식기로 움직임을 쫓던중 아래 메소드를 호출하면,
    // 컬렉션뷰는 트래킹 과정이 끝났다고 생각해 트래킹을 중단하고 아이템을 원래위치로 돌려놓게 된다.
    func cancelInteractiveMovement()
    ...
}
```
***

### Interface Builder Attributes  
> 아래의 표는 Interface Builder에서 collection views와 관련된 attribute list이다.  

| **Attribute** | **Description** |
| :------- | :------- |
| Items | Prototype cells의 수이다. 이 property는 스토리보드에서 설정할 수 있는 prototype cell의 개수를 컨트롤 할 수 있다. Collection view는 최소한 한 개 이상의 cell을 가져야 한다. 여러 cell을 사용해서 다른 타입의 컨텐츠를 보여주거나, 같은 컨텐츠를 다른 방식으로 보여줄 수 있다. |
| Layout | 사용할 layout object이다. 이 컨트롤을 사용해서 [UICollectionViewFlowLayout](https://developer.apple.com/documentation/uikit/uicollectionviewflowlayout) object와 custom layout object 사이에서 하나를 선택해라. Flow layout이 선택되었다면, scrolling 방향과, (flow layout이) 헤더와 푸터를 갖게 할 지 설정할 수 있다. 헤더와 푸터를 재사용할 수 있게 하는 view를 추가해서 헤더와 푸터 컨텐츠를 설정할 수 있다. 커스텀 레이아웃이 선택되었다면, [UICollectionViewLayout](https://developer.apple.com/documentation/uikit/uicollectionviewlayout)를 subclss 해줘야 한다. |

* Flow layout이 선택되면, Size inspector는 flow layout metrics (cell size, header/footer size, cell 사이의 minimum spacing, cell section 주변의 margin) 들을 설정할 수 있는 attribute를 포함하고 있다.  
***

### Internationalization  
* 컬렉션 뷰 자체로는 따로 국제화 과정이 필요 없으며, 그 안에 있는 cell이나 재사용 가능한 view만 국제화 과정을 거치면 된다.  

### Accessibility  
* Collection view에는 accessble하게 만들 content가 없다. 만약 cell이나 재사용 가능한 view에서 standard UIKit (`UILabel` 혹은 `UITextField)`을 사용했다면, 그것들은 accessible하게 만들어야 한다. Collection view가 onscreen layout을 변경할 때, `UIACcessibilityLayoutChangedNotification`을 post한다. 
***

## Topics


### Collection View를 초기설정 할 때  

```swift
// 특정 frame과 layout을 갖고 있는 collection view를 만들 때
init(frame: CGRect, 
collectionViewLayout layout: UICollectionViewLayout)

// 주어진 unarchiver의 data 내의 collection view object를 만들 때
init?(coder: NSCoder)
```

### Collection view data를 제공할 때  

```swift
// Collection view를 위한 data를 제공하는 object (dataSource)
weak var dataSource: UICollectionViewDataSource? { get set }

// Collection view를 위한 data를 관리하고 cell을 제공하는 object (DiffableDataSource)
class UICollectionViewDiffableDataSource<SectionIdentifierType, ItemIdentifierType> : NSObject where SectionIdentifierType : Hashable, ItemIdentifierType : Hashable

// Collection view를 위한 data를 관리하고 cell을 제공하는 object에 채택된 메서드들 (protocol)  
protocol UICollectionViewDataSource
```

### Collection View Cells와 Data를 prefetching 할 때  

```swift
// Cell과 data prefetching이 가능한지를 나타내는 boolean 값  
var isPrefetchingEnabled: Bool { get set }  

// 다음 cell data requirement에 대한 알림을 받는, prefetching data source object  
weak var prefetchDataSource: UICollectionViewDataSourcePrefetching? { get set }  

// 비동기적인 data load opertion을 허용하면서, data requirements에 대한 경고를 미리 제공하는 protocol  
protocol UICollectionViewDataSourcePrefetching  
```

### Cell을 만들 때

```swift
// Collection view의 cell 등록할 때 사용하는 구조체  
struct CellRegistration<Cell, Item> where Cell : UICollectionViewCell  

// 재사용가능한 cell object를 dequeue할 때  
func dequeueConfiguredReusableCell<Cell, Item>(using registration: UICollectionView.CellRegistration<Cell, Item>, 
                                           for indexPath: IndexPath, 
                                          item: Item?) -> Cell where Cell : UICollectionViewCell  

// Class를 등록해서 새로운 collection view cell을 만들 때  
func register(_ cellClass: AnyClass?, 
forCellWithReuseIdentifier identifier: String)  

// Nib 파일을 등록해서 새로운 collection view cell을 만들 때  
func register(_ nib: UINib?, 
forCellWithReuseIdentifier identifier: String)  

// identifier로 지정된 reusable cell object를 dequeue할 때  
func dequeueReusableCell(withReuseIdentifier identifier: String, 
                     for indexPath: IndexPath) -> UICollectionViewCell  
```

### Header와 Footer를 만들 때
```swift
// Supplementary view를 등록할 때 사용하는 구조체  
struct SupplementaryRegistration<Supplementary> where Supplementary : UICollectionReusableView  

// Reusable supplementary view object를 dequeue할 때  
func dequeueConfiguredReusableSupplementary<Supplementary>(using registration: UICollectionView.SupplementaryRegistration<Supplementary>, 
                                                       for indexPath: IndexPath) -> Supplementary where Supplementary : UICollectionReusableView  

// Class를 등록해서 supplementary view 만들 때  
func register(_ viewClass: AnyClass?, 
forSupplementaryViewOfKind elementKind: String, 
withReuseIdentifier identifier: String)  

// Nib 파일 등록해서 supplementary view 만들 때  
func register(_ nib: UINib?, 
forSupplementaryViewOfKind kind: String, 
withReuseIdentifier identifier: String)  

// identifier로 지정된 reusable supplementary view를 dequeue할 때  
func dequeueReusableSupplementaryView(ofKind elementKind: String, 
                  withReuseIdentifier identifier: String, 
                                  for indexPath: IndexPath) -> UICollectionReusableView  
```

### Background view를 설정할 때
```swift
// Background 모습을 제공하는 view
// Default는 nil로, collection view의 background color를 보여준다.
var backgroundView: UIView? { get set }
```

### Layout을 바꿀 때
```swift
// item을 정리할 때 사용하는 layout
var collectionViewLayout: UICollectionViewLayout { get set }

// layout을 바꾸고, 선택적으로 변화를 animate 해주는 메서드
func setCollectionViewLayout(_ layout: UICollectionViewLayout, 
                    animated: Bool)

// layout을 바꾸고, 애니메이션이 끝날 때 알려주는 메서드
func setCollectionViewLayout(_ layout: UICollectionViewLayout, 
                    animated: Bool, 
                  completion: ((Bool) -> Void)? = nil)

// 현재 layout을 interactive transition effect를 이용해서 바꿔주는 메서드
func startInteractiveTransition(to layout: UICollectionViewLayout, 
                     completion: UICollectionView.LayoutInteractiveTransitionCompletion? = nil) -> UICollectionViewTransitionLayout

// Collection view에게 interactive transition이 끝났다는 것을 알려주고, intended target layout을 설치하는 메서드
func finishInteractiveTransition()

// Collection view에게 interactive transition이 취소되었다는 것을 알려주고, original layout을 return하는 메서드
func cancelInteractiveTransition()

// interactive transition 끝에 호출되는 completion block
typealias LayoutInteractiveTransitionCompletion = (Bool, Bool) -> Void
```

### Collection view의 상태를 받을 때
```swift
// Section의 개수
var numberOfSections: Int { get }

// 특정 section의 아이템 개수를 fetch함
func numberOfItems(inSection section: Int) -> Int

// 현재 Collection view에 표시된 visible cell들의 array
var visibleCells: [UICollectionViewCell] { get }
```

### 아이템 삽입, 이동, 삭제할 때
```swift
// 새로운 아이템을 특정 index path에 넣을 때
func insertItems(at indexPaths: [IndexPath])

// 아이템을 다른 곳으로 옮길 때
func moveItem(at indexPath: IndexPath, 
           to newIndexPath: IndexPath)

// 특정 index path의 아이템을 삭제할 때
func deleteItems(at indexPaths: [IndexPath])
```

### 섹션을 삽입, 이동, 삭제할 때
```swift
// 특정한 index에 section을 넣을 때
func insertSections(_ sections: IndexSet)

// Section의 위치를 이동시킬 때
func moveSection(_ section: Int, 
       toSection newSection: Int)

// 특정한 index에 section을 삭제할 때
func deleteSections(_ sections: IndexSet)
```

### Interactively하게 아이템을 재배치할때
```swift
// 특정 index path에서 아이템을 interactive하게 움직이는 것을 시작하는 메서드
func beginInteractiveMovementForItem(at indexPath: IndexPath) -> Bool

// (Collection view의 bound 안에서) 아이템의 위치를 업데이트 하는 메서드
func updateInteractiveMovementTargetPosition(_ targetPosition: CGPoint)

// interactive movement tracking을 끝내고, 타겟 아이템을 새로운 위치로 움직이게 하는 메서드
func endInteractiveMovement()

// interactive movement tracking을 끝내고, 타겟 아이템을 original location으로 되돌려놓는 메서드
func cancelInteractiveMovement()
```

### Drag Interaction들을 관리할 때
```swift
// 컬렉션 뷰에서 아이템을 드래그하는 것을 관리하는 delegate object
weak var dragDelegate: UICollectionViewDragDelegate? { get set }

// 컬렉션 뷰에서 드래그를 시작하는데 필요한 인터페이스
protocol UICollectionViewDragDelegate

// 아이템이 컬렉션 뷰에서 lift됐지만, 아직 drop 되지 않았음을 알려주는 boolean value
var hasActiveDrag: Bool { get }

// App들 사이에서 컬렉션 뷰가 드래그 앤 드랍을 지원하는지를 나타내는 boolean value
var dragInteractionEnabled: Bool { get set }
```

### Drop interaction들을 관리할 때
```swift
// 컬렉션 뷰로 아이템을 드랍하는 것을 관리하는 delegate object
weak var dropDelegate: UICollectionViewDropDelegate? { get set }

// 컬렉션 뷰가 현재 drop session을 tracking하는지 알려주는 boolean value
var hasActiveDrop: Bool { get }

// 컬렉션 뷰의 아이템이 예측 드랍 위치를 보여주기 위해 reorder되는 스피드
var reorderingCadence: UICollectionView.ReorderingCadence { get set }

// Drop 중에 Collection view 아이템이 reorganized 되는 속도 (immediate, fast, slow)
enum ReorderingCadence : Int
```

### Cell을 선택할 때
```swift
// 선택된 아이템의 index path
var indexPathsForSelectedItems: [IndexPath]? { get }

// 특정 index path의 아이템을 선택하고, 선택적으로 view로 스크롤한다.
func selectItem(at indexPath: IndexPath?, 
       animated: Bool, 
 scrollPosition: UICollectionView.ScrollPosition)

// 특정 index path의 아이템을 선택취소할 때
// allowsSelection 프로퍼티가 false라면, 이 메서드를 호출해도 효과가 없다.
func deselectItem(at indexPath: IndexPath, 
         animated: Bool)

// User가 컬렉션 뷰의 아이템을 선택할 수 있는지 없는지 나타내는 boolean value
var allowsSelection: Bool { get set }

// User가 컬렉션 뷰에서 하나 이상의 아이템을 선택할 수 있는지 없는지 나타내는 boolean value
var allowsMultipleSelection: Bool { get set }

// Editing 모드에서 User가 cell을 선택할 수 있는지를 결정하는 boolean value
var allowsSelectionDuringEditing: Bool { get set }

// Editing 모드에서 User가 하나 이상의 아이템을 선택할 수 있는지 없는지 나타내는 boolean value
var allowsMultipleSelectionDuringEditing: Bool { get set }

// focus가 cell로 옮겨졌을 때 자동 선택을 trigger할지 결정하는 boolean value
var selectionFollowsFocus: Bool { get set }
```

### 컬렉션 뷰를 Edit 모드로 둘 때  
```swift
// 컬렉션 뷰가 editing mode에 있는지 없는지를 알려주는 boolean value
var isEditing: Bool { get set }
```

### 컬렉션 뷰에 item과 view를 둘 때  
```swift
// 컬렉션 뷰에서 특정한 지점에서의 item의 index path를 얻는다.
func indexPathForItem(at point: CGPoint) -> IndexPath?

// 컬렉션 뷰에서 보여지는 아이템들의 array
var indexPathsForVisibleItems: [IndexPath] { get }

// 특정 cell의 index path를 얻는다.
func indexPath(for cell: UICollectionViewCell) -> IndexPath?

// 특정 index path에서 보여지는 cell object를 얻는다.
func cellForItem(at indexPath: IndexPath) -> UICollectionViewCell?

// 보여지는 모든 supplementary view의 모든 index path를 얻는다.
func indexPathsForVisibleSupplementaryElements(ofKind elementKind: String) -> [IndexPath]

// 특정 index path의 supplementary view를 얻는다.
func supplementaryView(forElementKind elementKind: String, 
                    at indexPath: IndexPath) -> UICollectionReusableView?

// 보여지는 모든 supplementary view를 array로 얻는다.
func visibleSupplementaryViews(ofKind elementKind: String) -> [UICollectionReusableView]
```

### Layout 정보를 얻을 때  
```swift
// 특정 index path에서 아이템의 layout information을 얻을 때
func layoutAttributesForItem(at indexPath: IndexPath) -> UICollectionViewLayoutAttributes?

// 특정 supplementary view의 layout information을 얻을 때
func layoutAttributesForSupplementaryElement(ofKind kind: String, 
                                          at indexPath: IndexPath) -> UICollectionViewLayoutAttributes?

```

### Item을 view로 스크롤할 때  
```swift
// 특정 아이템을 볼 수 있을 때까지 컬렉션 뷰 컨텐츠를 스크롤한다.
func scrollToItem(at indexPath: IndexPath, 
               at scrollPosition: UICollectionView.ScrollPosition, 
         animated: Bool)

// 컬렉션 뷰에서 보여지는 부분에서 item을 어떻게 스크롤하는지를 보여주는 constant
struct ScrollPosition

// layout의 스크롤 방향을 나타내는 constant
enum ScrollDirection : Int
```

### 컬렉션 뷰에서 여러 변화를 animate할 때  
```swift
// 여러 insert, delete, reload, move operation을 그룹으로 animate 할 때
func performBatchUpdates(_ updates: (() -> Void)?, 
              completion: ((Bool) -> Void)? = nil)
```

### 컨텐츠를 reloading할 때  
```swift
// 컬렉션 뷰가 drop placeholder를 가지고 있는지와,
// 상단을 조작해서 item을 reordering할 수 있는지를 나타내는 boolean value
var hasUncommittedUpdates: Bool { get }

// 컬렉션 뷰의 모든 data를 reload할 때
func reloadData()

// 컬렉션 뷰의 특정 section의 data를 reload할 때
func reloadSections(_ sections: IndexSet)

// 특정 index path의 아이템들을 reload할 때
func reloadItems(at indexPaths: [IndexPath])
```

### 컬렉션 뷰 element를 identify할 때  
```swift
// View type을 구분하는 constant
enum ElementCategory : UInt

// 주어진 section의 footer를 구분하는 supplementary view
class let elementKindSectionFooter: String

// 주어진 section의 header를 구분하는 supplementary view
class let elementKindSectionHeader: String
```

### Last focused cell을 기억해야 할 때
```swift
// 컬렉션 뷰가 자동으로 마지막으로 focus된 index path의 아이템으로 focus를 지정하는지에 대한 boolean value
var remembersLastFocusedIndexPath: Bool { get set }
```
