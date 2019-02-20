---
title: 기술 개요
---

## Flutter란?

Flutter는 고성능, 고품질의 iOS, 안드로이드 앱을 단일 코드 베이스로 개발할 수 있는 모바일 앱 SDK입니다.

스크롤 동작, 글씨, 아이콘과 같이 플랫폼 별로 달라지는 부분들을 아울러서 서로 다른 플랫폼에서도 자연스럽게 동작하는 고성능의 앱을 개발할 수 있게 하는 것이 Flutter의 목표입니다.

<object type="image/svg+xml" data="/images/whatisflutter/hero-shrine.svg" style="width: 100%; height: 100%;"></object>

지금 보시는 앱은 Flutter 설치 및 환경 구축 후 직접 돌려볼 수 있는 데모 앱입니다. 
그 외 다른 Flutter 샘플 앱들은 [Gallery]({{site.github}}/flutter/flutter/tree/master/examples/flutter_gallery/lib/demo)에서 확인할 수 있습니다.
Shrine은 고품질의 이미지 스크롤, 대화식 카드, 버튼, 드롭다운 리스트 그리고 쇼핑 카트 페이지를 갖고 있습니다. 
이 앱의 단일 코드 베이스 혹은 더 많은 예제들을 보고 싶다면, [GitHub 저장소를 방문하세요]({{site.github}}/flutter/flutter/tree/master/examples).

Flutter 앱 개발을 시작하기 위해 모바일 개발 경험이 반드시 필요하지는 않습니다. 앱은 [다트]({{site.dart-site}})로 작성되는데, 만약 자바나 자바스크립트와 같은 언어를 사용해본 경험이 있다면 익숙할 수 있습니다. 객체지향 언어에 대한 경험은 분명 도움이 되겠지만, 프로그래머가 아닌 사람들도 Flutter 앱을 만들었습니다!

## 왜 Flutter를 사용해야 할까요?

Flutter의 장점은 무엇일까요:

*   높은 생산성
    *   단일 코드베이스로 iOS와 안드로이드 개발
    *   모던하고 표현적인 언어 그리고 선언적 접근법을 통해 단일 OS에서 더 적은 코드로 더 많은 것을 할 수 있습니다.
    *   쉬운 프로토타입과 반복적 개발
        *   앱 실행 중에 코드를 바꾸고 리로드하여 개발을 할 수 있습니다. (hot reload)
        *   앱이 중단된 지점에서 문제를 수정하고 디버깅을 이어나갈 수 있습니다.
*   아름답고, 고도로 커스터마이징된 UX를 만들 수 있습니다.
    *   Flutter의 자체 프레임워크를 사용하여 머티리얼 디자인과 쿠퍼티노 (iOS) 스타일의 풍부한 위젯들을 만들 수 있습니다.
    *   OEM 위젯의 제한없이 맞춤형의 아름다운 브랜드 주도 디자인을 실현할 수 있습니다.

## Core principles

Flutter includes a modern react-style framework, a 2D rendering engine,
ready-made widgets, and development tools. These components work together to help
you design, build, test, and debug apps. Everything is organized around a few core
principles.

### Everything's a Widget

Widgets are the basic building blocks of a Flutter app's user interface. Each widget is an
immutable declaration of part of the user interface.  Unlike other frameworks that
separate views, view controllers, layouts, and other properties, Flutter has a
consistent, unified object model: the widget.

A widget can define:

*   a structural element (like a button or menu)
*   a stylistic element (like a font or color scheme)
*   an aspect of layout (like padding)
*   and so on...

Widgets form a hierarchy based on composition.  Each widget nests inside, and
inherits properties from, its parent.  There is no separate "application" object.
Instead, the root widget serves this role.

You can respond to events, like user interaction, by telling the framework to
replace a widget in the hierarchy with another widget.  The framework then
compares the new and old widgets and efficiently updates the user interface.

#### Composition > inheritance

Widgets are themselves often composed of many small, single-purpose widgets that
combine to produce powerful effects.  For example,
[Container]({{site.github}}/flutter/flutter/blob/master/packages/flutter/lib/src/widgets/container.dart),
a commonly-used widget, is made up of several widgets responsible for layout,
painting, positioning, and sizing. Specifically, Container is made up of
[LimitedBox]({{site.api}}/flutter/widgets/LimitedBox-class.html),
[ConstrainedBox]({{site.api}}/flutter/widgets/ConstrainedBox-class.html),
[Align]({{site.api}}/flutter/widgets/Align-class.html),
[Padding]({{site.api}}/flutter/widgets/Padding-class.html),
[DecoratedBox]({{site.api}}/flutter/widgets/DecoratedBox-class.html),
and [Transform]({{site.api}}/flutter/widgets/Transform-class.html)
widgets.  Rather than subclassing Container to produce a customized effect, you
can compose these, and other, simple widgets in novel ways.

The class hierarchy is shallow and broad to maximize the possible number of
combinations.

<object type="image/svg+xml" data="/images/whatisflutter/diagram-widgetclass.svg" style="width: 100%; height: 100%;"></object>

