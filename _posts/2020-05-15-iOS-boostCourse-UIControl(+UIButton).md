---
layout: post
title:  "UIControl(+UIButton)"
date:   2020-05-15 20:21:00 +0900
categories: iOS
tags: boostCourse
---

## 개요

> * [UIControl](http://hyex.github.io/ios/2020/05/15/iOS-docs-UIControl)이 무엇인지 이해한다.
> * UIControl.event 의 몇 가지 종류를 알아본다. 
> * UIButton 에 대해 간단하게 소개하고, IBAction과 addTarget 으로 action 메서드와 연결한다. 
  
---
## UIControl

UIKit에는 UIButton, UISwitch, UIStepper 등 UIControl을 상속받은 다양한 컨트롤 클래스가 있다. 그런 컨트롤 객체에 발생한 다양한 이벤트 종류를 특정 액션 메서드에 연결할 수 있다.

**즉, 컨트롤 객체에서 특정 이벤트가 발생하면, 미리 지정해 둔 타겟의 액션을 호출한다.**

## UIControl.Event
컨트롤 이벤트는 UIControl에 Event라는 타입으로 정의되어 있다.

### category
usage : UIControl.Event.event_type ex) UIControl.Event.touchDown

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

## UIButton 

### 버튼과 메서드 연결하는 방법
1. addTarget(_: action: for: ) 메서드 사용

```swift
func addPlayPauseButton() {

	let button: UIButton = UIButton(type: UIButton.ButtonType.custom)
button.translatesAutoresizingMaskIntoConstraints = **false**
	self.view.addSubview(button)

	button.setImage(UIImage(named: "button_play"), for: UIControl.State.normal)
	button.setImage(UIImage(named: "button_pause"), for: UIControl.State.selected)

	button.addTarget(self, action: #selector(self.touchUpPlayPauseButton(**_**:)), for: UIControl.Event.touchUpInside)

	let centerX: NSLayoutConstraint
	centerX = button.centerXAnchor.constraint(equalTo: self.view.centerXAnchor)
	let centerY: NSLayoutConstraint
	centerY = NSLayoutConstraint(item: button, attribute: NSLayoutConstraint.Attribute.centerY, relatedBy: NSLayoutConstraint.Relation.equal, toItem: **self**.view, attribute: NSLayoutConstraint.Attribute.centerY, multiplier: 0.8, constant: 0)

	let width: NSLayoutConstraint
	width = button.widthAnchor.constraint(equalTo: self.view.widthAnchor, multiplier: 0.5)
	let ratio: NSLayoutConstraint
	ratio = button.heightAnchor.constraint(equalTo: button.widthAnchor, multiplier: 1)

	centerX.isActive = true
	centerY.isActive = true
	width.isActive = true
	ratio.isActive = true

	self.playPauseButton = button
```

2. 인터페이스 빌더에서 연결 (@IBAction)

```swift
@IBOutlet weak var playPauseButton: UIButton!

@IBAction func touchUpPlayPauseButton(_ sender: UIButton) {

	sender.isSelected = !sender.isSelected

	if sender.isSelected {
		self.player?.play()
	} else {
		self.player?.pause()
	}

	if sender.isSelected {
		self.makeAndFireTimer()
	} else {
		self.invalidateTimer()
	}
}
```

### 버튼의 상태
- 종류 : default, highlighted, focused, selected, disabled
- 버튼의 상태는 조합된 상태일 수 있다.
- 프로그래밍 방식 또는 인터페이스 빌더를 이용해 버튼의 각 상태에 대한 속성을 별도로 지정할 수 있다.

### 버튼 주요 프로퍼티

`enum UIButtonType`
- 버튼의 유형
- 처음 버튼을 생성할 때 init(type:) 메서드를 이용하거나, 인터페이스 빌더의 'Attribute Inspector' 에서 버튼 유형을 지정할 수 있다.
- 한 번 생성된 버튼의 유형은 이후 변경할 수 없다.
- 가장 많이 사용하는 유형은 Custom과 System이지만 필요에 따라 다른 유형(Detail Disclosure, Info Light, Info Dark, Add Contact)를 사용할 수 있다.

`var titleLabel: UILabel?`

`var imageView: UIImageView?`

`var tintColor: UIColor!` : 버튼 타이틀과 이미지의 틴트 컬러

### 버튼 주요 메서드

```swift
// 특정 상태의 버튼의 문자열 설정
func setTitle(String?, for: UIControlState)
// 특정 상태의 버튼의 문자열 반환
func title(for: UIControlState) -> String?
// 특정 상태의 버튼 이미지 설정
func setImage(UIImage?, for: UIControlState)
// 특정 상태의 버튼 이미지 반환
func image(for: UIControlState) -> UIImage?
// 특정 상태의 백그라운드 이미지 설정
func setBackgroundImage(UIImage?, for: UIControlState)
// 특정 상태의 백그라운드 이미지 반환
func backgroundImage(for: UIControlState) -> UIImage?
// 특정 상태의 문자열 색상 설정
func setTitleColor(UIColor?, for: UIControlState)
// 특정 상태의 attributed 문자열 설정
func setAttributedTitle(NSAttributedString?, for: UIControlState)

```


### Link

[애플 공식 문서 - UIControl](https://developer.apple.com/documentation/uikit/uicontrol)
[애플 공식 문서 - UIButton](https://developer.apple.com/documentation/uikit/uibutton)