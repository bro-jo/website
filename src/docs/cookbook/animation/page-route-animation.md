---
title: 페이지 route 전환 애니메이션
next:
  title: 물리 시뮬레이션을 사용한 위젯 애니메이션
  path: /docs/cookbook/animation/physics-simulation
---

머티리얼과 같은 디자인 언어는 routes (또는 스크린) 간 전환을 할 때  
일반적인 행동을 정의합니다. 그럼에도 가끔은 화면 전환을 커스터마이징해서 더 유니크한 
앱을 만들 수 있습니다. 이를 돕기 위해  
[PageRouteBuilder]({{site.api}}/flutter/widgets/PageRouteBuilder-class.html)가
[Animation]({{site.api}}/flutter/animation/Animation-class.html) 객체를
제공합니다. 이 `Animation`은 
[Tween]({{site.api}}/flutter/animation/Tween-class.html),
[Curve]({{site.api}}/flutter/animation/Curve-class.html) 객체와 함께 쓰일 수 있고,
이를 통해 전환 애니메이션을 커스터마이징 할 수 있습니다.
이 문서는 화면 아래부터 새로운 route로 진입하는 애니메이션을 사용하여 
routes 간 전환을 하는 방법을 보여줍니다.

커스텀 화면 전환을 만들기 위해, 이 문서는 아래와 같은 단계를 따릅니다:

1. PageRouteBuilder 설정
2. `Tween` 생성
3. `AnimatedWidget` 추가 
4. `CurveTween` 사용
5. 두 개의 `Tween` 조합

## 1. PageRouteBuilder 설정

먼저, 
[PageRouteBuilder]({{site.api}}/flutter/widgets/PageRouteBuilder-class.html)를 사용하여
[Route]({{site.api}}/flutter/widgets/Route-class.html)를 생성하세요.
`PageRouteBuilder`에는 2개의 콜백이 있는데, 하나는 route에 콘텐츠를 빌드하기 위해 사용하고(`pageBuilder`), 
다른 하나는 route의 전환을 빌드하기 위해 사용합니다(`transitionsBuilder`).

{{site.alert.note}}
  pageBuilder에서 만들어진 위젯이 transitionsBuilder의 `child` 매개 변수로 전달됩니다. 
  `pageBuilder` 함수는 route가 처음 빌드 될 때만 호출됩니다. 
  전환 과정에서 `child`가 동일하게 유지될 때는 
  프레임워크가 추가 작업을 하지 않을 수 있습니다.
{{site.alert.end}}

다음 예제는 두 개의 route를 만듭니다. 
"Go!" 버튼이 있는 홈 route와 제목이 "Page 2"인 두 번째 route입니다.

```dart
import 'package:flutter/material.dart';

main() {
  runApp(MaterialApp(
    home: Page1(),
  ));
}

class Page1 extends StatelessWidget {
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: RaisedButton(
          child: Text('Go!'),
          onPressed: () {
            Navigator.of(context).push(_createRoute());
          },
        ),
      ),
    );
  }
}

Route _createRoute() {
  return PageRouteBuilder(
    pageBuilder: (context, animation, secondaryAnimation) => Page2(),
    transitionsBuilder: (context, animation, secondaryAnimation, child) {
      return child;
    },
  );
}

class Page2 extends StatelessWidget {
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: Text('Page 2'),
      ),
    );
  }
}
```

## 2. Tween 생성

새 페이지를 아래에서 올라오는 애니메이션으로 만들고 싶다면, 
`Offset(0, 1)`에서 `Offset(0, 0)`(일반적으로`Offset.zero` 생성자를 사용하여 정의)으로 
애니메이션해야합니다. 이 경우, offset은 
[FractionalTranslation]({{site.api}}/flutter/widgets/FractionalTranslation-class.html) 위젯의 2D 벡터입니다.
`dy` 인자를 1로 설정하여 페이지 전체 높이의 수직 변환을 표현할 수 있습니다.

`transitionsBuilder` 콜백에는 `animation` 매개 변수가 있습니다.
이 매개 변수는 0에서 1 사이의 값을 생성하는 `Animation<double>`입니다. 
Tween을 사용하여 Animation<double>을 Animation<Offset>으로 변환합니다: 

```dart
transitionsBuilder: (context, animation, secondaryAnimation, child) {
  var begin = Offset(0.0, 1.0);
  var end = Offset.zero;
  var tween = Tween(begin: begin, end: end);
  var offsetAnimation = animation.drive(tween);
  return child;
},
```

## 3. AnimatedWidget 사용

