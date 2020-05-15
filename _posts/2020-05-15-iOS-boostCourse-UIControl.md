---
layout: post
title:  "UIControl.Event"
date:   2020-05-15 20:21:00 +0900
categories: iOS
tags: boostCourse
---

## 개요
* IBAction을 사용해서 인터페이스 빌더에서 생성한 객체가 전달하는 이벤트를 연결한다.
* 객체에서 발생한 액션의 타입이 여러 종류가 있음을 이해한다.

## UIControl
UIKit에는 UIButton, UISwitch, UIStepper 등 UIControl을 상속받은 다양한 컨트롤 클래스가 있다. 그런 컨트롤 객체에 발생한 다양한 이벤트 종류를 특정 액션 메서드에 연결할 수 있다.
즉, 컨트롤 객체에서 특정 이벤트가 발생하면, 미리 지정해 둔 타겟의 액션을 호출한다.

## UIControl.Event


## 컨트롤 이벤트의 종류
컨트롤 이벤트는 UIControl에 Event라는 타입으로 정의되어 있다.
사용 : UIControl.Event.event_type ex) UIControl.Event.touchDown
| name | mean |
|:--:|:--:|
|touchDown|터치|
|touchDownRepeat|연속 터치|
|touchDragInside|범위 내 드래그|
|touchDragOutside|범위 외에서 드래그|
|touchDragExit|범위 일정 영역 바깥쪽으로 나갔을 때|
|touchUpInside|터치 후 뗐을 때|
|touchUpOutside|안쪽에서 터치 후 바깥쪽에서 뗐을 때|
|touchCancel|터치 취소|
이 외에도 다양한 이벤트가 존재한다.
