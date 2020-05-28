---
layout: post
title:  "Singleton"
date:   2020-05-28 23:20:00 +0900
categories: iOS
tags: boostCourse
---

## 개요
> * [싱글턴](#singleton) 패턴
> * 싱글턴 패턴을 활용하는 Cocoa 프레임워크의 클래스들

## SingleTon
특정 클래스의 인스턴스가 오직 하나임을 보장하는 객체를 의미한다. 싱글턴은 애플리케이션이 요청한 횟수와는 관계없이 이미 생성된 같은 인스턴스를 반환한다. 즉, 애플리케이션 내에서 특정 클래스의 인스턴스가 딱 하나만 있기 때문에 다른 인스턴스들이 공유해서 사용할 수 있다.

<img src="/assets/img/singleton.png" alt="singleton" />

## Cocoa 프레임워크에서의 싱글턴 디자인 패턴
Cocoa 프레임워크에서 싱글턴 디자인 패턴을 활용하는 대표적인 클래스를 소개한다.
싱글턴 인스턴스를 반환하는 팩토리 메서드나 프로퍼티는 일반적으로 shared라는 이름을 사용한다.

-   [`FileManager`](https://developer.apple.com/documentation/foundation/filemanager)
    -   애플리케이션 파일 시스템을 관리하는 클래스입니다.
    -   `FileManager.default`
-   [`URLSession`](https://developer.apple.com/documentation/foundation/urlsession)
    -   URL 세션을 관리하는 클래스입니다.
    -   `URLSession.shared`
-   [`NotificationCenter`](https://developer.apple.com/documentation/foundation/notificationcenter)
    -   등록된 알림의 정보를 사용할 수 있게 해주는 클래스입니다.
    -   `NotificationCenter.default`
-   [`UserDefaults`](https://developer.apple.com/documentation/foundation/userdefaults)
    -   Key-Value 형태로 간단한 데이터를 저장하고 관리할 수 있는 인터페이스를 제공하는 데이터베이스 클래스입니다.
    -   `UserDefaults.standard`
-   [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication)
    -   iOS에서 실행되는 중앙제어 애플리케이션 객체입니다.
    -   `UIApplication.shared`

**주의할 점**
싱글턴 디자인 패턴은 애플리케이션 내의 특정 클래스의 인스턴스가 하나만 존재하기 때문에 객체가 불필요하게 여러 개 만들어질 필요가 없는 경우에 많이 사용한다. 예를 들면 환경설정, 네트워크 연결관리, 데이터 관리 등이 있다. 하지만 멀티 스레드 환경에서 동시에 싱글턴 객체를 참조할 경우 원치 않은 결과를 가져올 수 있다. 

## 참고 링크
[Singleton - Apple Developer](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Singleton.html)

[Using Swift with Cocoa and Objective-C : Adopting Cocoa Design Patterns - Singleton](https://developer.apple.com/library/content/documentation/Swift/Conceptual/BuildingCocoaApps/AdoptingCocoaDesignPatterns.html#//apple_ref/doc/uid/TP40014216-CH7-ID177)