Flutter에는 애니메이션 값이 변경 될 때 다시 빌드되는 
[AnimatedWidget]({{site.api}}/flutter/widgets/AnimatedWidget-class.html)을 
확장한 여러 위젯 셋이 있습니다. 
예를 들어 SlideTransition은 `Animation<Offset>`을 가져와서 
애니메이션 값이 변경 될 때마다 자식을 변환합니다(FractionalTranslation 위젯을 사용).

AnimatedWidget은 `Animation<Offset>`과 자식 위젯을 사용하여  
[SlideTransition]({{site.api}}/flutter/widgets/SlideTransition-class.html)을 
반환합니다.

```dart
transitionsBuilder: (context, animation, secondaryAnimation, child) {
  var begin = Offset(0.0, 1.0);
  var end = Offset.zero;
  var tween = Tween(begin: begin, end: end);
  var offsetAnimation = animation.drive(tween);

  return SlideTransition(
    position: offsetAnimation,
    child: child,
  );
},
```

## 4. CurveTween 사용

Flutter는 시간 경과에 따른 애니메이션 비율을 조정하는 
다양한 easing curve을 제공합니다. 
[Curves]({{site.api}}/flutter/animation/Curves-class.html) 클래스에는  
많이 사용되는 curve가 미리 정의되어 있습니다. 
예를 들어, `Curves.easeOut`은
애니메이션이 빠르게 시작됐다가 천천히 끝나는 효과를 만듭니다.

Curve를 사용하려면 새로운 
[CurveTween]({{site.api}}/flutter/animation/CurveTween-class.html)을 
만들어 Curve에 전달하세요:

```dart
var curve = Curves.ease;
var curveTween = CurveTween(curve: curve);
```

이 새로운 트윈은 여전히 0에서 1 사이의 값을 생성합니다. 
다음 단계에서는 2단계에서 만들었던 `Tween<Offset>`이 결합됩니다.

## 5. 두 개의 Tween 조합

여러 tween을 조합하기 위해, 
[chain()]({{site.api}}/flutter/animation/Animatable/chain.html)을 사용하세요:

```dart
var begin = Offset(0.0, 1.0);
var end = Offset.zero;
var curve = Curves.ease;

var tween = Tween(begin: begin, end: end).chain(CurveTween(curve: curve));
```

그런 다음 트윈을 `animation.drive()`에 전달하여 사용하세요.
이렇게하면 `SlideTransition` 위젯에 전달할 수 있는 새로운 `Animation<Offset>`이 생성됩니다:

```dart
return SlideTransition(
  position: animation.drive(tween),
  child: child,
);
```

이 새로운 Tween(혹은 Animatable)은 
먼저 `CurveTween`을 계산하여 `Offset`을 값을 생성한 다음, `Tween<Offset>`을 계산합니다. 
애니메이션이 실행될 때, 값은 다음과 같은 순서로 계산됩니다.

1. (transitionsBuilder 콜백에서 제공되는) animation은 
   0에서 1 사이의 값을 생성합니다.
2. CurveTween은 해당 값을 curve를 기준으로 0과 1 사이의 
   새로운 값으로 매핑합니다.
3. `Tween<Offset>`은 `double` 값을 `Offset` 값에 매핑합니다.

easing curve로 `Animation<Offset>`을 만드는 또 다른 방법은  
`CurvedAnimation`을 사용하는 것입니다:

```dart
transitionsBuilder: (context, animation, secondaryAnimation, child) {
  var begin = Offset(0.0, 1.0); 
  var end = Offset.zero;
  var curve = Curves.ease;
  
  var tween = Tween(begin: begin, end: end);
  var curvedAnimation = CurvedAnimation(
   parent: animation,
   curve: curve,
  );

  return SlideTransition(
   position: tween.animate(curvedAnimation),
   child: child,
  );
}
```

## 전체 예제

```dart
import 'package:flutter/material.dart';

main() {
  runApp(MaterialApp(
    home: Page1(),
  ));
}

class Page1 extends StatelessWidget {
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: RaisedButton(
          child: Text('Go!'),
          onPressed: () {
            Navigator.of(context).push(_createRoute());
          },
        ),
      ),
    );
  }
}

Route _createRoute() {
  return PageRouteBuilder(
    pageBuilder: (context, animation, secondaryAnimation) => Page2(),
    transitionsBuilder: (context, animation, secondaryAnimation, child) {
      var begin = Offset(0.0, 1.0);
      var end = Offset.zero;
      var curve = Curves.ease;

      var tween = Tween(begin: begin, end: end).chain(CurveTween(curve: curve));

      return SlideTransition(
        position: animation.drive(tween),
        child: child,
      );
    },
  );
}

class Page2 extends StatelessWidget {
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: Text('Page 2'),
      ),
    );
  }
}
```

![Demo showing a custom page route transition animating up from the bottom of the screen](/images/cookbook/page-route-animation.gif){:.site-mobile-screenshot}
