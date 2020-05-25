---
layout: post
title:  "Modal(editing)"
date:   2020-05-25 20:50:00 +0900
categories: iOS
tags: boostCourse
---

## 개요
> * 
> * 

---
## Modal
Modal은 사용자의 이목을 끌기 위해 사용하는 화면전환 기법이다. 사실, 화면 전환보다는 이목을 집중해야 하는 다른 화면 위로 뛰어(presenting) 표현하는 방식이다. 모달로 보이는 화면을 사라지게 하려면 반드시 특정 선택을 해야한다는 특징이 있다. 예를 들어 얼럿을 통해 확인/취소 중 하나를 해야한다거나 액션시트에서 무엇인가 선택을 해야한다.
그래서 모달은 네비게이션 인터페이스와는 달리 정보의 흐름을 가지고 화면을 이동한다기 보다는 꼭 이목을 끌어야하는 화면에서 사용한다. 네비게이션 인터페이스를 통해 화면을 표현하는 것과는 용도가 완전히 다르고 볼 수 있다. 그래서 모달로 보이는 화면은 되도록 단순하고 사용자가 빠르게 처리할 수 있는 내용을 표현하는 것이 좋다.

## Presenting a View Controller
뷰 컨트롤러를 화면상에 나타내는 방법은 두 가지다.
1. 컨테이너뷰 컨테롤러에 임베드
2. 프리젠테이션을 통해서 나타내기
뷰 컨트롤러의 present 지원 기능은 UIViewController 클래스에 내장되어 있으며 모든 뷰 컨트롤러 객체에서 사용할 수 있다. 뷰 컨트롤러를 나타내면 원래 뷰 컨트롤러(presenting)와 새롭게 나타나는 뷰 컨트롤러(presented) 간의 관계가 생성된다. 이 관계는 뷰 컨트롤러 계층의 일부를 형성하며, 나타내는 뷰 컨트롤러가 사라질(dismissed) 때 까지 유지된다.

## The Presentation and Transition Process
뷰 컨트롤러의 프레젠테이션은 새로운 컨텐츠를 화면에 애니메이션으로 표시할 수 있는 쉽고 빠른 방법이다. UIKit에 내장된 프레젠테이션 기능은 내장 혹은 커스텀 애니메이션을 사용하여 새로운 뷰 컨트롤러를 표시할 수 있도록 한다. 내장 프레젠테이션과 애니메이션은 UIKit가 모든 작업을 처리하기 때문에 아주 적은 코드로 가능하다. 뷰 컨트롤러 프레젠테이션은 **프로그래밍 방식 또는 segue**를 사용하여 구현할 수 있다.

## Presentation Style
뷰 컨트롤러의 프레젠테이션 스타일에 따라 뷰 컨트롤러가 화면에 나타나는 모양이 달라진다. UIKit는 특정한 모양과 의도를 가진 많은 프레젠테이션 스타일을 정의한다. 물론 커스텀 스타일 정의도 가능하다. 애플리케이션을 디자인할 때 의도에 가장 잘 반영할 수 있는 프레젠테이션 스타일을 선택하고, 이를 나타내는 뷰 컨트롤러의 modalPresentationStyle 프로퍼티에 적절한 상수를 할당하면 된다.

### Full-Screen Presentation Style
- 화면 전체를 덮으며, 아래의(underlying) 기본 콘텐츠와 상호작용을 방지한다. 가로 모드인 환경에서는 전체화면 스타일 중 단 하나만이 기본 콘텐츠를 완전히 커버할 수 있다. 나머지 스타일은 뷰를 흐리게 하거나 투명도를 낮추어 기본 뷰 컨트롤러의 일부분을 보여줄 수 있다.
- 다음 사진이 가로 모드 환경에서의 프레젠테이션 스타일이다.

<img src="/assets/img/fullScreen.png" alt="full-screen presetation style" />

- Tip : 일반적으로, UIKit는 UIModalPresentationFullScreen 스타일을 사용하여 뷰 컨트롤러를 표시할 때, 전환 애니메이션이 끝난 후에 아래에 위치한 뷰 컨트롤러의 뷰는 화면에서 보이지 않는다. 그 대신에, UIModalPresentationoverFullScreen 스타일을 지정하여 아래의 뷰가 완전히 보이지 않는 것을 막을 수 있다. 

