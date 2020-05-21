---
layout: post
title:  "Auto Layout"
date:   2020-05-21 18:16:00 +0900
categories: iOS
tags: boostCourse
---

## 개요
> * [오토 레이아웃](#오토레이아웃)에 대해 공부합니다.
> * NSLayoutConstraint와 [Visual Format Language](#visual-format-language)를 알아본다.
> * [인터페이스 빌더에서 오토 레이아웃을 구현하는 방법](#인터페이스-빌더에서-오토레이아웃-구현하기)에 대해 공부한다.

---

## 오토레이아웃

오토레이아웃은 뷰의 제약 사항을 바탕으로 뷰 체계 내의 모든 뷰의 크기와 위치를 동적으로 계산한다.
오토레이아웃은 애플리케이션을 사용할 때 발생하는 외부 변경과 내부 변경에 동적으로 반응하는 사용자 인터페이스를 가능하게 한다. 오토레이아웃이 요구되는 외부 변경과 내부 변경에 대해 자세히 알아봅니다.

### 외부 변경(External Changes)

외부 변경은 **슈퍼 뷰의 크기나 모양이 변경될 때** 발생한다. 각각의 변화와 함께, 사용 가능한 공간을 가장 잘 사용할 수 있도록 뷰 체계의 레이아웃을 업데이트해줘야 한다.

다음은 외부 변경이 발생하는 경우입니다.
- 사용자가 아이패드의 분할뷰(Split View)를 사용하거나 사용하지 않는 경우(iOS).
- 장치를 회전하는 경우(iOS).
- 활성화콜(active call)과 오디오 녹음 바가 보여지거나 사라지는 경우(iOS).
- 다른 크기의 클래스를 지원하기 원하는 경우
- 다른 크기의 스크린을 지원하기 원하는 경우

이러한 변경사항은 대부분은 실행 시간에 발생할 수 있으며 애플리케이션으로부터 동적인 응답을 요구한다. 다른 스크린 크기를 지원하는 것과 같은 것은 애플리케이션이 다른 환경에 적응하는 것을 나타낸다. 스크린 크기가 일반적으로 실행 시간에 변경되지 않는다고 하더라도, 적응형 인터페이스를 만들면 애플리케이션이 아이폰 4S, 아이폰 6 플러스, 또는 아이패드에서도 모두 동일하게 잘 작동할 수 있다. 오토레이아웃은 아이패드 내부 변경에서 슬라이드와 분할뷰를 지원하는 핵심 요소이기도 하다.

### 내부 변경(Internal Changes)

내부 변경은 **사용자 인터페이스의 뷰의 크기 또는 설정이 변경되었을 때** 발생한다.

다음은 내부 변경이 발생하는 경우이다.
- 애플리케이션 변경에 의해 콘텐츠가 보여지는 경우
- 애플리케이션이 국제화를 지원하는 경우
- 애플리케이션이 동적 타입을 지원하는 경우

애플리케이션 콘텐츠가 변경됐을 때, 새로운 콘텐츠는 예전과 다른 레이아웃을 필요 할 수 있다. 새로운 애플리케이션이 각각의 신문 기사의 크기에 기반을 둔 레이아웃을 조정할 필요가 있는 경우와 같이, 텍스트 또는 이미지를 보여주는 애플리케이션에서 일반적으로 발생하는 일이다. 이와 비슷하게, 사진 콜라주는 이미지 크기와 영상의 가로 세로의 비율을 다뤄야만 한다.

### 오토레이아웃이 왜 필요할까?

오토레이아웃은 아래의 경우와 같이 인터페이스의 절대적인 좌표가 아닌 동적으로 상대적인 좌표가 필요한 경우에 유용합니다.

-   애플리케이션이 실행되는 iOS 기기의 스크린 화면의 크기가 다양한 경우.
-   애플리케이션이 실행되는 iOS 기기의 스크린이 회전할 수 있는 경우.
-   상태표시줄(Status Bar)에 전화 중임을 나타내는 액티브 콜(active call)과 오디오 녹음 중임을 나타내는 오디오 바가 보여지거나 사라지는 경우.
-   애플리케이션의 콘텐츠가 동적으로 보여지는 경우.
-   애플리케이션이 지역화(Localization)를 지원하는 경우.
-   애플리케이션이 동적 타입을 지원하는 경우.

### 오토레이아웃 속성

오토레이아웃의 속성은 정렬 사각형을 기반으로 합니다.

<img src="/assets/img/autolayout.png" height="450px" alt="autolayout" />

-   Width : 정렬 사각형의 너비
-   Height : 정렬 사각형의 높이
-   Top : 정렬 사각형의 상단
-   Bottom : 정렬 사각형의 하단
-   Baseline : 텍스트의 하단
-   Horizontal : 수평
-   Vertical : 수직
-   Leading : 리딩, 텍스트를 읽을 때 시작 방향
-   Trailing : 트레일링, 텍스트를 읽을 때 끝 방향
-   CenterX : 수평 중심
-   CenterY : 수직 중심

### 안전 영역(Safe Area)

-   안전 영역은 콘텐츠가 상태바, 내비게이션바, 툴바, 탭바를 가리는 것을 방지하는 영역이다. 표준 시스템이 제공하는 뷰들은 자동으로 안전 영역 레이아웃 가이드를 준수하게 되어있다.
-   기존의 상/하단 레이아웃 가이드(Top/Bottom Layout Guide)를 대체하며, 하위 버전에도 호환하여 작동한다.
    -   안전 영역은 iOS 11부터 사용할 수 있다.
    -   iOS 11 미만의 버전에서는 상/하단 레이아웃 가이드를 사용한다.

<img src="/assets/img/safeArea.png" height="450px" alt="safeArea" />

안전 영역 레이아웃 가이드는 UIView클래스의 var safeAreaLayoutGuide: UILayoutGuide로 접근할 수 있습니다.

### 제약(Constraint)

제약은 **뷰 스스로 또는 뷰 사이의 관계를 속성**을 통하여 정의한다. 제약은 방정식으로 나타낼 수 있습니다. 예제 방정식을 통해 자세히 알아본다.

<img src="/assets/img/constraints.png" height="300px" alt="constraints" />

-   Item1 : 방정식에 있는 첫 번째 아이템(B View) 이다. 첫 번째 아이템은 반드시 뷰 또는 레이아웃 가이드이어야 한다.
-   Attribute1 : 첫번째 아이템에 대한 속성이다. 이 경우, B View의 리딩이다.
-   Multiplier : 속성 2에 곱해지는 값이다. 이 경우 1.0 이다.
-   Item2 : 방정식에 있는 두 번째 아이템(A View) 이다.
-   Attribute2 : 두번째 아이템에 대한 속성이다. 이 경우, A View의 트레일이다.
-   Constant : 두번째 아이템의 속성에 더해지는 상수 값이다.

위의 예제 방정식의 제약을 해석하면 'B View의 리딩은 A View의 트레일링의 1.0배에 8.0을 더한 위치'가 된다.

### 고유 콘텐츠 크기(Intrinsic Content Size)

뷰의 고유 콘텐츠 크기는 뷰가 갖는 원래의 크기로 생각할 수 있다. 예를 들어 레이블의 고유 콘텐츠 크기는 레이블의 텍스트의 크기고, 이미지의 고유 콘텐츠 크기는 이미지 자체의 크기이다.

### 제약 우선도(Constraint Priorities)

오토레이아웃은 뷰의 고유 콘텐츠 크기를 각 크기에 대한 한 쌍의 제약을 사용하여 나타낸다. 우선도가 높을수록 다른 제약보다 우선적으로 레이아웃에 적용하며, 같은 속성의 다른 제약과 경합하는 경우, 우선도가 낮은 제약은 무시된다.

1.  콘텐츠 허깅 우선도(Content hugging priority) : 콘텐츠 고유 사이즈보다 뷰가 커지지 않도록 제한한다. 다른 제약사항보다 우선도가 높으면 뷰가 콘텐츠 사이즈보다 커지지 않는다.
2.  콘텐츠 축소 방지 우선도(Content compression resistance priority) : 콘텐츠 고유 사이즈보다 뷰가 작아지지 않도록 제한합니다. 다른 제약사항보다 우선도가 높으면 뷰가 콘텐츠 사이즈보다 작아지지 않는다.

### 레이아웃 마진

뷰에 콘텐츠 내용을 레이아웃할 때 사용하는 기본 간격(default spacing)이다.
-   레이아웃 마진 가이드(Layout Margins Guide) : 레이아웃 마진에 따라 형성되는 사각의 프레임 영역

### 앵커(Anchor)

오토레이아웃을 코딩으로 구현하여 제약(Constraint)을 만들기 위해 앵커(Anchor)를 사용할 수 있습니다. 

```Swift

button.translatesAutoresizingMaskIntoConstraints = false

var constraintX: NSLayoutConstraint
constraintX = button.centerXAnchor.constraint(equalTo: self.view.centerXAnchor)

var constraintY: NSLayoutConstraint
constraintY = button.centerYAnchor.constraint(equalTo: self.view.centerYAnchor)

constraintX.isActivte = true
constraintY.isActivte = true

```

`translatesAutoresizingMaskIntoConstraints` : 오토레이아웃이 도입되기 전 뷰를 유연하게 표현할 수 있도록 오토리사이징 마스크를 사용했다. 오토레이아웃을 사용하면 기존의 오토리사징 마스크가 가지고 있던 제약 조건이 자동으로 추가되기 때문에 충돌 가능성이 생긴다. 그래서 이 값을 false로 지정한 뒤 오토레이아웃을 적용한다. 참고로, 인터페이스 빌더에서 오토레이아웃을 적용한 경우 자동으로 값이 false로 설정된다.

#### 앵커 관련 프로퍼티

```swift
var constraints: [NSLayoutConstraint]
// 뷰에 부여한 제약사항들은 담은 배열

var bottomAnchor: NSLayoutYAxisAnchor { get }
// 뷰 프레임의 하단부 레이아웃 앵커

var centerXAnchor: NSLayoutXAxisAnchor { get }
// 뷰 프레임의 수평 중심부 레이아웃 앵커

var centerYAnchor: NSLayoutYAxisAnchor { get }
// 뷰 프레임의 수직 중심부 레이아웃 앵커

var heightAnchor: NSLayoutDimension { get }
// 뷰 프레임의 높이를 가리키는 레이아웃 앵커

var leadingAnchor: NSLayoutXAxisAnchor { get }
// 뷰 프레임의 리딩을 가리키는 레이아웃 앵커

var topAnchor: NSLayoutYAxisAnchor { get }
// 뷰 프레임의 상단부 레이아웃 앵커

var trailingAnchor: NSLayoutXAxisAnchor { get }
// 뷰 프레임의 트레일링을 가리키는 레이아웃 앵커

var widthAnchor: NSLayoutDimension { get }
// 뷰 프레임의 넓이를 가리키는 레이아웃 앵커
```
---
## Visual Format Language
visual format language를 사용해 제약조건을 지정하는 방법도 있다.

<img src="/assets/img/visualFormatLanguage.png" height="450px" alt="visualFormatLanguage"/>

---
## 인터페이스 빌더에서 오토레이아웃 구현하기
총 세 가지의 방법이 있다.
1. 뷰와 뷰 사이에 ctrl 키를 누르고 드래그하는 방식
2. 스택, 정렬, 핀 그리고 리졸브를 사용하는 방식
3. 인터페이스 빌더가 제약 설정하는 방식

그 중 2번 방식에 대해 알아보자.

### 스택, 정렬, 핀 그리고 리졸브 툴 사용

<p align="center">
<img src="/assets/img/autoLayoutTool.png" height="250px" alt="autoLayoutTool"/>
</p>

#### Stack Tool

스택 뷰를 생성할 수 있게 해준다. 레이아웃에서 하나 이상의 아이템을 선택한 후, 스택툴을 클릭하면 된다. 인터페이스 빌더는 스택뷰에 선택된 아이템을 추가하고, 스택을 콘텐츠의 최근 피팅 사이즈에 맞게 크기를 재설정한다.

#### Align

정렬하려는 뷰를 선택한 뒤, 정렬 툴을 선택하면 나오는 팝오버 창에서 설정한다. 보통 두 개 혹은 그 이상의 뷰를 정렬 툴을 사용하기 전에 선택한다. 하지만, 수평적 혹은 수직적으로 컨테이너 내에 있는 제약은 하나의 뷰에 추가될 수 있다. 

#### pin

핀 툴은 뷰의 이웃과 연관된 뷰의 위치 또는 그 크기를 재빠르게 정의하도록 한다. 고정되기 원하는 아이템의 위치나 크기를 선택하고 핀 툴을 클릭하면 팝오버 창이 나타난다. 

팝오버의 윗 부분은 선택된 아이템의 리딩, 위, 트레일링, 또는 아래 엣지를 가장 가까운 곳에 고정할 수 있도록 한다. 관련된 숫자는 현재 캔버스에 있는 아이템 사이의 공간을 나타낸다. 커스텀 공간에 입력해줄 수도 있고 어떤 뷰가 제약되어야 하는지를 설정하거나 기본 공간을 선택하기 위해 더 보기 삼각형을 클릭할 수도 있다. Constraint to margins 체크박스는 슈퍼 뷰가 슈퍼 뷰의 여백 또는 엣지를 사용하는 것을 제약할 것인지의 여부를 결정한다.

팝오버의 아래 부분은 아이템의 너비 또는 높이를 설정할 수 있게 해준다. 너비와 높이 제약은 다른 값으로 변경해줄 수 있지만, 현재 캔버스 크기를 기본값으로 한다. 화면 비율 제약은 아이템의 현재 화면 비율을 사용하지만, 이 값을 변경해주고 싶다면 일단 생성한 후에 제약을 다시 살펴보고 편집해야 한다.
일반적으로, 고정할 하나의 뷰를 선택한다. 하지만, 두 개 혹은 그 이상의 뷰를 선택할 수 있고 뷰에 모두 같은 너비 또는 높이를 줄 수도 있다. 또한 한 번에 여러 개의 제약을 생성할 수 있으며 제약을 추가함에 따라 프레임을 업데이트할 수 있다. 

#### reserve

리졸브 툴은 오토레이아웃 문제 해결을 위한 도구로, 일반적인 오토레이아웃의 문제를 고치는 몇 가지 옵션을 제공한다. 메뉴 절반 위의 옵션은 현재 선택된 뷰에 한해 영향을 준다. 절반 아래의 옵션은 scene에 있는 모든 뷰에 영향을 준다. 
현재 제약에 기반한 뷰 프레임이나 캔버스에 있는 뷰의 현재 위치에 기반한 제약을 업데이트 할 때 이 툴을 사용할 수 있다. 또한 빠진 제약을 추가해줄 수 있고 제약을 삭제하거나 인터페이스 빌더에 의해 추천된 제약 묶음에 뷰를 재설정해줄 수도 있다.

