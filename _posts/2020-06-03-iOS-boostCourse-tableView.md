---
layout: post
title:  "Table View"
date:   2020-06-03 19:46:00 +0900
categories: iOS
tags: boostCourse
---

## 개요
> * Table view
> * Table view cell
> * DataSource와 Delegate

---
## Table view 
iOS 애플리케이션에서 많이 활용하는 사용자 인터페이스이다. 테이블뷰는 리스트 형태를 지니고 있으며 스크롤이 가능해 많은 정보를 보여주기 용이하다.

### 기본 형태
- 하나의 열(column)과 여러 줄의 행(row)을 지니며, 수직으로만 스크롤이 가능하다.
- 각 행은 하나의 셀(cell)에 대응한다. 
- 섹션(section)을 이용해 행을 시각적으로 나눌 수 있다.
- 헤더(header)와 푸터(footer)에 이미지나 텍스트를 추가해 추가 정보를 보여줄 수 있다.

### 스타일
크게 두 가지 스타일(일반, 그룹)로 나뉜다.

- 일반 테이블뷰 (Plain TableView)
    - 더 이상 나뉘지 않는 **연속적인 행**의 리스트 형태
    - 하나 이상의 섹션을 가질 수 있으며, 각 섹션은 여러 개의 행을 지닐 수 있다.
    - 각 섹션은 헤더 혹은 푸터를 옵션으로 가질 수 있다  
    - 색인을 이용해 빠른 탐색을 하거나 옵션을 선택할 때 용이하다.

- 그룹 테이블뷰 (Grouped TableView)
    - **섹션을 기준으로 그룹화**되어 있는 리스트 형태
    - 하나 이상의 섹션을 가질 수 있으며, 각 섹션은 여러 개의 행을 지닐 수 있다.
    - 각 섹션은 헤더 혹은 푸터를 옵션으로 가질 수 있다.
    - 정보를 특정 기준에 따라 개념적으로 구분할 때 적합
    - 사용자가 정보를 빠르게 이해하는 데 도움이 된다.

<img src="/assets/img/plainTableView.png" alt="plain tableview" />
<img src="/assets/img/groupedTableView.png" alt="grouped tableview" />

### 생성
스토리보드에서 커스텀 UITableViewController 클래스의 객체를 이용할 수 도 있고, 소스코드에서 생성하는 것도 가능하다. 스토리보드에서 테이블뷰의 특성을 지정할 때, 동적 프로토타입 또는 정적 셀 중 하나를 선택할 수 있다. 새로운 테이블뷰를 생성할 때 기본 설정 값은 동적 프로토타입이다.

- 동적 프로토타입 (Dynamic Prototypes)
    - 셀 하나를 디자인해 이를 다른 셀의 템플릿으로 사용하는 방식
    - 같은 레이아웃의 셀을 여러 개 이용해 정보를 표시할 경우
    - UITableViewDataSource 인스턴스에 의해 콘텐츠를 관리하며, 셀의 개수가 상황에 따라 변하는 경우에 사용

- 정적 셀 (Static cells)
    - 고유의 레이아웃과 고정된 수의 행을 가지는 테이블 뷰에 사용
    - 테이블뷰를 디자인하는 시점에 테이블의 형태와 셀의 개수가 정해져 있는 경우 사용
    - 셀의 개수가 변하지 않음

### 구성요소
테이블뷰를 구성하기 위해 꼭 알아야 하는 개념에는 cell, delegate, data source가 있다. 

### 참고 링크

[UITableView - UIKit - Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uitableview)

