---
layout: post
title:  "Storyboard segue"
date:   2020-06-08 20:13:00 +0900
categories: iOS
tags: boostCourse
---

## 개요
> * Segue
> * UIStoryboardSegue

## Segue
세그는 스토리보드에서 뷰 컨트롤러 사이의 화면전환을 위해 사용하는 객체이다.

## UIStoryboardSegue 클래스
UIKit에서 사용할 수 있는 표준 화면 전환을 위한 프로퍼티와 메서드를 포함하고 있다. 또 커스텀 전환을 정의하기 위해 서브클래스를 구현해서 사용할 수 도 있다. 필요에 따라 UIViewController의 performSegue(withIdentifier: sender:) 메서드를 사용하여 세그 객체를 코드로 실행할 수 있다.
Segue 객체는 뷰 컨트롤러의 뷰 전환과 관련도니 정보를 가지고 있다. 세그는 뷰 컨트롤러의 뷰를 다른 뷰 컨트롤러의 뷰로 전환할 때 뷰 컨트롤러의 prepare(for: sender:) 메서드를 사용하여 새로 보여지는 뷰 컨트롤러에 데이터를 전달할 수 있다.

### 주요 프로퍼티
var source: UIViewController : 세그에 전환을 요청하는 VC
var destination: UIViewController : 전환될 VC
var identifier: String? : 세그 객체의 식별자

## 주요 메서드
func perforom() : VC의 전환을 수행

### Segue 종류
Show, Show Detail, Present Modally, Present As Popover, Custom
Show : 현재 기기나 화면 상태에서 가장 적절한 화면전환 방식을 판단하여 전환

## 참고 링크

[UIStoryboardSegue - UIKit - Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uistoryboardsegue)

[View Controller Programming Guide for iOS: Using Segues](https://developer.apple.com/library/archive/featuredarticles/ViewControllerPGforiPhoneOS/UsingSegues.html)

