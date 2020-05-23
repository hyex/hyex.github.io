---
layout: post
title:  "Window and Views(Editing)"
date:   2020-05-15 21:15:00 +0900
categories: iOS
tags: docs
---

## [본문 링크](#https://developer.apple.com/library/archive/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/WindowsandViews/WindowsandViews.html#//apple_ref/doc/uid/TP40009503-CH2-SW1)

# Introduction

iOS에서 화면에 애플리케이션의 콘텐츠를 나타내기 위해 윈도우와 뷰를 사용한다. **윈도우**는 그 자체로 콘텐츠를 표할 수 없지만 애플리케이션의 뷰를 위한 **컨테이너** 역할을 한다. **뷰**는 **UIView 클래스 또는 UIView 클래스의 하위 클래스(Subclass)의 인스턴스**로 윈도우의 한 영역에서 **콘텐츠를 보여준다**. 뷰가 나타낼 수 있는 콘텐츠는 이미지, 문자, 도형 등과 같이 다양하다. 뷰는 또 다른 뷰를 관리하고 구성하기 위해 사용되기도 한다.

## At a Glance (한 눈에 보기)
모든 애플리케이션은 컨텐츠를 표시하기 위한 하나 이상의 window와 하나 이상의 view가 있다. UIKit 및 기타 시스템 프레임워크는 컨텐츠를 표시하는데 사용할 수 있는 사전 정의된 view를 제공한다. 이러한 view는 간단한 button이나 text label부터 table view, picker view, scroll view와 같은 좀 더 복잡한 view까지 다양하게 존재한다. 이 view 들 중에 필요한 것이 없다면, ㅏ사용자 정의 view를 정의하고 직접 이벤트 핸들링, 관리를 할 수 있다.

## 등등
더 있지만 그냥 넘어가고 본론 보자.

# View and Window Architecture

Views and windows 는 애플리케이션의 사용자 인터페이스를 나타내고 해당 인터페이스와의 상호 작용을 처리한다. UIKit and other system frameworks 는 그대로 사용할 수 있는 여러 view를 제공한다. 물론, 직접 view를 만들어 사용할 수도 있다.