[Table View Programming Guide for iOS](https://developer.apple.com/documentation/uikit/views_and_controls/table_views)

[Tables - Views - iOS Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/ios/views/tables/)


---
## Table view cell
테이블뷰를 이루는 개별적인 행(row)으로, UITableViewCell 클래스를 상속받는다. UITableViewCell 클래스에 정의된 표준 스타일을 활용해 문자열 혹은 이미지를 제공하는 셀을 생성할 수 있으며, 커스텀 서브뷰를 올려 다양한 시각적 모습을 나타낼 수 있다. 

### 구조
크게 콘텐츠 영여과 액세서리뷰 영역으로 구조가 나뉜다.
- 콘텐츠 영역 : 셀의 왼쪽 부분에는 주로 문자열, 이미지 혹은 고유 식별자 등이 입력된다.
- 액세서리뷰 영역 : 셀의 오른쪽 작은 부분은 액세서리뷰로 상세보기, 재정렬, 스위치 등과 같은 컨트롤 객체가 위치한다.

<img src="/assets/img/tableviewCell.png" alt="tableviewCell" />

테이블 뷰를 편집 모드로 전환하면 아래와 같은 구조로 바뀐다
- 편집 컨트롤은 삭제 컨트롤또는 추가 컨트롤 중 하나가 될 수 있다.
- 재정렬이 가능한 경우, 재정렬 컨트롤이 액세서리뷰에 나타난다.

<img src="/assets/img/tableviewCellEditMode.png" alt="tableviewCell edit mode" />

### 일반 셀의 기본 기능
- UITableViewCell 클래스를 상속받는 기본 테이블뷰 셀은 표준 스타일을 이용할 수 있다. 표준 스타일의 콘텐츠 영역은 한 개 이상의 문자열 그리고 이미지를 지닐 수 있으며, 이미지가 오른쪽으로 확장됨에 따라 문자열이 오른쪽으로 밀려난다.
- UITableViewCell 클래스는 셀 콘텐츠에 세 가지 프로퍼티가 정의되어 있다.
    - textLabel: UILabel
    - detailTextLabel: UILabel
    - imageView: UIImageVIew

### 커스텀 테이블뷰 셀
셀을 커스텀할 수 있다. 방법은 두 가지가 있는데, 스토리보드를 이용하거나 코드로 구현하는 방법이 있다.

**참고** UITableViewCell의 서브클래스를 이용해 커스텀 이미지뷰를 생성하는 경우, 이미지뷰의 변수명을 imageView로 명명하면 기본 이미지뷰 프로퍼티의 변수명과 같아 이상하게 동작할 수 있으니, 네이밍에 유의하자.

### 참고 링크

[UITableViewCell - UIKit - Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uitableviewcell)

[Table View Programming Guide for iOS](https://developer.apple.com/documentation/uikit/views_and_controls/table_views)

[Tables - Views - iOS Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/ios/views/tables/)


---
## DataSource와 Delegate
MVC 모델에 따라 data source는 M과 관련되어 있으며, dalegate는 테이블뷰의 모양과 동작을 관리하기에 C의 역할에 가깝다. 테이블 뷰는 V의 역할을 한다. 이 둘 덕분에 테이블뷰를 매우 유연하게 만들 수 있다.

### data source
- UITableViewDataSource 프로토콜을 채택한다.
- 테이블 뷰를 생성하고 수정하는데 필요한 정보를 테이블뷰 객체에 제공
- 데이터 모델의 델리게이트로, 테이블뷰의 시각적인 모양에 대한 최소한의 정보를 제공
- UITableView 객체에 섹션의 수와 행의 수를 알려주며, 행의 삽입, 삭제 및 재정렬하는 기능을 선택적으로 구현할 수 있다.
- UITableViewDataSource 프로토콜의 주요 메서드는 아래와 같다. 이 중 @required로 선언된 두 가지 메서드는 이 프로토콜을 채택한 타입에 필수로 구현해야 한다.

```swift
 @required 
 // 특정 위치에 표시할 셀을 요청하는 메서드
 func tableView(UITableView, cellForRowAt: IndexPath) 
 
 // 각 섹션에 표시할 행의 개수를 묻는 메서드
 func tableView(UITableView, numberOfRowsInSection: Int)
 
 @optional
 // 테이블뷰의 총 섹션 개수를 묻는 메서드
 func numberOfSections(in: UITableView)
 
 // 특정 섹션의 헤더 혹은 푸터 타이틀을 묻는 메서드
 func tableView(UITableView, titleForHeaderInSection: Int)
 func tableView(UITableView, titleForFooterInSection: Int)
 
 // 특정 위치의 행을 삭제 또는 추가 요청하는 메서드
 func tableView(UITableView, commit: UITableViewCellEditingStyle, forRowAt: IndexPath)
 
 // 특정 위치의 행이 편집 가능한지 묻는 메서드
 func tableView(UITableView, canEditRowAt: IndexPath)

 // 특정 위치의 행을 재정렬 할 수 있는지 묻는 메서드
 func tableView(UITableView, canMoveRowAt: IndexPath)
 
 // 특정 위치의 행을 다른 위치로 옮기는 메서드
 func tableView(UITableView, moveRowAt: IndexPath, to: IndexPath)
 ```

 ### delegate
 - UITableViewDelegate 프로토콜을 채택한다.
 - 테이블뷰의 시각적인 부분 수정, 행의 선택 관리, 액세서리뷰 지원 그리고 테이블뷰의 개별 행 편집을 도와준다.
 - 델리게이트 메서드를 활용하여 테이블뷰의 세세한 부분을 조정할 수 있다.
 - UITableViewDelegate 프로토콜의 주요 메서드는 아래와 같다.

 ```swift
 // 특정 위치 행의 높이를 묻는 메서드
 func tableView(UITableView, heightForRowAt: IndexPath)
 // 특정 위치 행의 들여쓰기 수준을 묻는 메서드
 func tableView(UITableView, indentationLevelForRowAt: IndexPath)

 // 지정된 행이 선택되었음을 알리는 메서드
 func tableView(UITableView, didSelectRowAt: IndexPath)

 // 지정된 행의 선택이 해제되었음을 알리는 메서드
 func tableView(UITableView, didDeselectRowAt: IndexPath)

 // 특정 섹션의 헤더뷰 또는 푸터뷰를 요청하는 메서드
 func tableView(UITableView, viewForHeaderInSection: Int)
 func tableView(UITableView, viewForFooterInSection: Int)

 // 특정 섹션의 헤더뷰 또는 푸터뷰의 높이를 물어보는 메서드
 func tableView(UITableView, heightForHeaderInSection: Int)
 func tableView(UITableView, heightForFooterInSection: Int)

 // 테이블뷰가 편집모드에 들어갔음을 알리는 메서드
 func tableView(UITableView, willBeginEditingRowAt: IndexPath)

 // 테이블뷰가 편집모드에서 빠져나왔음을 알리는 메서드
 func tableView(UITableView, didEndEditingRowAt: IndexPath?)
 ```

### 참고 링크

[Table View Programming Guide for iOS](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/TableView_iPhone/TableViewAPIOverview/TableViewAPIOverview.html)


[UITableViewDataSource - UIKit - Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uitableviewdatasource)

[UITableViewDelegate - UIKit - Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uitableviewdelegate)
