---
layout: post
title:  "UIControl"
date:   2020-05-15 20:21:00 +0900
categories: iOS
tags: docs
---

## Class
> 컨트롤의 기본 클래스이다. 이 클래스는 사용자 상호 작용에 대한 응답으로 특정 동작이나 의도를 전달하는 시각적 요소이다.

## Declaration
```swift
class UIControl : UIView
```
## Overview
컨트롤은 버튼 및 슬라이더와 같은 요소를 구현하여 앱에서 탐색을 용이하게 하고 사용자 입력을 수집하거나 콘텐츠를 조작하는 데 사용할 수 있습니다. 컨트롤은 Target-Action 메커니즘을 사용하여 사용자 상호 작용을 앱에 보고합니다.

이 클래스의 인스턴스를 직접 작성하지 마십시오. UIControl 클래스는 사용자 지정 컨트롤을 구현하기 위해 확장하는 하위 클래스입니다. 기존 제어 클래스를 서브 클래싱하여 동작을 확장하거나 수정할 수도 있습니다. 예를 들어, 터치 이벤트를 직접 추적하거나 컨트롤 상태가 변경되는시기를 결정하기 위해 이 클래스의 메소드를 대체 할 수 있습니다.

컨트롤의 상태가 모양과 사용자 상호 작용을 지원하는 기능을 결정합니다. 컨트롤은 `UIControl.State` 유형에 의해 정의되는 여러 상태 중 하나 일 수 있습니다. 앱의 요구에 따라 프로그래밍으로 컨트롤 상태를 변경할 수 있습니다. 예를 들어, 사용자가 컨트롤과 상호 작용하지 못하도록 컨트롤을 비활성화 할 수 있습니다.  사용자도 컨트롤 상태를 변경할 수도 있습니다.

**The Target-Action Mechanism**
컨트롤은 Target-Action Mechanism을 사용하여 코드에서 발생하는 흥미로운 이벤트를 보고합니다.  Target-Action Mechanism은 앱에서 컨트롤을 사용하기 위해 작성하는 코드를 단순화합니다. 터치 이벤트를 추적하는 코드를 작성하는 대신에, 제어 특정 이벤트에 응답하는 action 메소드를 작성합니다. 예를 들어 슬라이더 값의 변경에 응답하는 동작 방법을 작성할 수 있습니다. 컨트롤은 수신되는 터치 이벤트를 추적하고 메서드 호출시기를 결정하는 모든 작업을 처리합니다.

컨트롤에 action 메소드를 추가 할 때 `addTarget (_ : action : for :)` 메소드에 해당 메소드를 정의하는 오브젝트와 action 메소드를 모두 지정하십시오. (인터페이스 빌더에서 제어의 대상 및 조치를 구성 할 수도 있습니다.) 대상 오브젝트는 모든 오브젝트가 될 수 있지만 일반적으로 컨트롤을 포함하는 루트 view 의 view controller입니다. 대상 개체에 대해 nil을 지정하면 컨트롤은 responder 체인에서 지정된 작업 방법을 정의하는 개체를 검색합니다.

action 메소드의 signature은 목록 1에 나열된 세 가지 형식 중 하나를 사용합니다. sender 매개 변수는 action 메소드를 호출하는 control에 해당하고 event 매개 변수는 제어 관련 이벤트를 트리거 한 UIEvent object에 해당합니다.

###### Listing 1 Action method definitions 

```swift
@IBAction func doSomething()
@IBAction func doSomething(sender: UIButton)
@IBAction func doSomething(sender: UIButton, forEvent event: UIEvent)
```

사용자가 특정 방식으로 컨트롤과 상호 작용할 때 action 메서드가 호출됩니다. `UIControl.Event` 유형은 컨트롤이 보고할 수있는 사용자 상호 작용 유형을 정의하며 이러한 상호 작용은 대부분 컨트롤 내의 특정 터치 이벤트와 관련이 있습니다. 컨트롤을 구성 할 때 메서드 호출을 트리거 할 이벤트를 지정해야합니다. 버튼 컨트롤의 경우 `touchDown` 또는 `touchUpInside` 이벤트를 사용하여 action 메서드에 대한 호출을 트리거 할 수 있습니다. 슬라이더의 경우 슬라이더의 값 변경에 대해서만 신경을 쓸 수 있으므로 `valueChanged` 이벤트에 액션 방법을 첨부하도록 선택할 수 있습니다.

