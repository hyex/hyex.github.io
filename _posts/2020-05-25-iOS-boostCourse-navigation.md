---
layout: post
title:  "Navigation"
date:   2020-05-25 20:50:00 +0900
categories: iOS
tags: boostCourse
---

## 개요
> * [네비게이션 컨트롤러](#네비게이션-컨트롤러)가 무엇인지 이해한다.
> * [네비게이션바의 구성](#네비게이션바의-구성)을 간단하게 살펴본다.

---

## 네비게이션 인터페이스
iOS에서 네비게이션 인터페이스는 주로 계층적 구조의 화면전환을 위해 사용되는 drill-down interface이다. drill-down interface란, 아래 그림과 같이 각 선택할 수 있는 항목에 대한 세부항목이 존재하는 인터페이스이다.

<img src="/assets/img/drillDown.png" alt="drill-down interface" />

네비게이션 인터페이스는 네비게이션 컨트롤러를 통해 구현한다. 

### 네비게이션 컨트롤러
container view controller로써 네비게이션 스택을 사용하여 다른 뷰 컨트롤러를 관리한다. 여기서 네비게이션 스택에 담겨서 콘텐츠를 보여주게 되는 뷰 컨트롤러들을 content view controller라고 한다.
네비게이션 컨트롤러는 두 개의 뷰를 화면에 표시한다. 
1. 네비게이션 스택뷰에 포함된 최상위 컨텐트 뷰 컨트롤러의 콘텐츠를 나타내는 뷰
2. 네비게이션 컨트롤러가 직접 관리하는 뷰(네비게이션바 또는 툴바)
추가로 네비게이션 인터페이스의 변화에 따른 특정 액션을 동작하도록 하기 위해 네비게이션 델리게이트 객체를 사용할 수 있다.

<img src="/assets/img/naviView.png" alt="navigation View" />

### 네비게이션 스택
뷰 컨트롤러를 담을 수 있는 배열과 같다. 가장 하위에 있는(가장 먼저 스택에 추가된) 뷰 컨트롤러는 네비게이션 컨트롤러의 root view controller가 된다. 루트 뷰 컨트롤러는 네비게이션 스텍에서 pop되지 않는다. 네비게이션 스택의 가장 상위에 있는(가장 마지막에 push된) 뷰 컨트롤러는 최상위 뷰 컨트롤러로 화면에 보이게 된다.
이름에서 알 수 있듯이 네비게이션 스택은 Push, pop을 통해 뷰 컨트롤러를 관리한다. 네비게이션 스택에 푸시된 각 뷰 컨트롤러들은 애플리케이션에 자신이 가지고 있는 뷰 계층 구조를 통해 컨텐츠를 표시하게 된다.

#### 네비게이션 스택에서의 화면 이동
UINavigationController 클래스의 메서드 또는 segue를 사용하여 네비게이션 스택의 뷰 컨트롤러를 추가/삭제할 수 있다. 또한 애플리케이션 실행 중 사용자가 네비게이션 인터페이스의 back 버튼을 사용하거나 왼쪽 가장자리를 스와이프하여 스택에 있는 최상위 뷰 컨트롤러를 삭제하고 그 아래에 가려져 있던 뷰 컨트롤러의 콘텐츠를 보여줄 수도 있다.
> segue : 스토리보드에서 한 화면에서 다른 화면으로의 전환을 말한다. 세그도 내부적으로 UINavigationController 클래스의 메서드를 사용한다. 

예제 ) 네비게이션 스택의 push
푸시될 때, UIViewController 인스턴스가 생성되고 네비게이션 스택에 추가된다.
1. 가장 먼저 네비게이션 스택에 루트 뷰 컨트롤러만 들어가있는 초기상태이다. (네비게이션 컨트롤러를 생성할 때 반드시 루트 뷰 컨트롤러가 설정되어야함)
2. 이 루트 뷰 컨트롤러에서 뷰 컨트롤러1로 이동하는 버튼을 만들고, 이 버튼을 클릭하면 네비게이션 스택에 뷰 컨트롤러1이 push 된다. 뷰 컨트롤러1의 인스턴스가 생성되고 네비게이션의 스택에 추가된다. 뷰 컨트롤러1이 최상위 뷰 컨트롤러로써 화면에 보이게 된다.
3. 뷰 컨트롤러2로 이동하는 버튼을 만들고 클릭하면, 뷰 컨트롤러2의 인스턴스가 생성되고 네비게이션 스택에 추가된다. 뷰 컨트롤러2가 최상위 뷰 컨트롤러로써 화면에 보이게 된다. 여기서 주목할 점은 **새로운 뷰 컨트롤러가 추가될 때 아래에 있는 뷰 컨트롤러들이(인스턴스) 삭제되지 않고 유지되고 있다는 점**이다.

예제) 네비게이션 스택의 pop
1. 푸시 예제의 마지막 상태에서 백버튼을 눌러 뷰 컨트롤러2를 pop한다. 뷰 컨트롤러2가 네비게이션 스택에서 삭제된다. 뷰 컨트롤러1이 다시 최상위 컨트롤러로써 화면에 보여진다.
2. 이 후 한 번 더 백버튼을 클릭하여 뷰 컨트롤러1을 pop한다. 뷰 컨트롤러1이 메모리에서 해제되고 네비게이션 스택에서 삭제된다. 루트 뷰 컨트롤러가 최상위 뷰 컨트롤러가 되고 화면에 보여진다. 루트 뷰 컨트롤러는 절대 pop되지 않는다.

