---
layout: post
title:  "StackView"
date:   2020-05-28 23:32:00 +0900
categories: iOS
tags: boostCourse
---

## 개요
> * stackView


## UIStackView
여러 뷰들의 수평 또는 수직 방향의 선형적인 레이아웃의 인터페이스를 사용할 수 있도록 해준다. 스택뷰와 오토레이아웃 기능을 활용하여 디바이스의 방향과 화면크기에 따라 동적으로 적응할 수 있는 UI를 만들 수 있다. 스택뷰의 레이아웃은 스택뷰의 'axis', 'distribution', 'alignment', 'spacing'과 같은 프로퍼티를 통해 조정한다.

<img src="/assets/img/stackview.png" alt="stackview property" />

### 수평(horizontal) axis 스택뷰 모식도
스택뷰는 'arrangeSubviews' 프로퍼티에 스택뷰에 포함된 뷰들을 관리한다. 이 프로퍼티는 순서가 있는 배열과 같다. 스택뷰에 포함된 뷰의 순서가 레이아웃에 영향을 미친다.

> horizontal : 1 2 3 <br />
> vertical :  
> 1 <br />
> 2 <br />
> 3
				
### 주요 프로퍼티
-   `var arrangedSubviews: [UIView]` : 스택뷰의 정렬된 뷰의 배열입니다. 스택뷰에 포함된 뷰들을 이 프로퍼티에 저장하고 관리합니다.
-   `var axis: UILayoutConstraintAxis` : 레이아웃의 방향을 결정합니다.(수직 vertical, 수평 horizontal)
-   `var distribution: UIStackViewDistribution` : 스택뷰에 포함된 뷰가 스택뷰 내에서 어떻게 배치(분배)될지 결정합니다.
-   `var spacing: CGFloat` : 스택뷰에 정렬된 뷰들 사이의 간격을 결정합니다. 기본 값은 0.0 입니다.

### UIStackView 클래스의 주요 메서드

-   `func addArrangeSubview(UIView)`:  `arrangedSubviews`  배열에 마지막 요소에 뷰를 추가합니다.
-   `func insertArrangedSubview(UIView, at: Int)`:  `arrangedSubviews`  배열의 특정 인덱스에 뷰를 추가합니다.
-   `func removeArrangedSubview(UIView)`: 스택뷰의  `arrangedSubviews`  배열로부터 뷰를 제거합니다.

## 참고 링크

[UIStackView - UIKit | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uistackview)