컨트롤 관련 이벤트가 발생하면 컨트롤은 모든 관련 동작 메서드를 즉시 호출합니다. action 메소드는 현재 UIApplication 오브젝트를 통해 전달되며, 필요한 경우 responder 체인에 따라 메시지를 처리 할 적절한 오브젝트를 찾습니다. 응답자 및 응답자 체인에 대한 자세한 내용은 [UIKit 앱용 이벤트 처리 안내서](https://developer.apple.com/library/archive/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/index.html#//apple_ref/doc/uid/TP40009541)를 참조하십시오.

## Interface Builder Attributes

Table1에는 `UIControl` 클래스의 인스턴스와 연관된 속성이 나열되어 있습니다.

###### Table 1 Control attributes

| Attribute | Description |
|:--:|:--:|
| Alignment | **컨트롤 콘텐츠**의 가로 및 세로 정렬. 버튼 및 텍스트 필드와 같은 텍스트 또는 이미지가 포함 된 컨트롤의 경우, 이러한 속성을 사용하여 컨트롤 범위 내에서 해당 콘텐츠의 위치를 구성하십시오. 이러한 정렬 옵션은 컨트롤 자체가 아닌 컨트롤의 내용에 적용됩니다. 다른 컨트롤 및 뷰와 관련하여 컨트롤을 정렬하는 방법에 대한 자세한 내용은 자동 레이아웃 안내서를 참조하십시오.|
| Content | 컨트롤의 초기 상태입니다. 체크박스을 사용하여 컨트롤이 처음에 활성화, 선택 또는 강조 표시되는지 여부를 구성하십시오. |

## Internationalization 국제화
Because `UIControl` is an abstract class, you do not internationalize it specifically. However, you do internationalize the content of subclasses like [`UIButton`](https://developer.apple.com/documentation/uikit/uibutton). For information about internationalizing a specific control, see the reference for that control.

## Accessibility 접근성

Controls are accessible by default. To be useful, an accessible user interface element must provide accurate and helpful information about its screen position, name, behavior, value, and type. This is the information VoiceOver speaks to users. Visually impaired users can rely on VoiceOver to help them use their devices.

Controls support the following accessibility attributes:

-   **Label.**  A short, localized word or phrase that succinctly describes the control or view, but does not identify the element’s type. Examples are “Add” or “Play.”
    
-   **Traits.**  A combination of one or more individual traits, each of which describes a single aspect of an element’s state, behavior, or usage. For example, an element that behaves like a keyboard key and that is currently selected can be characterized by the combination of the Keyboard Key and Selected traits.
    
-   **Hint.**  A brief, localized phrase that describes the results of an action on an element. Examples are “Adds a title” or “Opens the shopping list.”
    
-   **Frame.**  The frame of the element in screen coordinates, which is given by the CGRect structure that specifies an element’s screen location and size.
    
-   **Value.**  The current value of an element, when the value is not represented by the label. For example, the label for a slider might be “Speed,” but its current value might be “50%.”

The  `UIControl`  class provides default content for the value and frame attributes. Many controls enable additional specific traits automatically as well. You can configure other accessibility attributes programmatically or using the Identity inspector in Interface Builder.

For more information about accessibility attributes, see  [Accessibility Programming Guide for iOS](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/iPhoneAccessibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008785).

## Subclassing Notes
Subclassing  `UIControl`  gives you access to the built-in target-action mechanism and simplified event-handling support. You can subclass existing controls and modify its behavior in one of two ways:

-   Override the  [`sendAction(_:to:for:)`](https://developer.apple.com/documentation/uikit/uicontrol/1618237-sendaction)  method of an existing subclass to observe or modify the dispatching of action methods to the control’s associated targets. You might use this method to modify the dispatch behavior based on the specified object, selector, or event.
    
-   Override the  [`beginTracking(_:with:)`](https://developer.apple.com/documentation/uikit/uicontrol/1618227-begintracking),  [`continueTracking(_:with:)`](https://developer.apple.com/documentation/uikit/uicontrol/1618216-continuetracking),  [`endTracking(_:with:)`](https://developer.apple.com/documentation/uikit/uicontrol/1618234-endtracking), and  [`cancelTracking(with:)`](https://developer.apple.com/documentation/uikit/uicontrol/1618219-canceltracking)  methods to track touch events occurring in the control. You can use the tracking information to perform additional actions. Always use these methods to track touch events instead of the methods defined by the  `UIResponder`  class.
    
If you subclass  `UIControl`  directly, your subclass is responsible for setting up and managing your control’s visual appearance. Use the methods for tracking events to update your control’s state and to send an action when the control’s value changes.

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