무엇을 사용하든,  [UIView](https://developer.apple.com/documentation/uikit/uiview)  와 [UIWindow](https://developer.apple.com/documentation/uikit/uiwindow)  클래스가 제공하는 것을 이해해야 한다. 이 클래스들은 뷰의 레이아웃 및 프리젠테이션을 관리하기 위한 정교한 기능을 제공한다. 이러한 기능의 작동하는 방식을 이해하는 것은 애플리케이션에서 변경이 발생할 때 뷰가 올바르게 작동하도록 하는데 중요하다.


## View Architecture Fundamentals
### (아키텍처의 기본 사항)
시각적으로 할 수 있는 대부분의 것들은  [UIView](https://developer.apple.com/documentation/uikit/uiview)  클래스의 객체 인스턴스를 사용하여 수행된다. view 객체는 화면에서 직사각형 영역을 정의하고, 그 영역의 터치 이벤트와 그리기를 처리한다. view는 다른 view의 부모로서 행동할 수 있고, 그러한 뷰의 배치와 크기를 조정할 수 있다. UIView 클래스는 view 사이의 관계를 관리하는 대부분의 일을 하지만, 필요에 따라 기본 동작을 사용자 정의 할 수 있다.

뷰는 Core Animation layer와 함께 작동하여 뷰 컨텐츠의 렌더링 및 애니메이션을 처리한다. UIKit의 모든 뷰는, 뷰의 백업 저장소를 관리하고 뷰와 관련된 애니메이션을 처리하는 레이어 객체(보통 [CALayer](https://developer.apple.com/documentation/quartzcore/calayer) 클래스의 인스턴스) 에 의해 백업된다. 당신이 수행하는 대부분의 작업은 UIView 인스턴스를 통해 이루어져야 한다. 그러나 뷰의 렌더링 또는 애니메이션 동작을 더 많이 제어해야 하는 상황에서는, 레이어를 사용해 작업을 수행할 수 있다.

view와 layer의 관계를 이해하기 위해서, 이 예제를 보는 것이 도움이 될 것이다. Figure 1-1는 [ViewTransitions](https://developer.apple.com/library/archive/samplecode/ViewTransitions/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007411)  샘플 애플리케이션의 뷰 아키텍처와 기본 core animation layers의 관계를 보여준다. 이 애플리케이션에서의 뷰는 window(view이기도 함), 컨테이너 뷰의 역할을 하는 일반 UIView 객체, 이미지 뷰, 컨트롤을 표시하는 툴바, 바 버튼 아이템(뷰 자체는 아니지만 내부적으로 뷰를 관리하는 항목)을 포함한다. (실제  _[ViewTransitions](https://developer.apple.com/library/archive/samplecode/ViewTransitions/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007411)_  샘플 어플리케이션은 전환을 구현하는데 사용되는 추가적인 이미지 뷰를 포함한다. 단순화를 위해, 그리고 뷰는 보통 숨겨져 있기 때문에, Figure 1-1에서는 보여지지 않는다. 모든 뷰는 뷰의 [layer](https://developer.apple.com/documentation/uikit/uiview/1622436-layer)의 대응하는 레이어 객체를 가진다. 이 객체는 해당 뷰의 레이어 속성에서 액세스할 수 있다. ( 바 버튼 아이템은 뷰가 아니기 때문에, 레이어로 직접 접근할 수 없다) 이러한 레이어 객체의 뒤는 Core Animation 렌더링 객체와 궁극적으로 화면의 실제 bits를 관리하는데 사용되는 하드웨어 버퍼가 있다.

<img src="/assets/img/figure1-1.png" alt="figure1-1"/>

Core Animation layer 객체의 사용은 성능에 중요한 역향을 미친다. 뷰 객체의 실제 드로잉 코드는 가능한 적게 호출되며, 코드가 호출될 때 결과는 Core Animation에 의해 캐시되고 나중에 최대한 많이 재사용된다. 이미 렌더링된 컨텐츠를 재사용하는 것은 일반적으로 뷰를 업데이트하는데 필요한 비싼 그리기 주기가 필요없게 한다. 컨텐츠의 재사용은 특히 이미 존재하는 컨텐츠를 조작할 수 있는 애니메이션을 할 때 중요하다. 이러한 재사용은 새로운 컨텐츠를 만드는 것보다 훨씬 저렴하다.

(여기까지 헀다)

### View Hierachies and Subview Management
In addition to providing its own content, a view can act as a container for other views. When one view contains another, a parent-child relationship is created between the two views. The child view in the relationship is known as the  _subview_  and the parent view is known as the  _superview_. The creation of this type of relationship has implications for both the visual appearance of your application and the application’s behavior.

Visually, the content of a subview obscures all or part of the content of its parent view. If the subview is totally opaque, then the area occupied by the subview completely obscures the corresponding area of the parent. If the subview is partially transparent, the content from the two views is blended together prior to being displayed on the screen. Each superview stores its subviews in an ordered array and the order in that array also affects the visibility of each subview. If two sibling subviews overlap each other, the one that was added last (or was moved to the end of the subview array) appears on top of the other.

The superview-subview relationship also impacts several view behaviors. Changing the size of a parent view has a ripple effect that can cause the size and position of any subviews to change too. When you change the size of a parent view, you can control the resizing behavior of each subview by configuring the view appropriately. Other changes that affect subviews include hiding a superview, changing a superview’s alpha (transparency), or applying a mathematical transform to a superview’s coordinate system.

The arrangement of views in a view hierarchy also determines how your application responds to events. When a touch occurs inside a specific view, the system sends an event object with the touch information directly to that view for handling. However, if the view does not handle a particular touch event, it can pass the event object along to its superview. If the superview does not handle the event, it passes the event object to its superview, and so on up the responder chain. Specific views can also pass the event object to an intervening  responder object, such as a view controller. If no object handles the event, it eventually reaches the application object, which generally discards it.

For more information about how to create view hierarchies, see  [Creating and Managing a View Hierarchy](https://developer.apple.com/library/archive/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/CreatingViews/CreatingViews.html#//apple_ref/doc/uid/TP40009503-CH5-SW47).

### The View Drawing Cycle
Later.
### Content Modes
Later.
### Stretchable Views
Later.
### Built-In Animation Support
Later.
## View Geometry and Coordinate Systems
Later.
## The Runtime Interaction Model for Views
Later.
## Tips for Using Views Effectively
Later.
# Windows
Later.
## Tasks That involve Windows
## Creating and Configuring a Window
## Monitoring Window Changes
## Displaying Content on an External Display
# Views
## Creating and Configuring View Objects
next !!!
## Creating and Managing a View Hierarchy
Later.
## Adjusting the Size and Position of Views at Runtime
Later.
## Modifying Views at Runtime
Later.
## Interacting with Core Animation Layers
Later.
## Defining a Custom View
Later.
# Animations
Later.
## What Can Be Animated?
## Animating Property Changes in a View
## Creating Animated Transitions Between Views
## Linking Multiple Animations Together
## Animating View and Layer Changes Together