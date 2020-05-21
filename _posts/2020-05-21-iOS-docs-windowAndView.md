---
layout: post
title:  "Window and Views(Editing)"
date:   2020-05-15 21:15:00 +0900
categories: iOS
tags: docs
---

[본문](#https://developer.apple.com/library/archive/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/WindowsandViews/WindowsandViews.html#//apple_ref/doc/uid/TP40009503-CH2-SW1)

# Introduction

iOS에서 화면에 애플리케이션의 콘텐츠를 나타내기 위해 윈도우와 뷰를 사용한다. **윈도우**는 그 자체로 콘텐츠를 표할 수 없지만 애플리케이션의 뷰를 위한 **컨테이너** 역할을 한다. **뷰**는 **UIView 클래스 또는 UIView 클래스의 하위 클래스(Subclass)의 인스턴스**로 윈도우의 한 영역에서 **콘텐츠를 보여준다**. 뷰가 나타낼 수 있는 콘텐츠는 이미지, 문자, 도형 등과 같이 다양하다. 뷰는 또 다른 뷰를 관리하고 구성하기 위해 사용되기도 한다.

## At a Glance (한 눈에 보기)
모든 애플리케이션은 컨텐츠를 표시하기 위한 하나 이상의 window와 하나 이상의 view가 있다. UIKit 및 기타 시스템 프레임워크는 컨텐츠를 표시하는데 사용할 수 있는 사전 정의된 view를 제공한다. 이러한 view는 간단한 button이나 text label부터 table view, picker view, scroll view와 같은 좀 더 복잡한 view까지 다양하게 존재한다. 이 view 들 중에 필요한 것이 없다면, ㅏ사용자 정의 view를 정의하고 직접 이벤트 핸들링, 관리를 할 수 있다.

## 등등
더 있지만 그냥 넘어가고 본론 보자.

# View and Window Architecture

Views and windows 는 애플리케이션의 사용자 인터페이스를 나타내고 해당 인터페이스와의 상호 작용을 처리한다. UIKit and other system frameworks 는 그대로 사용할 수 있는 여러 view를 제공한다. 물론, 직접 view를 만들어 사용할 수도 있다.

무엇을 사용하든,  `[UIView](https://developer.apple.com/documentation/uikit/uiview)`  와 `[UIWindow](https://developer.apple.com/documentation/uikit/uiwindow)`  클래스가 제공하는 것을 이해해야 한다. 이 클래스들은 뷰의 레이아웃 및 프리젠테이션을 관리하기 위한 정교한 기능을 제공한다. 이러한 기능의 작동하는 방식을 이해하는 것은 애플리케이션에서 변경이 발생할 때 뷰가 올바르게 작동하도록 하는데 중요하다.

(여기까지 헀다)

## View Architecture Fundamentals
Most of the things you might want to do visually are done with view objects—instances of the  `[UIView](https://developer.apple.com/documentation/uikit/uiview)`  class. A view object defines a rectangular region on the screen and handles the drawing and touch events in that region. A view can also act as a parent for other views and coordinate the placement and sizing of those views. The  `UIView`  class does most of the work in managing these relationships between views, but you can also customize the default behavior as needed.

Views work in conjunction with Core Animation layers to handle the rendering and animating of a view’s content. Every view in UIKit is backed by a layer object (usually an instance of the  `[CALayer](https://developer.apple.com/documentation/quartzcore/calayer)`  class), which manages the backing store for the view and handles view-related animations. Most operations you perform should be through the  `UIView`  interface. However, in situations where you need more control over the rendering or animation behavior of your view, you can perform operations through its layer instead.

To understand the relationship between views and layers, it helps to look at an example.  Figure 1-1  shows the view architecture from the  _[ViewTransitions](https://developer.apple.com/library/archive/samplecode/ViewTransitions/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007411)_  sample application along with the relationship to the underlying Core Animation layers. The views in the application include a window (which is also a view), a generic  `UIView`  object that acts as a container view, an image view, a toolbar for displaying controls, and a bar button item (which is not a view itself but which manages a view internally). (The actual  _[ViewTransitions](https://developer.apple.com/library/archive/samplecode/ViewTransitions/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007411)_  sample application includes an additional image view that is used to implement transitions. For simplicity, and because that view is usually hidden, it is not included in  Figure 1-1.) Every view has a corresponding layer object that can be accessed from that view’s  `[layer](https://developer.apple.com/documentation/uikit/uiview/1622436-layer)`  property. (Because a bar button item is not a view, you cannot access its layer directly.) Behind those layer objects are Core Animation rendering objects and ultimately the hardware buffers used to manage the actual bits on the screen.

<img src="figure1.png" />

The use of Core Animation layer objects has important implications for performance. The actual drawing code of a view object is called as little as possible, and when the code is called, the results are cached by Core Animation and reused as much as possible later. Reusing already-rendered content eliminates the expensive drawing cycle usually needed to update views. Reuse of this content is especially important during animations, where the existing content can be manipulated. Such reuse is much less expensive than creating new content.

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