### UINavigationController 클래스
#### 생성
`init(rootViewController: UIViewController)`

-> 사용할 때는 `UINavigationCOntroller(rootViewController: 뷰컨이름)`

#### 네비게이션 스택의 뷰 컨트롤러에 대한 접근
```swift
// 네비게이션 스택에 있는 최상위 뷰 컨트로럴에 접근하기 위한 프로퍼티
var topViewController: UIViewController?

// 현재 네비게이션 인터페이스에서 보이는 뷰와 관련된 뷰 컨트롤러에 접근하는 프로퍼티
var visibleViewController: UIViewController?

// 특정 뷰 컨트롤러에 접근하기 위한 프로퍼티 (루트 뷰 컨트롤러의 인덱스는 0)
var viewController: [UIViewController]
```

#### push, pop 관련 메서드
```swift
// 내비게이션 스택에 뷰 컨트롤러를 push
// 푸시 된 뷰 컨트롤러는 최상위 뷰 컨트롤러로 화면에 표시됩니다.
func pushViewController(UIViewController, animated: Bool)

// 내비게이션 스택에 있는 최상위 뷰 컨트롤러를 pop
// 최상위 뷰 컨트롤러 아래에 있던 뷰 컨트롤러의 콘텐츠가 화면에 표시됩니다.
func popViewController(animated: Bool) -> UIViewController?

// 내비게이션 스택에서 루트 뷰 컨트롤러를 제외한 모든 뷰 컨트롤러를 pop
// 루트 뷰 컨트롤러가 최상위 뷰 컨트롤러가 됩니다.
// 삭제된 모든 뷰 컨트롤러의 배열이 반환됩니다.
func popToRootViewController(animated: Bool) -> [UIViewController]?

// 특정 뷰 컨트롤러가 내비게이션 스택에 최상위 뷰 컨트롤러가 되기 전까지 상위에 있는 뷰 컨트롤러들을 pop
func popToViewController(_ viewController: UIViewController, 
		animated: Bool) -> [UIViewController]?
```

### 네비게이션 인터페이스를 구성하는 두 가지 방법
1. 스토리보드 사용
2. 코드작성 
예를 들어 네비게이션 컨트롤러가 애플리케이션 window의 루트 뷰로서 역할을 한다면, 네비게이션 컨트롤러를 applicationDidFinishLaunching 메서드에 구현할 수 있다.
```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        
        // 루트 뷰 컨트롤러가 될 뷰 컨트롤러를 생성합니다.
        let rootViewController = UIViewController()
        // 위에서 생성한 뷰 컨트롤러로 내비게이션 컨트롤러를 생성합니다.
        let navigationController = UINavigationController(rootViewController: rootViewController)
        
        self.window = UIWindow(frame: UIScreen.main.bounds)
        // 윈도우의 루트 뷰 컨트롤러로 내비게이션 컨트롤러를 설정합니다.
        self.window?.rootViewController = navigationController
        self.window?.makeKeyAndVisible()
        
        return true
    }
```

### 네비게이션바의 구성
네비게이션바는 네비게이션 컨트롤러에 의해 생성된다. 네비게이션바는 네비게이션 컨트롤러의 관리를 받는 모든 뷰 컨트롤러 상단에 표시된다. 최상위 뷰 컨트롤러가 변경될 때 마다 네비게이션 컨트롤러는 네비게이션바를 업데이트한다.
네비게이션바의 구조는 이렇다.

<img src="/assets/img/naviBar.png" alt="navigation bar" />

- 네비게이션바는 네비게이션 인터페이스에서 상단에 표시된다.
- 네비게이션바는 네비게이션 아이템을 가질 수 있다.
- 뷰 컨트롤러가 전환될 때마다 네비게이션바의 콘텐츠들이 바뀌지만 네비게이션바 자체는 네비게이션 컨트롤러가 관리하는 하나의 공통 객체이다.
- 네비게이션바의 타이틀을 통해 현재의 위치(최상위 뷰컨트롤러)를 알 수 있다.


### 참고 링크
[원문](https://www.edwith.org/boostcourse-ios/lecture/16857/)

[Navigation - App Architecture - iOS Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/ios/app-architecture/navigation/)

[View Controller Catalog for iOS - Navigation Controllers](https://developer.apple.com/library/archive/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/NavigationControllers.html)

[UINavigationBar - UIKit](https://developer.apple.com/documentation/uikit/uinavigationbar)

[UINavigationController - UIKit](https://developer.apple.com/documentation/uikit/uinavigationcontroller)