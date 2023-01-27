## iOS 커리어 스타터 캠프

# 📝 프로젝트 매니저 

## 📖 목차
1. [팀 소개](#-팀-소개)
2. [기능 소개](#-기능-소개)
3. [Diagram](#-diagram)
4. [폴더 구조](#-폴더-구조)
5. [타임라인](#-타임라인)
6. [프로젝트에서 경험하고 배운 것](#-프로젝트에서-경험하고-배운-것)
7. [트러블 슈팅](#-트러블-슈팅)
8. [참고 링크](#-참고-링크)

## 🌱 팀 소개
 |[inho](https://github.com/inho-98)|
 |:---:|
| <a href="https://github.com/inho-98"><img width="180px" src="https://user-images.githubusercontent.com/71054048/188081997-a9ac5789-ddd6-4682-abb1-90d2722cf998.jpg"></a>|

## 🛠 기능 소개

|<img src=https://i.imgur.com/tty9ofQ.gif width=400>|<img src=https://i.imgur.com/6nCuYP1.gif width=400>|
|:-:|:-:|
|리스트 추가 기능|리스트 편집 기능|
<img src=https://i.imgur.com/ixEfCxX.gif width=400>|<img src=https://i.imgur.com/ERrFnI8.gif width=400>
|리스트 이동 기능|리스트 삭제 기능|


## 👀 Diagram

|<img width=900, src=https://i.imgur.com/3U3A3G3.png>|
|---|

## 🗂 폴더 구조

```
|── ProjectManager
│   ├── Info.plist
│   ├── Resource
│   │   ├── Assets.xcassets
│   │   │   ├── AccentColor.colorset
│   │   │   ├── AppIcon.appiconset
│   │   │   └── Contents.json
│   └── Source
│       ├── App
│       │   ├── AppDelegate.swift
│       │   └── SceneDelegate.swift
│       ├── Extension
│       │   ├── Calendar +.swift
│       │   ├── Date +.swift
│       │   └── DateFormatter +.swift
│       ├── Model
│       │   ├── ListItem.swift
│       │   └── ListType.swift
│       ├── Protocol
│       │   ├── ListFormViewControllerDelegate.swift
│       │   └── MenuPresentable.swift
│       ├── View
│       │   ├── AdditionalView
│       │   │   ├── ListFormView.swift
│       │   │   └── ListFormViewController.swift
│       │   └── ListView
│       │       ├── EllipseLabel.swift
│       │       ├── ListItemCell.swift
│       │       ├── ListStackView.swift
│       │       └── MainViewController.swift
│       └── ViewModel
│           ├── ListFormViewModel.swift
│           ├── ListItemCellViewModel.swift
│           └── MainViewModel.swift
└── ProjectManagerTests
    ├── ListFormViewModelTests.swift
    ├── ListItemCellViewModelTests.swift
    └── MainViewModelTests.swift
```

## ⏰ 타임라인

|날짜|구현 내용|
|--|--| 
|23.1.10|**<STEP1>** SwiftLint 적용 및 코드 수정|
|23.1.11|프로젝트에 SPM을 이용한 Firebase 추가|
|23.1.12|**<STEP2>** ListStackView 구현 및 MainViewController 구성, 커스텀 ListItemCell 구현|
|23.1.13|ListItemCell의 style, ListStackView의 오토레이아웃 수정|
|23.1.14|SceneDelegate이용한 네비게이션 컨트롤러 구현, 네비게이션 바 구성, EllipseLabel 구현|
|23.1.15|테이블뷰에 DiffableDataSource 적용, ListFormView & ListFormViewController 구현|
|23.1.17|**<MVVM>** ListItemCellViewModel 구현 및 셀과 바인딩 메서드 구현, ListFormViewControllerDelegate 프로토콜 생성|
|23.1.18|MenuPresentable 프로토콜 생성, ListItemCell에 longPressGesture 추가 코드 작성|
|23.1.19|**<MVVM>** MainViewModel, ListFormViewModel 구현, tableView의 스와이프 삭제기능 구현, 각 뷰모델과 뷰의 바인딩 구현, ListFormViewController에서 edit모드기능 추가|
|23.1.25|MenuPresentable프로토콜 리팩토링, Main.storyboard파일 삭제, MainViewController의 dataSource프로퍼티를 lazy var에서 옵셔널로 변경|

## ✅ 프로젝트에서 경험하고 배운 것
- `MVVM` 디자인패턴
    ☑️ 비즈니스 로직의 대한 분리
    ☑️ 바인딩을 활용한 뷰에 데이터를 표시하는 방법
    ☑️ 뷰모델에 대한 테스트코드
- iPad환경에서의 `modalPresentaionStyle`
    ☑️ formSheet
    ☑️ popover
- `LongPressGuestureRecognizer` 활용
- `UIStackView`의 `layoutMargin` 활용

## 🚀 트러블 슈팅
## STEP 2
### 1️⃣ `ListItemCellViewModel`에서 데이터 처리 방법
- 프로젝트의 모델타입인 `ListItem`의 `dueDate` 프로퍼티가 화면에 보여지기 위해서는 `Date`타입에서 `String`타입으로 변환되어야 하고, 이를 관리하는 역할을 뷰모델이 한다고 판단했습니다. 
- 그래서 화면에 보여질 타입과 정보를 반영한 `ListItemData` 구조체를 구현하고 활용했습니다. 
    ```swift
    final class ListItemCellViewModel {
        struct ListItemData {
            var title: String
            var body: String
            var dueDate: String
            var isOverDue: Bool = false
        }
        ...
        private var handler: ((ListItemData) -> Void)?
        private var listItemData: ListItemData {
            didSet {
                self.handler?(listItemData)
            }
        }
        ...
        func bind(_ handler: @escaping (ListItemData) -> Void) {
            self.handler = handler
        }
    }
    ```
    - 뷰에 보여질 타입을 담은 구조체와 handler를 이용해서 뷰에게 전달하는 방식입니다.

### 2️⃣ `MainViewModel`에서 모든 리스트아이템 배열 관리방법
- 기존에는 각 타입`(todo, doing, done)`마다 배열을 생성하였는데요, 이렇게 했을때 해당 타입의 리스트를 불러올때 모든 로직마다 switch문이 반복해서 발생하는 중복이 있었습니다.
- 이런 중복을 줄이기 위해서 `totalListItems` 프로퍼티를 생성해 하나의 이중 배열로 관리하고, 인덱스 관련 오류를 줄이기 위해서 `ListType`열거형에 Int타입 rawValue를 채택하고, 각 rawValue의 순서에 맞게 구성하였습니다.
    ```swift
    enum ListType: Int, CaseIterable {
        case todo
        case doing
        case done
    }

    final class MainViewModel {
        private var totalListItems: [[ListItem]] = [[], [], []] 
        // [[todo배열], [doing배열], [done]배열] 
        ...
    }
    ```
### 3️⃣  할일 추가/편집 화면을 재사용하는 방법
- 할일을 추가/편집하는 화면의 레이아웃이 동일해서 뷰와 뷰컨을 재사용할 방법을 고민했습니다. 두 화면의 공통점은 입력될 양식을 나타내고 있다고 생각하여 `ListFormView`와 `ListFormViewController`라는 네이밍을 사용했습니다. 
- 두 화면의 차이점은 `ListItem`을 모델로 가지고있는지 없는지에 차이가 있다고 생각했습니다. 그래서 `ListFormViewModel`을 옵셔널 프로퍼티로 선언하여 생성후에 값이 nil일때는 할일을 추가하는 것이고, 값이 있을때는 편집하는 화면으로 구분 가능하도록 구현하였습니다.
    ```swift
    class ListFormViewController: UIViewController {
        ...
        var formViewModel: ListFormViewModel?
        ...

        init(formViewModel: ListFormViewModel? = nil) {
            super.init(nibName: nil, bundle: nil)

            self.formViewModel = formViewModel
        }
        ...
    }
    ```

### 4️⃣  `StackView`에서 `inset`을 지정하는 방법
- 기존에는 `stackView` 내에 여백을 주고싶을때, `stackView.leading`을 container뷰에서 일정 상수만큼 멀어지도록 `constraint`를 지정하였습니다.
- 이번에는 주로 `stackView.layoutMargin` 프로퍼티를 이용해서 한곳에서 여백에 관한 크기를 관리하는 방법으로 구현했습니다.
    ```swift
    class ListItemCell: UITableViewCell {
        private let cellStackView: UIStackView = {
            let stackView = UIStackView()
            ...
            return stackView
        }()
        ...
            cellStackView.isLayoutMarginsRelativeArrangement = true
            cellStackView.layoutMargins = .init(top: 5, left: 15, bottom: 5, right: 15)
        ...
    }
    ```
    
### 5️⃣ `DateFormatter`의 재사용 방법
- `DateFormatter`가 사용할때 무거운 객체여서 사용할 때마다 인스턴스를 생성하면 불필요한 낭비가 생긴다고 알게 되어서 타입프로퍼티로 인스턴스를 한번 생성하고 재사용하는 방법을 채택했습니다.
    ```swift
    extension DateFormatter {
        static let listDateFormatter: DateFormatter = {
            let formatter = DateFormatter()
            formatter.dateFormat = "yyyy. MM. dd."

            return formatter
        }()
    }
    ```

## 🔗 참고 링크

[공식문서]
- [📎 UIDatePicker](https://developer.apple.com/documentation/uikit/uidatepicker/)
- [📎 popoverpresentationcontroller](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621428-popoverpresentationcontroller)
- [📎 Handling LongPressGesture](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/handling_uikit_gestures/handling_long-press_gestures)


[WWDC]
- [📎 Advances in UI Data Sources](https://developer.apple.com/videos/play/wwdc2019/220/)

[그 외 참고문서]
- [📎 MVVM Turorials](https://www.kodeco.com/34-design-patterns-by-tutorials-mvvm)


