---
layout: post
title:  "UIControl(Editing)"
date:   2020-05-15 20:21:00 +0900
categories: iOS
tags: docs
---

## UIControl

**Class**

컨트롤의 기본 클래스이다. 이 클래스는 사용자 상호 작용에 대한 응답으로 특정 동작이나 의도를 전달하는 시각적 요소이다.

### Declaration
```swift
class UIControl : UIView
```
### Overview
컨트롤은 버튼 및 슬라이더와 같은 요소를 구현하여 앱에서 탐색을 용이하게 하고 사용자 입력을 수집하거나 콘텐츠를 조작하는 데 사용할 수 있습니다. 컨트롤은 Target-Action 메커니즘을 사용하여 사용자 상호 작용을 앱에 보고합니다.

이 클래스의 인스턴스를 직접 작성하지 마십시오. UIControl 클래스는 사용자 지정 컨트롤을 구현하기 위해 확장하는 하위 클래스입니다. 기존 제어 클래스를 서브 클래싱하여 동작을 확장하거나 수정할 수도 있습니다. 예를 들어, 터치 이벤트를 직접 추적하거나 컨트롤 상태가 변경되는시기를 결정하기 위해 이 클래스의 메소드를 대체 할 수 있습니다.

컨트롤 상태에 따라 모양과 사용자 상호 작용을 지원하는 기능이 결정됩니다. 컨트롤은 UIControl.State 유형에 의해 정의되는 여러 상태 중 하나 일 수 있습니다. 앱의 요구에 따라 프로그래밍 방식으로 컨트롤 상태를 변경할 수 있습니다. 예를 들어, 사용자가 컨트롤과 상호 작용하지 못하도록 컨트롤을 비활성화 할 수 있습니다. 사용자 상호 작용은 컨트롤 상태를 변경할 수도 있습니다.

**The Target-Action Mechanism**
컨트롤은 Target-Action Mechanism을 사용하여 코드에서 발생하는 흥미로운 이벤트를 보고합니다.  Target-Action Mechanism은 앱에서 컨트롤을 사용하기 위해 작성하는 코드를 단순화합니다. 터치 이벤트를 추적하는 코드를 작성하는 대신 제어 특정 이벤트에 응답하는 action 메소드를 작성합니다. 예를 들어 슬라이더 값의 변경에 응답하는 동작 방법을 작성할 수 있습니다. 컨트롤은 수신되는 터치 이벤트를 추적하고 메서드 호출시기를 결정하는 모든 작업을 처리합니다.

제어에 action 메소드를 추가 할 때 `addTarget (_ : action : for :)` 메소드에 해당 메소드를 정의하는 오브젝트와 action 메소드를 모두 지정하십시오. (인터페이스 빌더에서 제어의 대상 및 조치를 구성 할 수도 있습니다.) 대상 오브젝트는 모든 오브젝트가 될 수 있지만 일반적으로 루트보기에 제어가 포함 된 뷰 제어기입니다. 대상 개체에 대해 nil을 지정하면 컨트롤은 응답자 체인에서 지정된 작업 방법을 정의하는 개체를 검색합니다.

action 메소드의 signature은 목록 1에 나열된 세 가지 형식 중 하나를 사용합니다. 송신자 매개 변수는 조치 메소드를 호출하는 제어에 해당하고 이벤트 매개 변수는 제어 관련 이벤트를 트리거 한 UIEvent 오브젝트에 해당합니다.

###### Listing 1 Action method definitions 

```swift
@IBAction func doSomething()
@IBAction func doSomething(sender: UIButton)
@IBAction func doSomething(sender: UIButton, forEvent event: UIEvent)
```

사용자가 특정 방식으로 컨트롤과 상호 작용할 때 action 메서드가 호출됩니다. `UIControl.Event` 유형은 컨트롤이 보고할 수있는 사용자 상호 작용 유형을 정의하며 이러한 상호 작용은 대부분 컨트롤 내의 특정 터치 이벤트와 관련이 있습니다. 컨트롤을 구성 할 때 메서드 호출을 트리거 할 이벤트를 지정해야합니다. 버튼 컨트롤의 경우 `touchDown` 또는 `touchUpInside` 이벤트를 사용하여 액션 메서드에 대한 호출을 트리거 할 수 있습니다. 슬라이더의 경우 슬라이더의 값 변경에 대해서만 신경을 쓸 수 있으므로 `valueChanged` 이벤트에 액션 방법을 첨부하도록 선택할 수 있습니다.

컨트롤 관련 이벤트가 발생하면 컨트롤은 모든 관련 동작 메서드를 즉시 호출합니다. action 메소드는 현재 UIApplication 오브젝트를 통해 전달되며, 필요한 경우 응답자 체인에 따라 메시지를 처리 할 적절한 오브젝트를 찾습니다. 응답자 및 응답자 체인에 대한 자세한 내용은 UIKit 앱용 이벤트 처리 안내서를 참조하십시오.

