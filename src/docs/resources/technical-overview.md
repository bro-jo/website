---
title: 기술 개요
---

## Flutter란?

Flutter는 고성능, 고품질의 iOS, 안드로이드 앱을 단일 코드 베이스로 개발할 수 있는 모바일 앱 SDK입니다.

스크롤 동작, 글씨, 아이콘과 같이 플랫폼 별로 달라지는 부분들을 아울러서 서로 다른 플랫폼에서도 자연스럽게 동작하는 고성능의 앱을 개발할 수 있게 하는 것이 Flutter의 목표입니다.

<object type="image/svg+xml" data="/images/whatisflutter/hero-shrine.svg" style="width: 100%; height: 100%;"></object>

지금 보시는 앱은 Flutter 설치 및 환경 구축 후 직접 돌려볼 수 있는 데모 앱입니다. 그 외 다른 Flutter 샘플 앱들은 [Gallery](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery/lib/demo)에서 확인할 수 있습니다.
Shrine은 고품질의 이미지 스크롤, 대화식 카드, 버튼, 드롭다운 리스트 그리고 쇼핑 카트 페이지를 갖고 있습니다. 이 앱의 단일 코드 베이스 혹은 더 많은 예제들을 보고 싶다면, [GitHub 저장소를 방문하세요](https://github.com/flutter/flutter/tree/master/examples).

Flutter 앱 개발을 시작하기 위해 모바일 개발 경험이 반드시 필요하지는 않습니다. 앱은 [다트](https://www.dartlang.org)로 작성되는데, 만약 자바나 자바스크립트와 같은 언어를 사용해본 경험이 있다면 익숙할 수 있습니다. 객체지향 언어에 대한 경험은 분명 도움이 되겠지만, 프로그래머가 아닌 사람들도 Flutter 앱을 만들었습니다!

## 왜 Flutter를 사용해야 할까요?

Flutter의 장점은 무엇일까요:
* 높은 생산성
* 단일 코드베이스로 iOS와 안드로이드 개발
* 모던하고 표현적인 언어 그리고 선언적 접근법을 통해 단일 OS에서 더 적은 코드로 더 많은 것을 할 수 있습니다.
* 쉬운 프로토타입과 반복적 개발
* 앱 실행 중에 코드를 바꾸고 리로드하여 개발을 할 수 있습니다. (hot reload)
* 앱이 중단된 지점에서 문제를 수정하고 디버깅을 이어나갈 수 있습니다.
* 아름답고, 고도로 커스터마이징된 UX를 만들 수 있습니다.
* Flutter의 자체 프레임워크를 사용하여 머티리얼 디자인과 쿠퍼티노 (iOS) 스타일의 풍부한 위젯들을 만들 수 있습니다.
* OEM 위젯의 제한없이 맞춤형의 아름다운 브랜드 주도 디자인을 실현할 수 있습니다.

## 핵심 원리

Flutter는 현대적인 react-style 프레임워크, 2D 렌더링 엔진, 바로 이용 가능한 위젯들, 그리고 개발 툴들을 포함합니다. 이러한 구성 요소들을 통해 앱을 디자인, 개발, 테스트 그리고 디버깅할 수 있습니다. 모든 것은 몇가지 핵심 원리들을 중심으로 구성됩니다.

### 모든 것은 위젯입니다

위젯은 Flutter 앱 UI의 기본 단위입니다. 각 위젯은 UI의 불변 선언입니다. 뷰, 뷰 컨트롤러, 레이아웃 그리고 기타 다른 속성들을 분리하는 다른 프레임워크들과 다르게, Flutter는 일관적이고 통일된 오브젝트 모델을 갖고 있는데, 그것이 바로 위젯입니다.

위젯은 다음의 것들을 정의할 수 있습니다:

* 구조적인 요소 (예: 버튼이나 메뉴)
* 스타일적인 요소 (예: 폰트나 색상)
* 레이아웃 요소 (예: 패딩)
* 기타 등등...

위젯은 구성을 기반으로 계층 구조를 형성합니다. 각 위젯은 내부에 중첩되고 부모의 속성들을 상속받습니다. 별도 "application" 오브젝트가 없는 대신 최상위 위젯이 그 역할을 하게 됩니다.

프레임워크에게 위젯을 계층 구조 상 다른 위젯으로 교체하게 함으로써, 사용자 상호작용과 같은 이벤트를 구현할 수 있습니다. 프레임워크는 새로운 위젯과 기존 위젯을 비교하고 효을적으로 UI를 업데이트하게 됩니다.

#### 구성 > 상속

위젯은 종종 강력한 효과를 내기 위해 단일 목적의 여러 작은 위젯들로 구성됩니다. 예를 들어, 일반적으로 사용되는 [Container](https://github.com/flutter/flutter/blob/master/packages/flutter/lib/src/widgets/container.dart) 위젯은 painting, positioning, sizing과 같은 레이아웃 관련 위젯들로 구성됩니다. 좀더 구체적으로 [LimitedBox](https://docs.flutter.io/flutter/widgets/LimitedBox-class.html),
[ConstrainedBox](https://docs.flutter.io/flutter/widgets/ConstrainedBox-class.html),
[Align](https://docs.flutter.io/flutter/widgets/Align-class.html),
[Padding](https://docs.flutter.io/flutter/widgets/Padding-class.html),
[DecoratedBox](https://docs.flutter.io/flutter/widgets/DecoratedBox-class.html),
그리고 [Transform](https://docs.flutter.io/flutter/widgets/Transform-class.html) 위젯들로 구성됩니다. 커스터마이징을 위해 Container의 서브 클래스를 만들기 보다는 앞서 언급한 위젯들 혹은 그외 다른 간단한 위젯들을 참신한 방법으로 조합할 수 있습니다.

가능한 조합의 수를 최대화하기 위해 클래스 계층 구조는 얕고 넓습니다.

<object type="image/svg+xml" data="/images/whatisflutter/diagram-widgetclass.svg" style="width: 100%; height: 100%;"></object>

다른 위젯들과 함께 구성하는 방식으로 위젯의 *레이아웃*을 조작할 수 있습니다. 예를 들어, 위젯을 가운데로 위치시키려면 그 위젯을 Center 위젯으로 감싸면 됩니다. 패딩, 정렬, 행, 열, 그리드와 같은 여러 레이아웃 위젯들이 있는데, 이러한 레이아웃 위젯들은 그 자체로 시각적 표현을 갖고 있지는 않습니다. 이들의 유일한 목적은 다른 위젯의 레이아웃을 제어하는 것이기 때문입니다. 만약 어떤 위젯이 어째서 이렇게 렌더링된건지 궁금하다면, 인근에 위치한 다른 위젯들을 살펴보면 알 수 있습니다.

#### 레이어 케이크는 맛있습니다

Flutter 프레임워크는 겹겹이 쌓인 일련의 레이어 층으로 구성됩니다.

<object type="image/svg+xml" data="/images/whatisflutter/diagram-layercake.svg" style="width: 85%; height: 85%"></object>

프레임워크의 상위 레이어로 갈수록 하위 레이어보다 더 빈번히 사용됩니다. Flutter의 계층화된 프레임워크를 구성하는 전체 라이브러리 셋은 [API 문서](https://docs.flutter.io)에서 확인할 수 있습니다.

이러한 설계의 목표는 개발자로 하여금 더 적은 코드로 더 많은 일을 할 수 있게 하는 것입니다. 예를 들어, 머티리얼 계층은 위젯 계층의 기본적인 위젯들을 조합하여 만들어집니다. 그리고 위젯 계층은 랜더링 계층의 하위 레벨 오브젝트들의 조합으로 만들어집니다.

계층들은 앱을 만드는데 많은 옵션들을 제공합니다. 프레임워크의 풍부한 표현력을 활용할 수 있는 커스터마이즈한 접근 법을 선택하거나 위젯 계층의 블럭들을 사용하세요, 혹은 이것들을 조화롭게 잘 조합하여 사용하세요. Flutter가 제공하는 자체 위젯들을 사용하거나, Flutter 팀에서 프레임워크 개발할 떄 사용한 것과 동일한 툴과 기술들을 갖고 직접 커스텀 위젯도 만들 수 있습니다.


모든 것은 공개되어 있습니다. 프레임워크의 하위 계층을 뜯어보지 않고도 상위 레벨의 위젯 컨셉만으로도 생산성을 높일 수 있습니다.

### 위젯 만들기

위젯의 트리 (혹은 계층 구조)를 반환하는 [build](https://docs.flutter.io/flutter/widgets/StatelessWidget/build.html) 함수를 구현하여 위젯의 고유한 특성을 정의할 수 있습니다. 반환된 트리 (계층 구조)는 위젯의 UI 부분을 좀더 구체적으로 나타냅니다. 예를 들어, 툴바 위젯의 build 함수는 약간의 [text](https://docs.flutter.io/flutter/widgets/Text-class.html)와 [다양한](https://docs.flutter.io/flutter/material/IconButton-class.html)
[버튼들](https://docs.flutter.io/flutter/material/PopupMenuButton-class.html)로 구성된 [horizontal layout](https://docs.flutter.io/flutter/widgets/Row-class.html)를 반환할 것입니다. 프레임워크는 각 위젯들이 [완전히 구체적인 위젯](https://docs.flutter.io/flutter/widgets/RenderObjectWidget-class.html)으로 처리될 때까지 재귀적으로 처리할 것입니다. 그리고 그 결과 위젯은 트리 (계층 구조)로 결합됩니다.

위젯의 build 함수는 부수 효과가 없어야 합니다. 매번 위젯이 새로 빌드될 때마다, 이전에 반환한 것과 관계없이 항상 새로운 트리를 반환해야 합니다. 프레임워크는 현재 빌드와 이전 빌드를 계속 비교하여 UI를 그리기 위해 어떤 수정이 필요한지 결정합니다.

이러한 자동화 비교는 아주 효율적이어서, 고성능의 상호작용 앱을 만들 수 있게 해줍니다. 그리고 이러한 빌드 함수의 설계는 UI의 상태를 변경하는 복잡성보다, 위젯이 무엇으로 구성되었는지 선언하는 것에 초점을 맞춤으로써 코드를 단순화 시켜줍니다.

### UI 다루기

만약 위젯 고유의 특성이 사용자 상호작용이나 기타 다른 요소에 의해 변경될 필요가 있다면 위젯은 *stateful*이라고 말할 수 있습니다. 예를 들어, 만약 사용자가 버튼을 누를 때마다 1씩 증가하는 카운터 위젯이 있다고 하면, 그 카운터의 값은 위젯의 상태입니다. 값이 변할 때마다, 위젯은 UI를 업데이트하기 위해 다시 빌드되어야 합니다.

이러한 위젯은 [StatefulWidget](https://docs.flutter.io/flutter/widgets/StatefulWidget-class.html)
([StatelessWidget](https://docs.flutter.io/flutter/widgets/StatelessWidget-class.html)과는 다른)의 서브 클래스이며 변경 가능한 상태를 서브 클래스의 [State](https://docs.flutter.io/flutter/widgets/State-class.html)에 저장합니다.

<object type="image/svg+xml" data="/images/whatisflutter/diagram-state.svg" style="width: 85%; height: 85%"></object>

State 오브젝트의 값이 변경될 때마다 (예를 들어 카운터 증가), [setState]()를 호출해야 합니다. 그렇게 해야 프레임워크가 State의 빌드 메소드를 다시 호출하여 UI를 업데이트하게 됩니다. 상태 관리 예제는 [MyApp template](https://github.com/flutter/flutter/blob/master/packages/flutter_tools/templates/app/lib/main.dart.tmpl)에서 확인할 수 있습니다.

상태와 위젯 객체를 분리시키면 다른 위젯들이 상태 손실에 대한 걱정없이 stateless 위젯과 stateful 위젯을 동일하게 처리하게 할 수 있습니다. 상태를 유지하기 위해 자식을 붙잡고 있을 필요 없이, 자식의 상태를 잃지 않고도 자유롭게 새로운 자식 인스턴스를 만들 수 있습니다. 프레임워크는 적절한 경우 기존 상태 객체를 찾고 재사용하는 모든 작업을 수행합니다.

## 해보세요!

이제 Flutter 프레임워크의 기본 구조와 원리, 앱이 어떻게 만들어지고 상호작용하는지 이해했으므로, 개발할 준비가 되었습니다.

다음 과정:

1. [Flutter 시작하기 가이드 따라하기](/docs/get-started).
1. [레이아웃 만들기 튜토리얼](/docs/development/ui/layout/tutorial)과 [Flutter 앱에 상호작용 추가하기](/docs/development/ui/interactive)를 해보세요.
1. [위젯 프레임워크 둘러보기](/docs/development/ui/widgets-intro)에 있는 상세한 예제들을 따라해보세요.

## 도움 받기

Flutter 프로젝트에 관심을 갖고 다양한 방식으로 커뮤니티에 참여하세요. Flutter는 오픈 소스이며 당신의 피드백을 환영합니다.

- [특정 솔루션들로 답변될 수 있는 HOWTO 질문들은 여기에 물어봐주세요][so]
- [Flutter 엔지니어들과 사용자들 간의 실시간 채팅][gitter]
- [Flutter 관련 논의, best practices, 앱 디자인 그 외 기타 등등을 위한 메일링 리스트][mailinglist]
- [버그 리포팅, 기능과 문서에 대한 요구 사항][issues]
- [Twitter 구독: @flutterio](https://twitter.com/flutterio/)


[issues]: https://github.com/flutter/flutter/issues
[apidocs]: https://docs.flutter.io
[so]: https://stackoverflow.com/tags/flutter
[mailinglist]: https://groups.google.com/d/forum/flutter-dev
[gitter]: https://gitter.im/flutter/flutter
