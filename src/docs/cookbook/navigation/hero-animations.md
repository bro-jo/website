---
title: 화면을 넘나드는 위젯 애니메이션
prev:
  title: 서비스에 에러 보고 하기
  path: /docs/cookbook/maintenance/error-reporting
next:
  title: 새로운 화면으로 이동하고, 되돌아오기
  path: /docs/cookbook/navigation/navigation-basics
---

한 화면에서 다른 화면으로 넘어갈 때, 사용자에게 어떠한 가이드를 주는 것은 종종 도움이 됩니다.
앱에서 사용할 수 있는 일반적인 기술은 다음 화면으로 전환할 때 위젯에 애니메이션 효과를 주는 것입니다.
이러한 방법은 두 화면을 이어주는 시각적 연결 고리를 만들어 줍니다.

Flutter에서는 한 화면에서 다음 화면으로 전환할 때 위젯에 어떻게 애니메이션 효과를 줄 수 있을까요?
[`Hero`]({{site.api}}/flutter/widgets/Hero-class.html) 위젯이 그 비법입니다!

## 진행 단계

  1. 같은 이미지를 보여주는 2개의 화면을 만듭니다.
  2. 첫 번째 화면에 `Hero` 위젯을 추가합니다.
  3. 두 번째 화면에 `Hero` 위젯을 추가합니다.

## 1. 같은 이미지를 보여주는 2개의 화면을 만듭니다.

이 예제에서는 같은 이미지를 두 개의 화면 모두에 보여줄 것입니다. 첫 번째 화면에서 사용자가 이미지를 탭하면
두 번째 화면으로 전환되면서 애니메이션 효과를 주는 것이 목표입니다. 일단 시각적 구조를 만들고, 본격적인 
애니메이션은 다음 단계에서 다루겠습니다.

*참고:* 이 예제는 
[새로운 화면으로 이동하고, 되돌아오기](/docs/cookbook/navigation/navigation-basics)
와 [탭 다루기](/docs/cookbook/gestures/handling-taps) 예제를 기반으로 합니다.

```dart
class MainScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Main Screen'),
      ),
      body: GestureDetector(
        onTap: () {
          Navigator.push(context, MaterialPageRoute(builder: (_) {
            return DetailScreen();
          }));
        },
        child: Image.network(
          'https://picsum.photos/250?image=9',
        ),
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GestureDetector(
        onTap: () {
          Navigator.pop(context);
        },
        child: Center(
          child: Image.network(
            'https://picsum.photos/250?image=9',
          ),
        ),
      ),
    );
  }
}
```

## 2. 첫 번째 화면에 `Hero` 위젯을 추가합니다.

두 화면을 하나의 애니메이션으로 연결하기 위해 각 화면에 존재하는 `Image`위젯을 `Hero`위젯으로
감쌉니다. `Hero`위젯에 2개의 인자를 넘겨주어야 합니다:

  1. `tag`: `Hero`위젯을 식별하기 위한 객체로 양쪽 모두 동일한 값을 가져야 합니다.
  2. `child`: 화면 전환 시 애니메이션 효과를 적용할 위젯입니다.

<!-- skip -->
```dart
Hero(
  tag: 'imageHero',
  child: Image.network(
    'https://picsum.photos/250?image=9',
  ),
);
```

## 3. 두 번째 화면에 `Hero` 위젯을 추가합니다.

첫 번째 화면과의 연결하기 위해, 두 번째 화면의 `Image`도 `Hero`위젯으로 감싸주어야 합니다! 이 때,
첫 번째 화면에 사용한 것과 동일한 `tag`를 사용해야 한다는 걸 명심하세요.

두 번째 화면에 `Hero`위젯을 적용하면, 애니메이션이 제대로 동작하는 것을 확인할 수 있습니다!

<!-- skip -->
```dart
Hero(
  tag: 'imageHero',
  child: Image.network(
    'https://picsum.photos/250?image=9',
  ),
);
```

참고: 이 코드는 첫 번째 화면에 사용했던 것과 동일한 코드입니다! 이 예제는 데모 목적이기 떄문에 중복하여 
사용했지만, 실제 코드를 작성하실 땐 재사용 가능한 위젯을 따로 만들어 사용하세요. 

## 완성된 예제

```dart
import 'package:flutter/material.dart';

void main() => runApp(HeroApp());

class HeroApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Transition Demo',
      home: MainScreen(),
    );
  }
}

class MainScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Main Screen'),
      ),
      body: GestureDetector(
        child: Hero(
          tag: 'imageHero',
          child: Image.network(
            'https://picsum.photos/250?image=9',
          ),
        ),
        onTap: () {
          Navigator.push(context, MaterialPageRoute(builder: (_) {
            return DetailScreen();
          }));
        },
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GestureDetector(
        child: Center(
          child: Hero(
            tag: 'imageHero',
            child: Image.network(
              'https://picsum.photos/250?image=9',
            ),
          ),
        ),
        onTap: () {
          Navigator.pop(context);
        },
      ),
    );
  }
}
```

![Hero Demo](/images/cookbook/hero.gif){:.site-mobile-screenshot}