### The Popover Style
- UIModalPresentationPopover 스타일은 뷰 컨트롤러를 팝오버뷰로 나타낸다. 팝오버는 추가 정보, 포커스, 선택된 객체와 관련된 항목 목록을 표시하는데 유용하다. 프렌젠테이션 스타일은 기본적으로 UIModalPresentationOverFullScreen이다. 팝업 뷰 외부에 탭을 하게 되면 자동으로 팝업을 닫는다(dismiss).
- 다음은 가로 모드 환경에서 팝오버 스타일을 보여준다.

<img src="/assets/img/popover.png" alt="popover presetation style" />

- 팝오버 스타일은 iPad에서만 지원한다.

### The Current Context Styles
- UIModalPresentationCurrentContext 스타일은 아래 뷰 컨트롤러의 콘텐츠 영역에 콘텐츠를 올리는 형태이다.
- 프레젠테이션 컨텍스트를 정의하는 뷰 컨트롤러는 프레젠테이션 중에 사용할 transition animations을 정의할 수도 있다. 일반적으로 UIKit는 나타난(presented) 뷰 컨트롤러의 modalTransitionStyle 프로퍼티 값을 사용하여 뷰 컨트롤러를 애니메이션으로 화면 상에 나타낸다. 프레젠테이션 컨텍스트 뷰 컨트롤러(즉, 가려지는 뷰 컨트롤러)의 providesPresentationContextTransitionStyle이 true로 설정된 경우, UIKit는 나타나는 뷰 컨트롤러의 modalTransitionStyle 프로퍼티 값 대신에, 가려지는 뷰 컨트롤러의 modalTransitionStyle 프로퍼티 값을 사용한다.
- 다음은 가로 모드 환경에서 C 뷰 컨트롤러의 뷰가 A 뷰 컨트롤러 뷰의 콘텐츠 영역을 덮는 컨텍스트 스타일을 보여준다.

<img src="/assets/img/currentContext.png" alt="currentContext presetation style" />

