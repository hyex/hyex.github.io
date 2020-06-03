---
layout: post
title:  "View reuse"
date:   2020-06-03 19:46:00 +0900
categories: iOS
tags: boostCourse
---

## 개요
> * 뷰의 재사용
> * 뷰의 재사용 원리

---
## 뷰의 재사용
iOS 기기는 한정된 메모리를 가지고 애플리케이션을 구동하기 때문에, 뷰를 재사용함으로써 메모리를 절약하고 성능 또한 향상할 수 있다.

### 재사용의 대표적인 예
UITableViewCell과 UICollectionViewCell 등이 있다.

## 뷰의 재사용 원리
1. 테이블뷰/컬렉션뷰에서 셀을 표시하기 위해 데이터 소스에 뷰(셀) 인스턴스를 요청한다.
2. 데이터 소스는 요청마다 새로운 셀을 만드는 대신 재사용 큐 (Reuse Queue)에 재사용을 위해 대기하고 있는 셀이 있는지 확인 후 있으면 그 셀에 새로운 데이터를 설정하고, 없으면 새로운 셀을 생성한다.
3. 테이블뷰 및 컬렉션뷰는 데이터 소스가 셀을 반환하면 화면에 표시한다.
4. 사용자가 스크롤을 하게 되면 일부 셀들이 화면 밖으로 사라지면서 다시 재사용 큐에 들어간다.
5. 1번-4번의 과정이 계속 반복된다.

<img src="/assets/img/viewReuse.png" alt="view(cell) reuse" />

## 참고 링크

[UITableView - UIKit - Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uitableview)


[UICollectionView - UIKit - Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uicollectionview)