### Interface Builder Attributes

Table1에는 `UIControl` 클래스의 인스턴스와 연관된 속성이 나열되어 있습니다.

###### Table 1 Control attributes
| Attribute | Description |
|:--:|:--:|
| Alignment | 컨트롤 콘텐츠의 가로 및 세로 정렬 버튼 및 텍스트 필드와 같은 텍스트 또는 이미지가 포함 된 컨트롤의 경우 이러한 속성을 사용하여 컨트롤 범위 내에서 해당 콘텐츠의 위치를 구성하십시오. 이러한 정렬 옵션은 컨트롤 자체가 아닌 컨트롤의 내용에 적용됩니다. 다른 컨트롤 및 뷰와 관련하여 컨트롤을 정렬하는 방법에 대한 자세한 내용은 자동 레이아웃 안내서를 참조하십시오.|
| Content | 컨트롤의 초기 상태입니다. 확인란을 사용하여 컨트롤이 처음에 활성화, 선택 또는 강조 표시되는지 여부를 구성하십시오. |

### Internationalization 국제화
`UIControl`은 추상 클래스이므로 구체적으로 국제화하지 않습니다. 그러나 `UIButton`과 같은 하위 클래스의 내용을 국제화합니다. 특정 컨트롤 국제화에 대한 자세한 내용은 해당 컨트롤에 대한 참조를 참조하십시오.

### Accessibility 접근성
기본적으로 컨트롤에 액세스 할 수 있습니다. 유용한 액세스 가능한 사용자 인터페이스 요소는 화면 위치, 이름, 동작, 값 및 유형에 대한 정확하고 유용한 정보를 제공해야합니다. 이것은 VoiceOver가 사용자에게 말하는 정보입니다. 시각 장애가있는 사용자는 VoiceOver를 사용하여 장치를 사용할 수 있습니다.

컨트롤은 다음과 같은 내게 필요한 옵션 특성을 지원합니다.

- 상표. 컨트롤이나보기를 간결하게 설명하지만 요소의 유형을 식별하지 못하는 짧고 현지화 된 단어 또는 구입니다. 예를 들면 "추가"또는 "재생"입니다.

- 특성. 하나 이상의 개별 특성의 조합으로 각각 요소의 상태, 동작 또는 사용법의 단일 측면을 설명합니다. 예를 들어, 키보드 키처럼 동작하고 현재 선택된 요소는 키보드 키와 선택한 특성의 조합으로 특징 지을 수 있습니다.

- 힌트. 요소에 대한 작업 결과를 설명하는 현지화 된 간단한 문구입니다. 예를 들면 "제목 추가"또는 "쇼핑 목록 열기"입니다.

- 틀. 화면 좌표의 요소 프레임으로, 요소의 화면 위치와 크기를 지정하는 CGRect 구조로 제공됩니다.

- 값. 값이 레이블로 표시되지 않은 경우 요소의 현재 값 예를 들어 슬라이더의 레이블은 "속도"이지만 현재 값은 "50 %"일 수 있습니다.

`UIControl` 클래스는 값 및 프레임 속성에 대한 기본 내용을 제공합니다. 많은 컨트롤이 추가 특성을 자동으로 활성화합니다. 프로그래밍 방식으로 또는 Interface Builder의 Identity 관리자를 사용하여 다른 내게 필요한 옵션 특성을 구성 할 수 있습니다.

내게 필요한 옵션 특성에 대한 자세한 내용은 iOS 용 내게 필요한 옵션 프로그래밍 안내서를 참조하십시오.

### Subclassing Notes
서브 클래싱 `UIControl`을 사용하면 내장 된 대상 동작 메커니즘 및 단순화 된 이벤트 처리 지원에 액세스 할 수 있습니다. 기존 컨트롤을 서브 클래싱하고 다음 두 가지 방법 중 하나로 동작을 수정할 수 있습니다.

- 기존 서브 클래스의 `sendAction (_ : to : for :) `메소드를 재정 의하여 조치 메소드를 제어의 연관된 대상으로 디스패치하는 것을 관찰하거나 수정하십시오. 이 메소드를 사용하여 지정된 객체, 선택기 또는 이벤트를 기반으로 디스패치 동작을 수정할 수 있습니다.

- `beginTracking (_ : with :)`, `continueTracking (_ : with :)`, `endTracking (_ : with :)` 및 `cancelTracking (with :)` 메서드를 재정 의하여 컨트롤에서 발생하는 터치 이벤트를 추적합니다. 추적 정보를 사용하여 추가 작업을 수행 할 수 있습니다. `UIResponder` 클래스에 의해 정의 된 메소드 대신 항상 이러한 메소드를 사용하여 터치 이벤트를 추적하십시오.

`UIControl`을 직접 서브 클래스하는 경우 서브 클래스는 컨트롤의 시각적 모양을 설정하고 관리해야합니다. 이벤트 추적 방법을 사용하여 컨트롤의 상태를 업데이트하고 컨트롤의 값이 변경 될 때 액션을 보냅니다.

