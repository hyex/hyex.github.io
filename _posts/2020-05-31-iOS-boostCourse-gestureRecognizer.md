---
layout: post
title:  "Gesture Recognizer"
date:   2020-05-31 20:47:00 +0900
categories: iOS
tags: boostCourse
---

## 개요
> * gesture recognizer
> * UIGestureRecognizer의 하위클래스를 통해 어떤 제스처를 인식할 수 있는지 알아본다

## Gesture Recognizer
제스처 인식기는 여러 제스처 관련 이벤트를 인식할 수 있다. 특정 제스처 이벤트가 일어날 때 마다 각 타깃에 맞는 액션 메시지를 보내어 제스처 관련 이벤트를 처리한다.

## UIGesture Recognizer class
이 클래스는 특정 제스처 인식기에 대한 동작을 정의한다. 또한 델리게이트 객체를 활용하여 일부 동작을 더욱 세밀하게 사용자화 할 수 있다.

### UIGestureRecognizer의 하위 클래스

아래의 7가지의 UIGestureRecognizer 하위 클래스를 통해 여러 제스처를 인식할 수 있다.

1.  UITapGestureRecognizer : 싱글탭 또는 멀티탭 제스처
2.  UIPinchGestureRecognizer : 핀치(Pinch) 제스처
3.  UIRotationGestureRecognizer : 회전 제스처
4.  UISwipeGestureRecognizer : 스와이프(swipe) 제스처
5.  UIPanGestureRecognizer : 드래그(drag) 제스처
6.  UIScreenEdgePanGestureRecognizer : 화면 가장자리 드래그 제스처
7.  UILongPressGestureRecognizer : 롱프레스(long-press) 제스처

-   제스처 인식기를 사용하기 위해서 타깃-액션 연결을 설정한 후 UIView의 메서드인  `addGestureRecognizer(_:)`  메서드를 통해 뷰에 연결합니다. 제스처가 인식되면 해당 제스처 이벤트에 연결된 타깃에 액션 메시지가 전달됩니다. 호출되는 액션메서드는 아래의 메서드 구현 형식 중 하나와 같아야 합니다.

```swift
@IBAction func myActionMethod()
@IBAction func myActionMethod(_ sender: UIGestureRecognizer)
```

-   윈도우는 뷰에 터치 이벤트를 전달하기 전에 뷰에 추가된 제스처 인식기에 터치 이벤트를 전달합니다. 제스처 인식기가 터치 이벤트를 인식했을 경우 뷰는 터치 이벤트를 받지 못하고, 제스처 인식기가 터치 이벤트를 인식하지 못했을 경우 터치 이벤트를 뷰가 받게 됩니다. 일반적인 제스처 인식기의 동작의 흐름은  `cancelsTouchesInView`,  `delaysTouchesBegan`,  `delaysTouchesEnded`  프로퍼티의 값에 영향을 받습니다.


### UIGestureRecognizer의 주요 메서드

-   `init(target: Any?, action: Selector?)`  : 제스처 인식기를 타깃-액션의 연결을 통해 초기화 합니다.
-   `func location(in: UIView?) -> CGPoint`  : 제스처가 발생한 좌표를 반환합니다.
-   `func addTarget(Any, action: Selector)`  : 제스처 인식기 객체에 타깃과 액션을 추가합니다.
-   `func removeTarget(Any?, action: Selector?)`  : 제스처 인식기 객체로부터 타깃과 액션을 제거합니다.
-   `func require(toFail: UIGestureRecognizer)`  : 여러 개의 제스처 인식기를 가지고 있을 때, 제스처 인식기 사이의 의존성을 설정한다.

  ### UIGestureRecognizer의 주요 프로퍼티

-   `var state: UIGestureRecognizerState`  : 현재 제스처 인식기의 상태를 나타냅니다.
-   `var view: UIView?`  : 제스처 인식기가 연결된 뷰입니다.
-   `var isEnabled: Bool`  : 제스처 인식기가 사용 가능한 상태인지를 나타냅니다.
-   `var cancelsTouchInView`  : 제스처가 인식되었을 때 터치 이벤트가 뷰로 전달되는 여부에 영향을 미칩니다.
    -   이 프로퍼티가 true(기본값)이고 제스처 인식기가 제스처를 인식했다면, 해당 제스처의 터치는 뷰로 전달되지 않습니다. 이전에 전달된 터치들은  `touchesCancelled(_:with:)`  메시지를 통해 취소됩니다. 제스처 인식기가 제스처를 인식 못하거나 이 프로퍼티의 값이 false라면 뷰가 모든 터치를 전달받게 됩니다.
-   `var delaysTouchesBegan`  : began 단계에서 제스처 인식기가 추가된 뷰에 터치의 전달 지연 여부를 결정합니다.
-   `var delaysTouchesEnded`  : end 단계에서 제스처 인식기가 추가된 뷰에 터치의 전달 지연 여부를 결정합니다.

## iOS의 Standard Gesture
1.  Tap : 컨트롤을 활성화하거나 항목을 선택합니다.
2.  Drag : 아이템을 좌우 또는 화면으로 드래그할 수 있습니다.
3.  Flick : 빠르게 스크롤하거나 화면을 넘길 수 있습니다.
4.  Swipe : 이전 화면으로 돌아가거나 테이블 뷰에서 숨겨진 삭제 버튼을 표시할 수 있습니다.
5.  Double tap : 이미지 또는 콘텐츠를 확대하거나 다시 축소합니다.
6.  Pinch : 이미지를 세밀하게 확대하거나 다시 축소할 수 있습니다.
7.  Touch and hold : 편집 가능한 텍스트 또는 선택 가능한 텍스트에서 수행될 경우 커서 지정을 위한 확대보기가 표시됩니다. 컬렉션 뷰의 경우 항목을 재배치할 수 있는 모드로 진입합니다.
8.  Shake : 실행 취소 또는 다시 실행 얼럿을 띄웁니다.

## 참고 링크

[UIGestureRecognizer - UIKit | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uigesturerecognizer)

[Gestures - User Interaction - iOS Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/user-interaction/gestures/)

