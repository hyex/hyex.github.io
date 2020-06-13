---
layout: post
title:  "4-1 Photos framework"
date:   2020-06-10 16:22:00 +0900
categories: iOS
tags: boostCourse
---

## 개요
> * Photos 프레임워크
> * 에셋

## Photos 
iOS, macOS에서 사진 애플리케이션, 사지 확장 기능을 지원하는 클래스를 제공
iOS, tvOS에서 iCloud 사진 라이브러리를 포함하여 사진 및 비디오에 직접 접근을 가능하게 해준다. 

### 특징 및 개념
**에셋**
<img src=/assets/img/boostCourse/4-1-asset.png />

**에셋 컬렉션**
<img src=/assets/img/boostCourse/4-1-assetCollection.png />

**컬렉션 리스트**
<img src=/assets/img/boostCourse/4-1-collectionList.png />


- 객체 가져오기 및 변경요청.
    - Photos 프레임워크 모델 클래스 (PHAsset, PHAssetCollection, PHCollectionList)의 인스턴스는 사진 애플리케이션에서 에셋(이미지, 비디오, 라이브 포토), 에셋 컬렉션(앨범, 특별한 순간) 및 사용자가 작업하는 항목을 나타냅니다. 그리고 컬렉션 리스트(앨범 폴더, 특별한 순간)입니다. 이 객체는 읽기 전용이며 변경할 수 없습니다. 그리고 메타 데이터만 포함합니다.
해당 객체를 사용하여 작업해야 하는 데이터를 가져와서 에셋 및 컬렉션 작업을 할 수 있습니다. 변경 요청을 하려면 변경 요청 객체를 만들고 이를 공유 PHPhotoLibrary 객체에 명시적으로 알려줍니다. 이 아키텍처를 사용하면 다수의 스레드 혹은 다수의 애플리케이션과 동일한 에셋을 사용하여 쉽고 안전하며 효율적으로 작업할 수 있습니다.
변경을 관찰.

가져온 에셋 및 컬렉션에 대한 변경 핸들러를 등록하려면 공유 PHPhotoLibrary 객체를 사용하세요. 다른 애플리케이션이나 기기가 에셋의 콘텐츠나 메타 데이터 또는 컬렉션의 리스트를 변경할 때마다 애플리케이션에 알려줍니다. PHChange 객체는 변경 전후의 객체 상태에 대한 정보를 제공하여 쉽게 컬렉션뷰 또는 유사한 인터페이스를 업데이트할 수 있도록 합니다.
Photos 애플리케이션의 기능들을 지원.

PHCollectionList 클래스를 사용해 사진 애플리케이션의 특별한 순간 계층에 해당하는 에셋을 찾습니다. 버스트, 파노라마 사진 및 고프레임 비디오를 식별하려면 PHAsset 클래스를 사용하세요. iCloud 사진 라이브러리가 활성화되면 Photos 프레임워크의 에셋과 컬렉션에는 동일한 iCloud 계정의 사용할 수 있는 내용이 반영됩니다.
에셋과 미리보기 로딩 및 캐싱.

PHImageManager 클래스를 사용해 지정된 크기로 에셋의 이미지를 요청하거나 비디오 에셋에 사용할 AVFoundation 객체를 요청합니다.
에셋 콘텐츠 편집.

PHAsset 및 PHAssetChangeRequest 클래스는 편집을 위해 사진 또는 비디오를 요청하여 사진 라이브러리에 편집한 내용을 반영하는 메서드를 정의합니다.