## Topics 
### Configuring the Control's Attributes
- [`var state: UIControl.State`](https://developer.apple.com/documentation/uikit/uicontrol/1618245-state)
The state of the control, specified as a bitmask value.

- [`var isEnabled: Bool`](https://developer.apple.com/documentation/uikit/uicontrol/1618217-isenabled)
A Boolean value indicating whether the control is enabled.

- [`var isSelected: Bool`](https://developer.apple.com/documentation/uikit/uicontrol/1618203-isselected)
A Boolean value indicating whether the control is in the selected state.

- [`var isHighlighted: Bool`](https://developer.apple.com/documentation/uikit/uicontrol/1618231-ishighlighted)
A Boolean value indicating whether the control draws a highlight.

- [`var contentVerticalAlignment: UIControl.ContentVerticalAlignment`](https://developer.apple.com/documentation/uikit/uicontrol/1618249-contentverticalalignment)
The vertical alignment of content within the control’s bounds.

- [`enum UIControl.ContentVerticalAlignment`](https://developer.apple.com/documentation/uikit/uicontrol/contentverticalalignment)
Constants for specifying the vertical alignment of content (text and images) in a control.

- [`var contentHorizontalAlignment: UIControl.ContentHorizontalAlignment`](https://developer.apple.com/documentation/uikit/uicontrol/1618213-contenthorizontalalignment)
The horizontal alignment of content within the control’s bounds.

- [`var effectiveContentHorizontalAlignment: UIControl.ContentHorizontalAlignment`](https://developer.apple.com/documentation/uikit/uicontrol/2866070-effectivecontenthorizontalalignm)
The horizontal alignment currently in effect for the control.

- [`enum UIControl.ContentHorizontalAlignment`](https://developer.apple.com/documentation/uikit/uicontrol/contenthorizontalalignment)
The horizontal alignment of content (text and images) within a control.

### Accessing the Control's Targets and Actions
- [`func addTarget(Any?, action: Selector, for: UIControl.Event)`](https://developer.apple.com/documentation/uikit/uicontrol/1618259-addtarget)
Associates a target object and action method with the control.

- [`func removeTarget(Any?, action: Selector?, for: UIControl.Event)`](https://developer.apple.com/documentation/uikit/uicontrol/1618248-removetarget)
Stops the delivery of events to the specified target object.

- [`func actions(forTarget: Any?, forControlEvent: UIControl.Event) -> [String]?`](https://developer.apple.com/documentation/uikit/uicontrol/1618251-actions)
Returns the actions performed on a target object when the specified event occurs.

- [`var allControlEvents: UIControl.Event`](https://developer.apple.com/documentation/uikit/uicontrol/1618225-allcontrolevents)
Returns the events for which the control has associated actions.

- [`var allTargets: Set<AnyHashable>`](https://developer.apple.com/documentation/uikit/uicontrol/1618207-alltargets)
Returns all target objects associated with the control.

### Triggering Actions
- [`func sendAction(Selector, to: Any?, for: UIEvent?)`](https://developer.apple.com/documentation/uikit/uicontrol/1618237-sendaction)
Calls the specified action method.

- [`func sendActions(for: UIControl.Event)`](https://developer.apple.com/documentation/uikit/uicontrol/1618211-sendactions)
Calls the action methods associated with the specified events.

### Tracking Touches and Redrawing Controls
- [`func beginTracking(UITouch, with: UIEvent?) -> Bool`](https://developer.apple.com/documentation/uikit/uicontrol/1618227-begintracking)
Called when a touch event enters the control’s bounds.

- [`func continueTracking(UITouch, with: UIEvent?) -> Bool`](https://developer.apple.com/documentation/uikit/uicontrol/1618216-continuetracking)
Called when a touch event associated with the control is updated.

- [`func endTracking(UITouch?, with: UIEvent?)`](https://developer.apple.com/documentation/uikit/uicontrol/1618234-endtracking)
Called when a touch event associated with the control ends.

- [`func cancelTracking(with: UIEvent?)`](https://developer.apple.com/documentation/uikit/uicontrol/1618219-canceltracking)
Tells the control to cancel tracking related to the given event.

- [`var isTracking: Bool`](https://developer.apple.com/documentation/uikit/uicontrol/1618210-istracking)
A Boolean value indicating whether the control is currently tracking touch events.

- [`var isTouchInside: Bool`](https://developer.apple.com/documentation/uikit/uicontrol/1618229-istouchinside)
A Boolean value indicating whether a tracked touch event is currently inside the control’s bounds.

### Constants
- [`struct UIControl.Event`](https://developer.apple.com/documentation/uikit/uicontrol/event)
Constants describing the types of events possible for controls.

- [`struct UIControl.State`](https://developer.apple.com/documentation/uikit/uicontrol/state)
Constants describing the state of a control.