### Custom Presentation Styles
- UIModalPresentationCustom 스타일을 사용하면 정의된 커스텀 스타일을 사용해 뷰 컨트롤러를 표시할 수 있다. 커스텀 스타일을 생성하는 것은 UIPresentationController를 상속받아 그 메서드를 사용하여 커스텀 뷰를 화면 상에 애니메이션으로 나타내고 표시된 뷰 컨트롤러의 크기와 위치를 설정하는 것이다. 커스텀 프레젠테이션 컨트롤러를 정의하는 것에 대한 자세한 정보는 [커스텀 프레젠테이션 생성하기(Creating Custom Presentations)](https://developer.apple.com/library/content/featuredarticles/ViewControllerPGforiPhoneOS/DefiningCustomPresentations.html#//apple_ref/doc/uid/TP40007457-CH25-SW1) 를 참고한다.

## Transition Styles
- 뷰 컨트롤러를 표시하는데 사용되는 **애니메이션 유형**을 결정한다. 기본으로 제공하는 전환 스타일의 경우 표시할 뷰 컨트롤러의 modalTransitionStyle 프로퍼티에서 지정할 수 있다. 뷰 컨트롤러를 표시할 때, UIKit는 해당 스타일에 맞는 애니메이션을 생성한다.
- UIModalTransitionStyleCoverVertical이 뷰 컨트롤러를 화면 상에서 어떻게 애니메이션으로 나타내는지를 알 수 있다. 뷰 컨트롤러 B는 화면 밖에서 시작해서 애니메이션을 통해 뷰 컨트롤러 A의 위쪽 상단까지 커버한다. 뷰 컨트롤러 B가 사라지면(dismissed) 애니메이션 또한 반전되어 B가 아래로 슬라이드되며 A가 드러나게 된다. 애니메이터 객체와 전환 델리게이트를 사용하여 커스텀 전환 과정을 생성할 수 있다. 커스텀 전환을 구현하는 방법에 대한 자세한 내용은 [전환 애니메이션 커스터마이징(Customizing the Transition Animations)](https://developer.apple.com/library/content/featuredarticles/ViewControllerPGforiPhoneOS/CustomizingtheTransitionAnimations.html#//apple_ref/doc/uid/TP40007457-CH16-SW1) 를 참고한다.
- 다음은 표준 슬라이드 앱 전환(standard slide-up transition)이다.

<img src="/assets/img/slideUp.png" alt="slideUp presetation style" />

## Presenting vs Showing a View Controller
UIViewController 클래스는 뷰 컨트롤러를 표시하는 두 가지 방법을 제공한다.

- `showViewController:send:`와 `showDetailViewController:sender:` 메서드는 뷰 컨트롤러를 표시하는 데에 가장 적응성이 우수하고 유연한 방법을 제공한다. 이러한 메서드를 사용하면 나타내는 뷰 컨트롤러가 프레젠테이션을 가장 잘 처리하는 방법을 결정할 수 있다. 예를 들어, 컨테이너뷰 컨트롤러는 뷰 컨트롤러를 모달 방식으로 표시하는 대신, 이를 서브뷰로 통합할 수 있다. 기본 동작은 뷰 컨트롤러를 모달 방식으로 표시한다.
- `presentViewController:animated:completion:` 메서드는 뷰 컨트롤러를 항상 모달 방식으로 표시한다. 이 메서드를 호출하는 뷰 컨트롤러는 궁극적으로 프레젠테이션을 처리하지 못할 수도 있으나, 프레젠테이션은 항상 모달 방식을 채택하고 있다.













(여기까지 함)
### 뷰 컨트롤러 표시하기(Presenting a View Controller)

-   뷰 컨트롤러의 프레젠테이션을 시작하는 방법은 여러 가지 입니다.
    -   뷰 컨트롤러를 자동으로 표시하려면 세그(segue)를 사용하세요. 세그는 인터페이스 빌더에서 지정한 정보를 사용하여 뷰 컨트롤러를 인스턴스화 하여 표시합니다. 세그를 구성하는 방법에 관한 자세한 내용은 [세그 사용하기(Using Segues)](https://developer.apple.com/library/content/featuredarticles/ViewControllerPGforiPhoneOS/UsingSegues.html#//apple_ref/doc/uid/TP40007457-CH15-SW1) 를 참고하세요.
    -   `showViewController:sender:` 또는 `showDetailViewController:sender:`메서드를 사용하여 뷰 컨트롤러를 나타낼 수 있습니다. 커스텀뷰 컨트롤러의 경우, 이러한 메서드의 동작을 여러분의 뷰 컨트롤러에 더욱 적합하도록 변경할 수 있습니다.
    -   `presentViewController:animated:completion:`메서드를 호출하여 뷰 컨트롤러를 모달로 나타낼 수 있습니다.

  

**뷰 컨트롤러 보여주기(Showing View Controllers)**

`show(_:sender:)` 와  showDetailViewController(_:sender:) 메서드를 사용할 때, 새로운 뷰 컨트롤러를 화면에 띄우는 과정은 간단합니다.

1.  나타나는 뷰 컨트롤러 객체를 만듭니다. 뷰 컨트롤러를 생성할 때, 작업을 수행하는데 필요한 모든 데이터의 초기화는 여러분의 책임입니다.
2.  새로운 뷰 컨트롤러의 `modalPresentationStyle` 프로퍼티를 선호하는 프레젠테이션 스타일로 설정합니다.
3.  뷰 컨트롤러의 `modalTransitionStyle` 프로퍼티를 원하는 전환 애니메이션 스타일로 설정합니다.
4.  현재 뷰 컨트롤러의 `showViewController:sender:` 와 `showDetailViewController:sender:` 메서드를 호출하세요.

`UIKit`은 `showViewController:sender:` 와 `showDetailViewController:sender:` 메서드에 대한 호출을 나타내는 뷰 컨트롤러(presenting view controller)에 전달합니다. 그런 다음, 해당 뷰 컨트롤러는 프레젠테이션을 가장 효과적으로 수행할 방법을 결정하고, 필요할 경우 프레젠테이션 및 전환 스타일을 변경할 수 있습니다. 예를 들면, 내비게이션 컨트롤러가 뷰 컨트롤러를 내비게이션 스택(navigation stack)에 푸시(push)할 수 있습니다.

**뷰 컨트롤러를 모달로 표시하기(Presenting View Controllers Modally)**

뷰 컨트롤러를 직접 나타내는 경우, `UIKit`에 새 뷰 컨트롤러를 표시하는 방법과 화면상에 애니메이션을 적용하는 방법을 알려 줍니다.

1.  나타나는 뷰 컨트롤러 객체를 만듭니다. 뷰 컨트롤러를 생성할 때, 작업을 수행하는데 필요한 모든 데이터의 초기화는 여러분의 책임입니다.
2.  새로운 뷰 컨트롤러의 `modalPresentationStyle` 프로퍼티를 선호하는 스타일로 설정합니다.
3.  뷰 컨트롤러의 `modalTransitionStyle` 프로퍼티를 원하는 전환 애니메이션 스타일로 설정합니다.
4.  현재 뷰 컨트롤러의 `presentViewController:animated:completion:` 메서드를 호출합니다.

present(_:animated:completion:) 메서드를 호출하는 뷰 컨트롤러는 모달 프레젠테이션 (modal presentation)을 실제로 수행하는 뷰 컨트롤러가 아닐 수도 있습니다. 프레젠테이션 스타일은 나타내는 뷰 컨트롤러에 필요한 특성을 포함하여, 뷰 컨트롤러가 나타나는 방식을 결정합니다. 예를 들어, 전체화면 프레젠테이션은 전체화면 뷰 컨트롤러에서 시작해야 합니다. 현재 표시된 뷰 컨트롤러가 적합하지 않은 경우에 `UIKit`은 적합한 뷰 컨트롤러를 찾을 때까지 뷰 컨트롤러 계층을 탐색합니다. 모달 프레젠테이션이 완료되면 `UIKit`은 이에 영향을 받은 뷰 컨트롤러의 `presentingViewController` 및 `presentViewController` 프로퍼티를 업데이트합니다.

  

**팝오버에 뷰 컨트롤러 나타내기(Presenting a View Controller in a Popover)**

팝오버를 나타내려면 추가적인 구성이 필요합니다. 모달 프레젠테이션 스타일을 `UIModalPresentationPopover`로 설정한 후, 다음과 같은 팝오버 관련 속성을 구성할 수 있습니다.

-   뷰 컨트롤러의 `preferredContentSize` 프로퍼티를 이용해 원하는 크기로 설정할 수 있습니다.
-   뷰 컨트롤러의 `popoverPresentationController` 프로퍼티에서 접근할 수 있는 연관된 `UIPopoverPresentationController` 객체를 사용하여 팝오버 고정 점(popover anchor point)을 설정할 수 있습니다.

`UIPopoverPresentationController` 객체를 사용하여 필요에 따라 팝오버의 모양을 다른 방식으로 조정할 수 있습니다. 팝오버 프레젠테이션 컨트롤러는 프레젠테이션 프로세스 중 발생하는 변경에 응답하는 데에 사용할 수 있는 델리게이트 객체도 지원합니다. 예를 들어, 팝오버가 나타나거나, 사라지거나, 화면 상에서 위치가 변경될 때, 델리게이를 사용하여 이에 응답할 수 있습니다. 이 객체에 대한 보다 자세한 내용은 [UIPopoverPresentationController 클래스 참조 (UIPopoverPresentationController Class Reference)](https://developer.apple.com/documentation/uikit/uipopoverpresentationcontroller)를 찾아보세요.

  

**나타난 뷰 컨트롤러 닫기(Dismissing a Presented View Controller)**

나타난 뷰 컨트롤러를 닫으려면 프레젠테이션 뷰 컨트롤러의  dismiss(animated:completion:) 메서드를 호출하십시오. 또한, 제공된 뷰 컨트롤러 자체에서 이 메서드를 호출할 수도 있습니다. 표시된 뷰 컨트롤러에서 이 메서드를 호출하면 `UIKit`은 자동으로 이 요청을 나타내는 뷰 컨트롤러(presenting view controller)로 전달합니다. 뷰 컨트롤러를 닫기 전에 항상 중요한 정보를 저장하세요. 뷰 컨트롤러를 닫으면 뷰 컨트롤러는 뷰 컨트롤러 계층에서 제거되며, 화면에서도 해당하는 뷰가 제거됩니다. 별도로 저장된 강한 참조가 없는 경우, 뷰 컨트롤러를 닫으면 이와 연결된 뷰 컨트롤러가 메모리에서 해제(releases)됩니다.

  

**다른 스토리보드에서 정의된 뷰 컨트롤러 나타내기(Presenting a View Controller Defined in a Different Storyboard)**

하나의 스토리보드에 있는 뷰 컨트롤러 사이에는 세그를 생성할 수는 있으나, 스토리보드 간의 세그를 생성할 수 없습니다. 다른 스토리보드에 저장된 뷰 컨트롤러를 나타내고자 할 경우 다음과 같이 뷰 컨트롤러를 명시적으로 표시하기 전에 먼저 인스턴스화 해야합니다. 예시에서는 뷰 컨트롤러를 모달로 나타내(presents)지만, 뷰 컨트롤러를 내비게이션 컨트롤러에 푸시하거나 다른 방법으로도 나타낼(display) 수 있습니다.