You can also control the *layout* of a widget by composing it with other widgets.
For example, to center a widget, you wrap it in a Center widget. There are
widgets for padding, alignment, row, columns, and grids. These layout widgets
do not have a visual representation of their own. Instead, their sole purpose is to
control some aspect of another widget's layout. To understand why a widget
renders in a certain way, it's often helpful to inspect the neighboring widgets.

#### Layer cakes are delicious

The Flutter framework is organized into a series of layers, with each layer
building upon the previous layer.

<object type="image/svg+xml" data="/images/whatisflutter/diagram-layercake.svg" style="width: 85%; height: 85%"></object>

The upper layers of the framework are used more frequently than the lower
layers. For the complete set of libraries that make up
the Flutter's layered framework, see our
[API documentation]({{site.api}}).

The goal of this design is to help you do more with less code.  For example,
the Material layer is built by composing basic widgets from the widgets layer,
and the widgets layer itself is built by orchestrating lower-level objects from
the rendering layer.

The layers offer many options for building apps. Choose a customized approach to
unlock the full expressive power of the framework, or use building blocks from
the widgets layer, or mix and match. You can compose the ready-made widgets
Flutter provides, or create your own custom widgets using the same tools and
techniques that the Flutter team used to build the framework.

Nothing is hidden from you.  You reap the productivity benefits of a high-level,
unified widget concept, without sacrificing the ability to dive as deeply as you
wish into the lower layers.

### Building widgets

You define the unique characteristics of a widget by implementing a
[build]({{site.api}}/flutter/widgets/StatelessWidget/build.html)
function that returns a tree (or hierarchy) of widgets. This tree represents
the widget's part of the user interface in more concrete terms.
For example, a toolbar widget might have a build function that returns
a [horizontal layout]({{site.api}}/flutter/widgets/Row-class.html)
of some [text]({{site.api}}/flutter/widgets/Text-class.html) and
[various]({{site.api}}/flutter/material/IconButton-class.html)
[buttons]({{site.api}}/flutter/material/PopupMenuButton-class.html).
The framework then recursively asks each of these widgets to build until the
process bottoms out in [fully concrete
widgets]({{site.api}}/flutter/widgets/RenderObjectWidget-class.html),
which the framework then stitches together into a tree.

A widget's build function should be free of side effects.  Whenever it is asked
to build, the widget should return a new tree of widgets regardless of what the
widget previously returned. The framework does the heavily lifting of comparing
the previous build with the current build and determining what modifications
need to be made to the user interface.

This automated comparison is quite effective, enabling high-performance,
interactive apps. And the design of the build function simplifies your code by
focusing on declaring what a widget is made of, rather than the complexities of
updating the user interface from one state to another.

### Handling user interaction

If the unique characteristics of a widget need to change based on user
interaction or other factors, that widget is *stateful*. For example, if a
widget has a counter that increments whenever the user taps a button, the value
of the counter is the state for that widget. When that value changes, the widget
needs to be rebuilt to update the UI.

These widgets subclass
[StatefulWidget]({{site.api}}/flutter/widgets/StatefulWidget-class.html)
(rather than
[StatelessWidget]({{site.api}}/flutter/widgets/StatelessWidget-class.html))
and store their mutable state in a subclass of
[State]({{site.api}}/flutter/widgets/State-class.html).

<object type="image/svg+xml" data="/images/whatisflutter/diagram-state.svg" style="width: 85%; height: 85%"></object>

Whenever you mutate a State object (e.g., increment the counter), you must call
[setState]({{site.api}}/flutter/widgets/State/setState.html)() to
signal the framework to update the user interface by calling the State's build
method again. For an example of managing state, see the [MyApp
template]({{site.github}}/flutter/flutter/blob/master/packages/flutter_tools/templates/app/lib/main.dart.tmpl)
that's created with each new Flutter project.

Having separate state and widget objects lets other widgets treat stateless and
stateful widgets in the same way, without being concerned about losing state.
Rather than needing to hold on to a child to preserve its state, the parent is
free to create a new instance of the child without losing the child's persistent
state. The framework does all the work of finding and reusing existing state
objects when appropriate.

## Try it!

Now that you're familiar with the basic structure and principles of the Flutter
framework, along with how to build apps and make them interactive, you're ready
to start developing and iterating.

Next steps:

1.  [Follow the Flutter Getting Started guide](/docs/get-started).
1.  Try [Building Layouts tutorial](/docs/development/ui/layout/tutorial) and
    [Adding Interactivity to Your Flutter App](/docs/development/ui/interactive).
1.  Follow a detailed example in [Tour of the Widget
    Framework](/docs/development/ui/widgets-intro).

## Get support

Track the Flutter project and join the conversation in a variety of ways.
We're open source and would love to hear from you.

- [Ask HOWTO questions that can be answered with specific solutions][so]
- [Live chat with Flutter engineers and users][gitter]
- [Discuss Flutter, best practices, app design, and more on our mailing list][mailinglist]
- [Report bugs, request features and docs][issues]
- [Follow us on Twitter: @flutterio](https://twitter.com/flutterio/)


[issues]: {{site.github}}/flutter/flutter/issues
[apidocs]: {{site.api}}
[so]: {{site.so}}/tags/flutter
[mailinglist]: {{site.groups}}/d/forum/flutter-dev
[gitter]: https://gitter.im/flutter/flutter
