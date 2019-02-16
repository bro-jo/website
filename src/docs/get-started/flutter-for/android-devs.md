---
title: 안드로이드 개발자를 위한 Flutter
description: Learn how to apply Android developer knowledge when building Flutter apps.
---

이 문서는 기존 안드로이드 지식을 활용하여 
Flutter 모바일 앱을 개발하고자 하는 안드로이드 개발자를 대상으로 합니다.
안드로이드 프레임워크의 기본 내용을 이해하고 있다면,
이 문서를 활용하여 Flutter 개발로 도약할 수 있습니다.

Flutter가 다양한 기능과 구성을 위해 모바일 운영체제를 사용하기 때문에 
안드로이드 관련 지식과 기술은 Flutter 앱을 만들 때 매우 유용합니다.
Flutter는 모바일에서 UI를 만드는 새로운 방법이지만,
UI 이외 작업을 위해, 안드로이드 (그리고 iOS)와 통신하는 플러그인 시스템도 가지고 있습니다.
안드로이드의 전문가라면, Flutter를 사용하기 위해 모든 것을 다시 배울 필요가 없습니다.

필요한 부분에 가장 적합한 질문을 찾아내는 방식으로 이 문서를 요리책(cookbook)처럼 활용하실 수도 있습니다.

## 뷰

### Flutter에서 `View`와 동일한 것은?

{{site.alert.secondary}}
react-style 프로그래밍(또는 선언적 프로그래밍)이 기존 명령형 스타일과 어떻게 다를까요? 
비교를 위해, [선언적 UI 소개](/docs/get-started/flutter-for/declarative)를 참조하세요.
{{site.alert.end}}

안드로이드에서, `위젯`는 화면에 나타나는 모든 것의 기반입니다. 
버튼, 툴바, 입력창 등 모든 것이 뷰입니다.
Flutter에서는 `위젯`이 `뷰`와 유사합니다. 
위젯이 안드로이드의 뷰와 정확하게 일치하는 건 아니지만, 
Flutter를 익힐 때 위젯이 "UI를 선언하고 구성하는 방식"이라고 이해할 수 있습니다.  

하지만, 위젯은 `뷰`와 조금 차이가 있습니다. 먼저, 생명주기(lifespan)가 다릅니다.
위젯은 변경 불가능하며 변경이 필요할 때까지만 존재합니다. 
위젯 혹은 위젯의 상태가 변경되면. Flutter는 위젯 인스턴스의 새로운 트리를 생성합니다.
반면, 안드로이드의 뷰는 한 번만 그려지고, `invalidate`가 호출되기 전까지는 다시 그리지 않습니다. 

Flutter의 위젯은 불변하기 때문에 가볍습니다.
위젯이 그 자체로 뷰가 아니기에 어떤 것도 직접 그리지 않고,
대신 UI에 대한 설명이며 내부적으로 이미 "inflate"된 실제 뷰 객체 UI의 의미론(semantics)이기 때문입니다.

Flutter는 [머터리얼 컴포넌트]({{site.material}}/develop/flutter/) 라이브러리를 포함합니다.
위젯은 [머터리얼 디자인 가이드라인]({{site.material}}/design/)을 따르고 있습니다.
머터리얼 디자인은 [모든 플랫폼에 최적화된]({{site.material}}/design/platform-guidance/cross-platform-adaptation.html#cross-platform-guidelines)
유연한 디자인 시스템입니다. 

그러나 Flutter는 모든 디자인 언어를 적용할 수 있을만큼 유연하고 표현력이 우수합니다.
예를 들어, iOS에서는 [쿠퍼티노(Cupertino) 위젯](/docs/development/ui/widgets/cupertino)을 적용하여
[애플의 iOS 디자인 언어](https://developer.apple.com/design/resources/)와 유사한 인터페이스를 만들 수 있습니다.

### `Widget`을 변경하는 방법은 무엇입니까?

안드로이드에서는 뷰를 직접 수정하여 변경사항을 적용합니다. 
하지만 Flutter에서 `위젯`은 불변이기 때문에 직접 변경할 수 없고, 대신 위젯의 state를 변경할 수 있습니다. 

이게 Stateful 위젯과 Stateless 위젯 개념이 탄생한 배경입니다.
`StatelessWidget`은 말그대로 상태 정보가 없는 위젯입니다.  

`StatelessWidget`은 작성하려는 사용자 인터페이스 부분이 
구성 정보 외에 다른 어떤 정보에도 의존하지 않을 때 유용합니다.

예를 들어, 안드로이드에서 `ImageView`로 로고를 배치하는 경우와 비슷합니다.
로고는 실행 중에 변경되지 않기 때문에, Flutter에선 `StatelessWidget`를 사용합니다.

HTTP 호출이나 사용자와의 상호작용을 통해 받은 데이터를 기반으로 UI를 동적으로 변경하기를 원한다면
`StatefulWidget`을 사용할 수 있고 
Flutter 프레임워크에 `State`가 변경되었다고 알려주면 위젯이 변경됩니다.

중요한 점은 stateless와 stateful 모두
핵심 부분은 동일하게 동작한다는 점입니다.
둘 다 모든 프레임을 다시 빌드합니다,
차이는 `StatefulWidget`는 
프레임 전체에 걸쳐 상태를 데이터를 저장하고 다시반환하는 `State` 객체를 가지고 있다는 점입니다.

의문점이 남아있다면, 이 규칙을 항상 기억하세요: 
위젯이 변경되면 (예를 들어 사용자와 상호 작용으로 인해) stateful입니다.
하지만, 위젯이 변경되어도 상위 위젯 자신이 변경되지 않는다면, 
상위 위젯은 stateless 일 수 있습니다.

아래 예제는 `StatelessWidget`을 사용하는 방법을 보여줍니다. 
`Text` 위젯은 일반적인 `StatelessWidget`입니다.
`Text` 위젯의 구현을 보면 `StatelessWidget`의 하위클래스라는 걸 알 수 있습니다.

{% prettify dart %}
Text(
  'I like Flutter!',
  style: TextStyle(fontWeight: FontWeight.bold),
);
{% endprettify %}

보다시피, `Text` 위젯은 생성자로 전달된 것들을 그릴 뿐이고, 그 자체에 연결된 상태 정보는 없습니다.

하지만, 예를 들어 `FloatingActionButton`을 클릭할 때 
"I Like Flutter"를 동적으로 변경하고 싶다면 어떻게 할까요?

그러고 싶다면, `Text`를 `StatefulWidget`으로 감싼 뒤 
사용자가 버튼을 클릭할 때 그것을 변경하면 됩니다.

예시:

{% prettify dart %}
import 'package:flutter/material.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  // Default placeholder text
  String textToShow = "I Like Flutter";

  void _updateText() {
    setState(() {
      // update the text
      textToShow = "Flutter is Awesome!";
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: Center(child: Text(textToShow)),
      floatingActionButton: FloatingActionButton(
        onPressed: _updateText,
        tooltip: 'Update Text',
        child: Icon(Icons.update),
      ),
    );
  }
}
{% endprettify %}

### 위젯을 어떻게 배치하나요? XML 레이아웃 파일은 어디에 있나요?

안드로이드에서는 레이아웃을 XML에 작성하지만, Flutter에서는 레이아웃을 위젯 트리로 작성합니다. 

아래 예시는 여백과 함께 간단한 위젯을 배치하는 방법을 보여줍니다:

{% prettify dart %}
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: Center(
        child: MaterialButton(
          onPressed: () {},
          child: Text('Hello'),
          padding: EdgeInsets.only(left: 10.0, right: 10.0),
        ),
      ),
    );
  }
{% endprettify %}


[위젯 카탈로그](/docs/development/ui/widgets/layout)에서
Flutter가 제공하는 레이아웃을 볼 수 있습니다.

### 어떻게 하면 레이아웃에서 컴포넌트를 추가하거나 제거할 수 있나요?

안드로이드에서는 `addChild()` 혹은 `removeChild()`를 호출하여 
동적으로 자식 뷰를 추가하거나 제거합니다.
Flutter에서는 위젯이 불변이기 때문에 `addChild()`와 동일한 명령은 없습니다.
대신, 부모에게 위젯을 리턴하는 함수를 전달하고, 
그 자식의 생성 여부를 boolean flag로 제어할 수 있습니다.  

예시로, `FloatingActionButton`을 클릭했을 때 두 위젯을 번갈아 보여주는 방법입니다: 

{% prettify dart %}
import 'package:flutter/material.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  // Default value for toggle
  bool toggle = true;
  void _toggle() {
    setState(() {
      toggle = !toggle;
    });
  }

  _getToggleChild() {
    if (toggle) {
      return Text('Toggle One');
    } else {
      return MaterialButton(onPressed: () {}, child: Text('Toggle Two'));
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: Center(
        child: _getToggleChild(),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _toggle,
        tooltip: 'Update Text',
        child: Icon(Icons.update),
      ),
    );
  }
}
{% endprettify %}

### 위젯에 애니메이션을 넣는 방법은?

안드로이드에서는 XML을 사용하거나 뷰에서 `animate()` 메서드를 호출하여 애니메이션을 만듭니다.
Flutter에서는 애니메이션용 위젯으로 위젯을 감싸는 방식으로 애니메이션 라이브러리를 사용하여 애니메이션을 만드세요.

Flutter에서는 중지하고, 탐색하고, 정지하고, 되감기할 수 있는 `Animation<double>`인 `AnimationController`를 사용하세요.
vsync가 발생할 때 알려주고 
작동하는 동안 각 프레임에서 0과 1 사이의 직선 궤적(linear interpolation)을 생성하는 `Ticker`도 필요합니다. 
그런 다음 1개 이상의 `Animation`을 만들어 controller에 연결하세요. 

예를 들어, `CurvedAnimation`을 사용하여 곡선 궤적(interpolated curve)을 따르는 애니메이션을 구현할 수 있습니다.
이러한 의미에서 controller는 애니메이션 진행의 "주" 원천이고
`CurvedAnimation`은 계산하는 기본 선형 동작 대신 커브로 대체하여 계산합니다.
Flutter에서는 애니메이션도 위젯처럼 구성물입니다.

위젯 트리를 만들 때
`Animation`을  `FadeTransition`의 opacity와 같은 
애니메이션이 필요한 속성에 할당하고 
controller에 애니메이션을 시작하도록 지시합니다.

다음 예에서는 `FloatingActionButton`을 누르면
위젯이 로고로 바뀌는 `FadeTransition`을 작성하는 방법을 보여줍니다.

{% prettify dart %}
import 'package:flutter/material.dart';

void main() {
  runApp(FadeAppTest());
}

class FadeAppTest extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Fade Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyFadeTest(title: 'Fade Demo'),
    );
  }
}

class MyFadeTest extends StatefulWidget {
  MyFadeTest({Key key, this.title}) : super(key: key);
  final String title;
  @override
  _MyFadeTest createState() => _MyFadeTest();
}

class _MyFadeTest extends State<MyFadeTest> with TickerProviderStateMixin {
  AnimationController controller;
  CurvedAnimation curve;

  @override
  void initState() {
    super.initState();
    controller = AnimationController(duration: const Duration(milliseconds: 2000), vsync: this);
    curve = CurvedAnimation(parent: controller, curve: Curves.easeIn);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
          child: Container(
              child: FadeTransition(
                  opacity: curve,
                  child: FlutterLogo(
                    size: 100.0,
                  )))),
      floatingActionButton: FloatingActionButton(
        tooltip: 'Fade',
        child: Icon(Icons.brush),
        onPressed: () {
          controller.forward();
        },
      ),
    );
  }
}
{% endprettify %}

자세한 내용은 
[애니메이션 & 동작 위젯](/docs/development/ui/widgets/animation), 
[애니메이션 튜토리얼](/docs/development/ui/animations/tutorial),
[애니메이션 개요](/docs/development/ui/animations)를
참조하세요

### `Canvas`를 사용하여 그리는 방법은?

화면에 이미지와 모양을 그리기 위해 안드로이드에서는 `Canvas`와 `Drawable`을 사용합니다.
Flutter도 저수준(low-level) 렌더링 엔진인 Skia를 사용하기 때문에 `Canvas`와 유사한 API를 가지고 있습니다,
그렇기 때문에 Flutter에서 캔버스에 그리는 것은 안드로이드 개발자에게 매우 익숙한 작업입니다. 

Flutter는 캔버스에 그리는 일을 도와주는 2개의 클래스를 가지고 있습니다.
`CustomPaint`와 `CustomPainter`입니다. 
`CustomPainter`는 캔버스에 어떻게 그릴지 알고리즘을 구현합니다.

Flutter에 서명 그림 그리기를 구현하는 방법은 [StackOverflow][]에 있는 Collin의 답변을 참고하세요.

[StackOverflow]: {{site.so}}/questions/46241071/create-signature-area-for-mobile-app-in-dart-flutter

{% prettify dart %}
import 'package:flutter/material.dart';

void main() => runApp(MaterialApp(home: DemoApp()));

class DemoApp extends StatelessWidget {
  Widget build(BuildContext context) => Scaffold(body: Signature());
}

class Signature extends StatefulWidget {
  SignatureState createState() => SignatureState();
}

class SignatureState extends State<Signature> {
  List<Offset> _points = <Offset>[];
  Widget build(BuildContext context) {
    return GestureDetector(
      onPanUpdate: (DragUpdateDetails details) {
        setState(() {
          RenderBox referenceBox = context.findRenderObject();
          Offset localPosition =
          referenceBox.globalToLocal(details.globalPosition);
          _points = List.from(_points)..add(localPosition);
        });
      },
      onPanEnd: (DragEndDetails details) => _points.add(null),
      child: CustomPaint(painter: SignaturePainter(_points), size: Size.infinite),
    );
  }
}

class SignaturePainter extends CustomPainter {
  SignaturePainter(this.points);
  final List<Offset> points;
  void paint(Canvas canvas, Size size) {
    var paint = Paint()
      ..color = Colors.black
      ..strokeCap = StrokeCap.round
      ..strokeWidth = 5.0;
    for (int i = 0; i < points.length - 1; i++) {
      if (points[i] != null && points[i + 1] != null)
        canvas.drawLine(points[i], points[i + 1], paint);
    }
  }
  bool shouldRepaint(SignaturePainter other) => other.points != points;
}
{% endprettify %}

### 커스텀 위젯을 만드는 방법은?

안드로이드에서는 
보통 `View`의 하위클래스를 만들거나 이미 있는 View를 사용하여
원하는 동작을 수행하는 메서드를 오버라이드하고 구현합니다.

Flutter에서는 
[더 작은 위젯들로 구성된](/docs/resources/technical-overview#everythings-a-widget) 새로운 위젯을 만듭니다
(상속 대신).

이것은 구성물(building block)이 이미 준비되어 있는 상황에서 약간 다른 동작을 만들고 싶을 때
(예를 들면, 새로운 레이아웃 로직을 첨가하고 싶을 때)
안드로이드에서 `ViewGroup`을 만드는 것과 유사합니다.

예를 들어, `CustomButton`의 생성자가 label을 받게 하려면 어떻게 해야 할까요?
`RaisedButton`을 상속하는 대신,
`RaisedButton`을 감싸고 있는 CustomButton을 만들고 생성자에 label을 받게 하면 됩니다.

{% prettify dart %}
class CustomButton extends StatelessWidget {
  final String label;

  CustomButton(this.label);

  @override
  Widget build(BuildContext context) {
    return RaisedButton(onPressed: () {}, child: Text(label));
  }
}
{% endprettify %}

그런 다음 다른 Flutter 위젯처럼 `CustomButton`을 사용하기만 하면 됩니다:

{% prettify dart %}
@override
Widget build(BuildContext context) {
  return Center(
    child: CustomButton("Hello"),
  );
}
{% endprettify %}

## Intents (인텐트)

### Flutter에서 `Intent`와 동일한 것은?

안드로이드에서 `Intent`는 두 가지 용도로 사용됩니다:
엑티비티 간 이동, 그리고 다른 컴포넌트와 통신할 때입니다.
반면, Flutter는 intent라는 개념을 가지고 있지 않습니다.
네이티브 통합을 활용하여 intent를 사용할 수는 있습니다.
([플러그인]({{site.pub}}/packages/android_intent)을 사용)

Flutter는 실제로 액티비티나 프레그먼트와 직접적으로 동등한 요소를 가지고 있지 않습니다.
Flutter에서는 하나의 `Activity`안에서 `Navigator`와 `Route`를 활용하여 스크린 간 내비게이션을 합니다.

`Route`는 앱의 “스크린”이나 “페이지”를 추상화한 것이고,
`Navigator`는 route를 관리하는 위젯입니다.
`Activity`와 route는 유사하지만, 똑같은 의미는 아닙니다.
Navigator는 routes를 push나 pop을 하여 스크린 간 이동을 할 수 있습니다.
Navigator는 스텍처럼 동작합니다. 
이동하려는 새로운 route로 `push()`할 수 있고,
되돌아가려면 `pop()`하면 됩니다.

안드로이드에서는 앱의 `AndroidManifest.xml`에 액티비티를 정의합니다.

Flutter에서는 페이지 간 이동을 하는 몇 가지 방법이 있습니다:

* route 이름의 `Map`을 지정한다. (MaterialApp)
* route로 바로 이동한다. (WidgetApp)

아래는 Map을 지정하는 방식입니다.

{% prettify dart %}
 void main() {
  runApp(MaterialApp(
    home: MyAppHome(), // becomes the route named '/'
    routes: <String, WidgetBuilder> {
      '/a': (BuildContext context) => MyPage(title: 'page A'),
      '/b': (BuildContext context) => MyPage(title: 'page B'),
      '/c': (BuildContext context) => MyPage(title: 'page C'),
    },
  ));
}
{% endprettify %}

`Navigator`에 route의 이름을 `push`하여 이동할 수 있습니다. 

{% prettify dart %}
Navigator.of(context).pushNamed('/b');
{% endprettify %}

`Intent`의 또다른 용도는 카메라나 파일 선택 도구같은 외부 컴포넌트를 를 호출하는 것입니다.
이 작업을 위해서는 네이티브 플랫폼 통합이 필요합니다.
(또는 [이미 있는 플러그인]({{site.pub}}/flutter/)을 활용하는 방법도 있습니다)

네이티브 플랫폼 통합을 배우기 위해서는, 
[패키지 및 플러그인 개발](/docs/development/packages-and-plugins/developing-packages)를 
참조하세요.

### 앱 외부에서 intent가 넘어올 때는 어떻게 처리해야?  

Flutter는 안드로이드 레이어와 직접 통신하고
공유된 데이터를 요청하여 외부에서 넘어오는 intent를 처리할 수 있습니다.   

아래 예제는
Flutter 코드를 실행하는 네이티브 액티비티에서 텍스트를 공유하는 intent filter를 등록해서,
다른 앱은 Flutter 앱과 텍스트를 공유할 수 있도록 합니다.

기본 흐름은 
먼저 안드로이드 네이티브 쪽(우리의 `Activity`)에 공유된 텍스트 데이터를 처리하고,
Flutter가 `MethodChannel`을 이용하여 데이터를 요청할 때까지 기다립니다.

먼저, `AndroidManifest.xml`에 모든 intent를 위한 intent filter를 등록하세요:

{% prettify xml %}
<activity
  android:name=".MainActivity"
  android:launchMode="singleTop"
  android:theme="@style/LaunchTheme"
  android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale|layoutDirection"
  android:hardwareAccelerated="true"
  android:windowSoftInputMode="adjustResize">
  <!-- ... -->
  <intent-filter>
    <action android:name="android.intent.action.SEND" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:mimeType="text/plain" />
  </intent-filter>
</activity>
{% endprettify %}

그런 다음 `MainActivity`에서 intent를 처리하고, intent에 공유된 텍스트를 추출한 후 저장해둡니다.
처리를 시작할 준비가 되면 Flutter가 플랫폼 채널을 사용해서 데이터를 요청하고, 네이티브 쪽에서 데이터가 전송됩니다.

{% prettify java %}
package com.example.shared;

import android.content.Intent;
import android.os.Bundle;

import java.nio.ByteBuffer;

import io.flutter.app.FlutterActivity;
import io.flutter.plugin.common.ActivityLifecycleListener;
import io.flutter.plugin.common.MethodCall;
import io.flutter.plugin.common.MethodChannel;
import io.flutter.plugin.common.MethodChannel.MethodCallHandler;
import io.flutter.plugins.GeneratedPluginRegistrant;

public class MainActivity extends FlutterActivity {

  private String sharedText;

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    GeneratedPluginRegistrant.registerWith(this);
    Intent intent = getIntent();
    String action = intent.getAction();
    String type = intent.getType();

    if (Intent.ACTION_SEND.equals(action) && type != null) {
      if ("text/plain".equals(type)) {
        handleSendText(intent); // Handle text being sent
      }
    }

    new MethodChannel(getFlutterView(), "app.channel.shared.data").setMethodCallHandler(
      new MethodCallHandler() {
        @Override
        public void onMethodCall(MethodCall call, MethodChannel.Result result) {
          if (call.method.contentEquals("getSharedText")) {
            result.success(sharedText);
            sharedText = null;
          }
        }
      });
  }

  void handleSendText(Intent intent) {
    sharedText = intent.getStringExtra(Intent.EXTRA_TEXT);
  }
}
{% endprettify %}

마지막으로, 위젯이 렌더링 될 때 Flutter쪽에서 데이터를 요청하도록 하세요:

{% prettify dart %}
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample Shared App Handler',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  static const platform = const MethodChannel('app.channel.shared.data');
  String dataShared = "No data";

  @override
  void initState() {
    super.initState();
    getSharedText();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(body: Center(child: Text(dataShared)));
  }

  getSharedText() async {
    var sharedData = await platform.invokeMethod("getSharedText");
    if (sharedData != null) {
      setState(() {
        dataShared = sharedData;
      });
    }
  }
}
{% endprettify %}

### `startActivityForResult()`와 동일한 것은?


`Navigator` 클래스는 Flutter에서 route를 처리해주고, 스택에 푸시한 route에서 결과를 다시 얻을 때 사용됩니다.
`push()`에서 리턴되는 `Future`를 `await`해서 이 작업을 할 수 있습니다.

예를 들어, 아래 코드처럼 사용자가 위치를 선택할 수 있도록 location route로 이동할 수 있습니다:

{% prettify dart %}
Map coordinates = await Navigator.of(context).pushNamed('/location');
{% endprettify %}

그런 다음, location route 안에서, 사용자가 위치를 선택하면 결과와 함께 스택에서 `pop`을 할 수 있습니다: 

{% prettify dart %}
Navigator.of(context).pop({"lat":43.821757,"long":-79.226392});
{% endprettify %}

## Async UI

### What is the equivalent of `runOnUiThread()` in Flutter?

Dart has a single-threaded execution model, with support for `Isolate`s
(a way to run Dart code on another thread), an event loop, and
asynchronous programming. Unless you spawn an `Isolate`, your Dart code
runs in the main UI thread and is driven by an event loop. Flutter's event
loop is equivalent to Android's main `Looper`&mdash;that is, the `Looper` that
is attached to the main thread.

Dart's single-threaded model doesn't mean you need to run everything as a
blocking operation that causes the UI to freeze. Unlike Android, which
requires you to keep the main thread free at all times, in Flutter,
use the asynchronous facilities that the Dart language provides, such as
`async`/`await`, to perform asynchronous work. You may be familiar with
the `async`/`await` paradigm if you've used it in C#, Javascript, or if you
have used Kotlin's coroutines.

For example, you can run network code without causing the UI to hang by
using `async`/`await` and letting Dart do the heavy lifting:

{% prettify dart %}
loadData() async {
  String dataURL = "https://jsonplaceholder.typicode.com/posts";
  http.Response response = await http.get(dataURL);
  setState(() {
    widgets = json.decode(response.body);
  });
}
{% endprettify %}

Once the `await`ed network call is done, update the UI by calling `setState()`,
which triggers a rebuild of the widget sub-tree and updates the data.

The following example loads data asynchronously and displays it in a `ListView`:

{% prettify dart %}
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();

    loadData();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: ListView.builder(
          itemCount: widgets.length,
          itemBuilder: (BuildContext context, int position) {
            return getRow(position);
          }));
  }

  Widget getRow(int i) {
    return Padding(
      padding: EdgeInsets.all(10.0),
      child: Text("Row ${widgets[i]["title"]}")
    );
  }

  loadData() async {
    String dataURL = "https://jsonplaceholder.typicode.com/posts";
    http.Response response = await http.get(dataURL);
    setState(() {
      widgets = json.decode(response.body);
    });
  }
}
{% endprettify %}

Refer to the next section for more information on doing work in the
background, and how Flutter differs from Android.

### How do you move work to a background thread?

In Android, when you want to access a network resource you would typically
move to a background thread and do the work, as to not block the main thread,
and avoid ANRs. For example, you may be using an `AsyncTask`, a `LiveData`,
an `IntentService`, a `JobScheduler` job, or an RxJava pipeline with a
scheduler that works on background threads.

Since Flutter is single threaded and runs an event loop (like Node.js), you
don't have to worry about thread management or spawning background threads. If
you're doing I/O-bound work, such as disk access or a network call, then
you can safely use `async`/`await` and you're all set. If, on the other
hand, you need to do computationally intensive work that keeps the CPU busy,
you want to move it to an `Isolate` to avoid blocking the event loop, like
you would keep _any_ sort of work out of the main thread in Android.

For I/O-bound work, declare the function as an `async` function,
and `await` on long-running tasks inside the function:

{% prettify dart %}
loadData() async {
  String dataURL = "https://jsonplaceholder.typicode.com/posts";
  http.Response response = await http.get(dataURL);
  setState(() {
    widgets = json.decode(response.body);
  });
}
{% endprettify %}

This is how you would typically do network or database calls, which are both
I/O operations.

On Android, when you extend `AsyncTask`, you typically override 3 methods,
`onPreExecute()`, `doInBackground()` and `onPostExecute()`. There is no
equivalent in Flutter, since you `await` on a long running function, and
Dart's event loop takes care of the rest.

However, there are times when you might be processing a large amount of data and
your UI hangs. In Flutter, use `Isolate`s to take advantage of
multiple CPU cores to do long-running or computationally intensive tasks.

Isolates are separate execution threads that do not share any memory
with the main execution memory heap. This means you can’t access variables from
the main thread, or update your UI by calling `setState()`. Unlike Android threads,
Isolates are true to their name, and cannot share memory (in the form of static fields,
for example).

The following example shows, in a simple isolate, how to share data back to
the main thread to update the UI.

{% prettify dart %}
loadData() async {
  ReceivePort receivePort = ReceivePort();
  await Isolate.spawn(dataLoader, receivePort.sendPort);

  // The 'echo' isolate sends its SendPort as the first message
  SendPort sendPort = await receivePort.first;

  List msg = await sendReceive(sendPort, "https://jsonplaceholder.typicode.com/posts");

  setState(() {
    widgets = msg;
  });
}

// The entry point for the isolate
static dataLoader(SendPort sendPort) async {
  // Open the ReceivePort for incoming messages.
  ReceivePort port = ReceivePort();

  // Notify any other isolates what port this isolate listens to.
  sendPort.send(port.sendPort);

  await for (var msg in port) {
    String data = msg[0];
    SendPort replyTo = msg[1];

    String dataURL = data;
    http.Response response = await http.get(dataURL);
    // Lots of JSON to parse
    replyTo.send(json.decode(response.body));
  }
}

Future sendReceive(SendPort port, msg) {
  ReceivePort response = ReceivePort();
  port.send([msg, response.sendPort]);
  return response.first;
}
{% endprettify %}

Here, `dataLoader()` is the `Isolate` that runs in its own separate execution thread.
In the isolate you can perform more CPU intensive processing (parsing a big JSON, for
example), or perform computationally intensive math, such as encryption or signal processing.

You can run the full example below:

{% prettify dart %}
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:async';
import 'dart:isolate';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();
    loadData();
  }

  showLoadingDialog() {
    if (widgets.length == 0) {
      return true;
    }

    return false;
  }

  getBody() {
    if (showLoadingDialog()) {
      return getProgressDialog();
    } else {
      return getListView();
    }
  }

  getProgressDialog() {
    return Center(child: CircularProgressIndicator());
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text("Sample App"),
        ),
        body: getBody());
  }

  ListView getListView() => ListView.builder(
      itemCount: widgets.length,
      itemBuilder: (BuildContext context, int position) {
        return getRow(position);
      });

  Widget getRow(int i) {
    return Padding(padding: EdgeInsets.all(10.0), child: Text("Row ${widgets[i]["title"]}"));
  }

  loadData() async {
    ReceivePort receivePort = ReceivePort();
    await Isolate.spawn(dataLoader, receivePort.sendPort);

    // The 'echo' isolate sends its SendPort as the first message
    SendPort sendPort = await receivePort.first;

    List msg = await sendReceive(sendPort, "https://jsonplaceholder.typicode.com/posts");

    setState(() {
      widgets = msg;
    });
  }

  // the entry point for the isolate
  static dataLoader(SendPort sendPort) async {
    // Open the ReceivePort for incoming messages.
    ReceivePort port = ReceivePort();

    // Notify any other isolates what port this isolate listens to.
    sendPort.send(port.sendPort);

    await for (var msg in port) {
      String data = msg[0];
      SendPort replyTo = msg[1];

      String dataURL = data;
      http.Response response = await http.get(dataURL);
      // Lots of JSON to parse
      replyTo.send(json.decode(response.body));
    }
  }

  Future sendReceive(SendPort port, msg) {
    ReceivePort response = ReceivePort();
    port.send([msg, response.sendPort]);
    return response.first;
  }
}
{% endprettify %}

### What is the equivalent of OkHttp on Flutter?

Making a network call in Flutter is easy when you use the popular
[`http` package]({{site.pub}}/packages/http).

While the http package doesn't have every feature found in OkHttp,
it abstracts away much of the networking that you would normally implement
yourself, making it a simple way to make network calls.

To use the `http` package, add it to your dependencies in `pubspec.yaml`:

{% prettify yaml %}
dependencies:
  ...
  http: ^0.11.3+16
{% endprettify %}

To make a network call, call `await` on the `async` function `http.get()`:

{% prettify dart %}
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
[...]
  loadData() async {
    String dataURL = "https://jsonplaceholder.typicode.com/posts";
    http.Response response = await http.get(dataURL);
    setState(() {
      widgets = json.decode(response.body);
    });
  }
}
{% endprettify %}

### How do I show the progress for a long-running task?

In Android you would typically show a `ProgressBar` view in your UI while
executing a long running task on a background thread.

In Flutter, use a `ProgressIndicator` widget.
Show the progress programmatically by controlling when it's rendered
through a boolean flag. Tell Flutter to update its state before your
long-running task starts, and hide it after it ends.

In the following example, the build function is separated into three different
functions. If `showLoadingDialog()` is `true` (when `widgets.length == 0`),
then render the `ProgressIndicator`. Otherwise, render the
`ListView` with the data returned from a network call.

{% prettify dart %}
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];

  @override
  void initState() {
    super.initState();
    loadData();
  }

  showLoadingDialog() {
    return widgets.length == 0;
  }

  getBody() {
    if (showLoadingDialog()) {
      return getProgressDialog();
    } else {
      return getListView();
    }
  }

  getProgressDialog() {
    return Center(child: CircularProgressIndicator());
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text("Sample App"),
        ),
        body: getBody());
  }

  ListView getListView() => ListView.builder(
      itemCount: widgets.length,
      itemBuilder: (BuildContext context, int position) {
        return getRow(position);
      });

  Widget getRow(int i) {
    return Padding(padding: EdgeInsets.all(10.0), child: Text("Row ${widgets[i]["title"]}"));
  }

  loadData() async {
    String dataURL = "https://jsonplaceholder.typicode.com/posts";
    http.Response response = await http.get(dataURL);
    setState(() {
      widgets = json.decode(response.body);
    });
  }
}
{% endprettify %}

## Project structure & resources

### Where do I store my resolution-dependent image files?

While Android treats resources and assets as distinct items, Flutter apps have
only assets. All resources that would live in the `res/drawable-*`
folders on Android, are placed in an assets folder for Flutter.

Flutter follows a simple density-based format like iOS. Assets might be `1.0x`,
`2.0x`, `3.0x`, or any other multiplier. Flutter doesn't have `dp`s but there
are logical pixels, which are basically the same as device-independent pixels.
The so-called
[`devicePixelRatio`]({{site.api}}/flutter/dart-ui/Window/devicePixelRatio.html)
expresses the ratio of physical pixels in a single logical pixel.

The equivalent to Android's density buckets are:

 Android density qualifier | Flutter pixel ratio
 --- | ---
 `ldpi` | `0.75x`
 `mdpi` | `1.0x`
 `hdpi` | `1.5x`
 `xhdpi` | `2.0x`
 `xxhdpi` | `3.0x`
 `xxxhdpi` | `4.0x`

Assets are located in any arbitrary folder&mdash;Flutter has no
predefined folder structure. You declare the assets (with location) in
the `pubspec.yaml` file, and Flutter picks them up.

Note that before Flutter 1.0 beta 2, assets defined in Flutter were not
accessible from the native side, and vice versa, native assets and resources
weren’t available to Flutter, as they lived in separate folders.

As of Flutter beta 2, assets are stored in the native asset folder,
and are accessed on the native side using Android's `AssetManager`:

{% prettify kotlin %}
val flutterAssetStream = assetManager.open("flutter_assets/assets/my_flutter_asset.png")
{% endprettify %}

As of Flutter beta 2, Flutter still cannot access native resources, nor it can
access native assets.

To add a new image asset called `my_icon.png` to our Flutter project, for example,
and deciding that it should live in a folder we arbitrarily called `images`, you
would put the base image (1.0x) in the `images` folder, and all the other
variants in sub-folders called with the appropriate ratio multiplier:

```
images/my_icon.png       // Base: 1.0x image
images/2.0x/my_icon.png  // 2.0x image
images/3.0x/my_icon.png  // 3.0x image
```

Next, you'll need to declare these images in your `pubspec.yaml` file:

{% prettify yaml %}
assets:
 - images/my_icon.jpeg
{% endprettify %}

You can then access your images using `AssetImage`:

{% prettify dart %}
return AssetImage("images/a_dot_burr.jpeg");
{% endprettify %}

or directly in an `Image` widget:

{% prettify dart %}
@override
Widget build(BuildContext context) {
  return Image.asset("images/my_image.png");
}
{% endprettify %}

### Where do I store strings? How do I handle localization?

Flutter currently doesn't have a dedicated resources-like system for strings.
At the moment, the best practice is to hold your copy text in a class as
static fields and accessing them from there. For example:

{% prettify dart %}
class Strings {
  static String welcomeMessage = "Welcome To Flutter";
}
{% endprettify %}

Then in your code, you can access your strings as such:

{% prettify dart %}
Text(Strings.welcomeMessage)
{% endprettify %}

Flutter has basic support for accessibility on Android, though this feature is
a work in progress.

Flutter developers are encouraged to use the [intl
package]({{site.pub}}/packages/intl) for internationalization and
localization.

### What is the equivalent of a Gradle file? How do I add dependencies?

In Android, you add dependencies by adding to your Gradle build script. Flutter
uses Dart's own build system, and the Pub package manager.
The tools delegate the building of the native Android and iOS wrapper apps to the
respective build systems.

While there are Gradle files under the `android` folder in your Flutter project,
only use these if you are adding native dependencies needed for
per-platform integration. In general, use `pubspec.yaml` to declare
external dependencies to use in Flutter. A good place to find Flutter packages is
[Pub]({{site.pub}}/flutter/packages/).

## Activities and fragments

### What are the equivalent of activities and fragments in Flutter?

In Android, an `Activity` represents a single focused thing the user can do. A
`Fragment` represents a behavior or a portion of user interface. Fragments
are a way to modularize your code, compose sophisticated user interfaces for
larger screens, and help scale your application UI. In Flutter, both of these
concepts fall under the umbrella of `Widget`s.

To learn more about the UI for building Activities and Fragements, see
the community-contributed medium article,
[Flutter For Android Developers : How to design an Activity UI in
Flutter]({{site.medium}}/@burhanrashid52/flutter-for-android-developers-how-to-design-activity-ui-in-flutter-4bf7b0de1e48).

As mentioned in the [Intents](#what-is-the-equivalent-of-an-intent-in-flutter)
section, screens in Flutter are represented by `Widget`s since everything is
a widget in Flutter. You use a `Navigator` to move between different `Route`s
that represent different screens or pages, or maybe just different states or
renderings of the same data.

### How do I listen to Android activity lifecycle events?

In Android, you can override methods from the `Activity` to capture lifecycle
methods for the activity itself, or register `ActivityLifecycleCallbacks` on
the `Application`. In Flutter, you have neither concept, but you can instead
listen to lifecycle events by hooking into the `WidgetsBinding` observer and
listening to the `didChangeAppLifecycleState()` change event.

The observable lifecycle events are:

* `inactive` — The application is in an inactive state and is not receiving user
  input. This event only works on iOS, as there is no equivalent event to map to
  on Android
* `paused` — The application is not currently visible to the user, not responding
  to user input, and running in the background. This is equivalent to `onPause()`
  in Android
* `resumed` — The application is visible and responding to user input. This is
  equivalent to `onPostResume()` in Android
* `suspending` — The application is suspended momentarily. This is equivalent
  to `onStop` in Android; it is not triggered on iOS as there is no equivalent
  event to map to on iOS

For more details on the meaning of these states, see the
[`AppLifecycleStatus` documentation][].

[`AppLifecycleStatus` documentation]: {{site.api}}/flutter/dart-ui/AppLifecycleState-class.html

As you might have noticed, only a small minority of the Activity lifecycle events
are available; while `FlutterActivity` does capture almost all the activity lifecycle
events internally and send them over to the Flutter engine, they're mostly shielded
away from you. Flutter takes care of starting and stopping the engine for you, and
there is little reason for needing to observe the activity lifecycle on the Flutter
side in most cases. If you need to observe the lifecycle to acquire or release any
native resources, you should likely be doing it from the native side, at any rate.

Here's an example of how to observe the lifecycle status of the containing activity:

{% prettify dart %}
import 'package:flutter/widgets.dart';

class LifecycleWatcher extends StatefulWidget {
  @override
  _LifecycleWatcherState createState() => _LifecycleWatcherState();
}

class _LifecycleWatcherState extends State<LifecycleWatcher> with WidgetsBindingObserver {
  AppLifecycleState _lastLifecycleState;

  @override
  void initState() {
    super.initState();
    WidgetsBinding.instance.addObserver(this);
  }

  @override
  void dispose() {
    WidgetsBinding.instance.removeObserver(this);
    super.dispose();
  }

  @override
  void didChangeAppLifecycleState(AppLifecycleState state) {
    setState(() {
      _lastLifecycleState = state;
    });
  }

  @override
  Widget build(BuildContext context) {
    if (_lastLifecycleState == null)
      return Text('This widget has not observed any lifecycle changes.', textDirection: TextDirection.ltr);

    return Text('The most recent lifecycle state this widget observed was: $_lastLifecycleState.',
        textDirection: TextDirection.ltr);
  }
}

void main() {
  runApp(Center(child: LifecycleWatcher()));
}
{% endprettify %}

## Layouts

### What is the equivalent of a LinearLayout?

In Android, a LinearLayout is used to lay your widgets out
linearly&mdash;either horizontally or vertically.
In Flutter, use the Row widget or Column
widget to achieve the same result.

If you notice the two code samples are identical with the exception of the
"Row" and "Column" widget. The children are the same and this feature can be
exploited to develop rich layouts that can change overtime with the same
children.

{% prettify dart %}
  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        Text('Row One'),
        Text('Row Two'),
        Text('Row Three'),
        Text('Row Four'),
      ],
    );
  }
{% endprettify %}

{% prettify dart %}
  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        Text('Column One'),
        Text('Column Two'),
        Text('Column Three'),
        Text('Column Four'),
      ],
    );
  }
{% endprettify %}

To 더 알아보기 about building linear layouts, see the community contributed medium
article [Flutter For Android Developers : How to design LinearLayout in
Flutter?]({{site.medium}}/@burhanrashid52/flutter-for-android-developers-how-to-design-linearlayout-in-flutter-5d819c0ddf1a).

### What is the equivalent of a RelativeLayout?

A RelativeLayout lays your widgets out relative to each other. In
Flutter, there are a few ways to achieve the same result.

You can achieve the result of a RelativeLayout by using a combination of
Column, Row, and Stack widgets. You can specify rules for the widgets
constructors on how the children are laid out relative to the parent.

For a good example of building a RelativeLayout in Flutter, see Collin's
answer on
[StackOverflow]({{site.so}}/questions/44396075/equivalent-of-relativelayout-in-flutter).

### What is the equivalent of a ScrollView?

In Android, use a ScrollView to lay out your widgets&mdash;if the user's
device has a smaller screen than your content, it scrolls.

In Flutter, the easiest way to do this is using the ListView widget. This might
seem like overkill coming from Android, but in Flutter a ListView widget is
both a ScrollView and an Android ListView.

{% prettify dart %}
  @override
  Widget build(BuildContext context) {
    return ListView(
      children: <Widget>[
        Text('Row One'),
        Text('Row Two'),
        Text('Row Three'),
        Text('Row Four'),
      ],
    );
  }
{% endprettify %}

### How do I handle landscape transitions in Flutter?

FlutterView handles the config change if AndroidManifest.xml contains:

{% prettify yaml %}
android:configChanges="orientation|screenSize"
{% endprettify %}

## Gesture detection and touch event handling

### How do I add an onClick listener to a widget in Flutter?

In Android, you can attach onClick to views such as button by calling
the method 'setOnClickListener'.

In Flutter there are two ways of adding touch listeners:

 1. If the Widget supports event detection, pass a function to it and handle it
    in the function. For example, the RaisedButton has an `onPressed` parameter:

    <!-- skip -->
    ```dart
    @override
    Widget build(BuildContext context) {
      return RaisedButton(
          onPressed: () {
            print("click");
          },
          child: Text("Button"));
    }
    ```

 2. If the Widget doesn't support event detection, wrap the
    widget in a GestureDetector and pass a function to the `onTap` parameter.

    <!-- skip -->
    ```dart
    class SampleApp extends StatelessWidget {
      @override
      Widget build(BuildContext context) {
        return Scaffold(
            body: Center(
          child: GestureDetector(
            child: FlutterLogo(
              size: 200.0,
            ),
            onTap: () {
              print("tap");
            },
          ),
        ));
      }
    }
    ```

### How do I handle other gestures on widgets?

Using the GestureDetector, you can listen to a wide range of Gestures such as:

* Tap

  * `onTapDown` - A pointer that might cause a tap has contacted the screen at a
     particular location.
  * `onTapUp` - A pointer that triggers a tap has stopped contacting the
     screen at a particular location.
  * `onTap` - A tap has occurred.
  * `onTapCancel` - The pointer that previously triggered the `onTapDown` won't
     cause a tap.

* Double tap

  * `onDoubleTap` - The user tapped the screen at the same location twice in
     quick succession.

* Long press

  * `onLongPress` - A pointer has remained in contact with the screen at the same
    location for a long period of time.

* Vertical drag

  * `onVerticalDragStart` - A pointer has contacted the screen and might begin to
    move vertically.
  * `onVerticalDragUpdate` - A pointer in contact with the screen
    has moved further in the vertical direction.
  * `onVerticalDragEnd` - A pointer that was previously in contact with the
    screen and moving vertically is no longer in contact with the screen and was
    moving at a specific velocity when it stopped contacting the screen.

* Horizontal drag

  * `onHorizontalDragStart` - A pointer has contacted the screen and might begin
    to move horizontally.
  * `onHorizontalDragUpdate` - A pointer in contact with the screen
    has moved further in the horizontal direction.
  * `onHorizontalDragEnd` - A pointer that was previously in contact with the
    screen and moving horizontally is no longer in contact with the screen and was
    moving at a specific velocity when it stopped contacting the screen.

The following example shows a `GestureDetector` that rotates the Flutter logo
on a double tap:

{% prettify dart %}
AnimationController controller;
CurvedAnimation curve;

@override
void initState() {
  controller = AnimationController(duration: const Duration(milliseconds: 2000), vsync: this);
  curve = CurvedAnimation(parent: controller, curve: Curves.easeIn);
}

class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        body: Center(
          child: GestureDetector(
            child: RotationTransition(
                turns: curve,
                child: FlutterLogo(
                  size: 200.0,
                )),
            onDoubleTap: () {
              if (controller.isCompleted) {
                controller.reverse();
              } else {
                controller.forward();
              }
            },
        ),
    ));
  }
}
{% endprettify %}

## Listviews & adapters

### What is the alternative to a ListView in Flutter?

The equivalent to a ListView in Flutter is … a ListView!

In an Android ListView, you create an adapter and pass it into the
ListView, which renders each row with what your adapter returns. However, you
have to make sure you recycle your rows, otherwise, you get all sorts of crazy
visual glitches and memory issues.

Due to Flutter's immutable widget pattern, you pass a List of
Widgets to your ListView, and Flutter takes care of making sure
that scrolling is fast and smooth.

{% prettify dart %}
import 'package:flutter/material.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: ListView(children: _getListData()),
    );
  }

  _getListData() {
    List<Widget> widgets = [];
    for (int i = 0; i < 100; i++) {
      widgets.add(Padding(padding: EdgeInsets.all(10.0), child: Text("Row $i")));
    }
    return widgets;
  }
}
{% endprettify %}

### How do I know which list item is clicked on?

In Android, the ListView has a method to find out which item was clicked
'onItemClickListener'.
In Flutter, use the touch handling provided by the passed-in widgets.

{% prettify dart %}
import 'package:flutter/material.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: ListView(children: _getListData()),
    );
  }

  _getListData() {
    List<Widget> widgets = [];
    for (int i = 0; i < 100; i++) {
      widgets.add(GestureDetector(
        child: Padding(
            padding: EdgeInsets.all(10.0),
            child: Text("Row $i")),
        onTap: () {
          print('row tapped');
        },
      ));
    }
    return widgets;
  }
}
{% endprettify %}

### How do I update ListView's dynamically?

On Android, you update the adapter and call `notifyDataSetChanged`.

In Flutter, if you were to update the list of widgets inside a `setState()`,
you would quickly see that your data did not change visually.
This is because when `setState()` is called, the Flutter rendering engine
looks at the widget tree to see if anything has changed. When it gets to your
`ListView`, it performs a `==` check, and determines that the two `ListView`s are the
same. Nothing has changed, so no update is required.

For a simple way to update your `ListView`, create a new `List` inside of
`setState()`, and copy the data from the old list to the new list.
While this approach is simple, it is not recommended for large data sets,
as shown in the next example.

{% prettify dart %}
import 'package:flutter/material.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = <Widget>[];

  @override
  void initState() {
    super.initState();
    for (int i = 0; i < 100; i++) {
      widgets.add(getRow(i));
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: ListView(children: widgets),
    );
  }

  Widget getRow(int i) {
    return GestureDetector(
      child: Padding(
          padding: EdgeInsets.all(10.0),
          child: Text("Row $i")),
      onTap: () {
        setState(() {
          widgets = List.from(widgets);
          widgets.add(getRow(widgets.length + 1));
          print('row $i');
        });
      },
    );
  }
}
{% endprettify %}

The recommended, efficient, and effective way to build a list uses a
ListView.Builder. This method is great when you have a dynamic
List or a List with very large amounts of data. This is essentially
the equivalent of RecyclerView on Android, which automatically
recycles list elements for you:

{% prettify dart %}
import 'package:flutter/material.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = <Widget>[];

  @override
  void initState() {
    super.initState();
    for (int i = 0; i < 100; i++) {
      widgets.add(getRow(i));
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text("Sample App"),
        ),
        body: ListView.builder(
            itemCount: widgets.length,
            itemBuilder: (BuildContext context, int position) {
              return getRow(position);
            }));
  }

  Widget getRow(int i) {
    return GestureDetector(
      child: Padding(
          padding: EdgeInsets.all(10.0),
          child: Text("Row $i")),
      onTap: () {
        setState(() {
          widgets.add(getRow(widgets.length + 1));
          print('row $i');
        });
      },
    );
  }
}
{% endprettify %}

Instead of creating a "ListView", create a ListView.builder that
takes two key parameters: the initial length of the list, and an ItemBuilder
function.

The ItemBuilder function is similar to the `getView` function in an Android
adapter; it takes a position, and returns the row you want rendered at
that position.

Finally, but most importantly, notice that the `onTap()` function
doesn't recreate the list anymore, but instead `.add`s to it.

## Working with text

### How do I set custom fonts on my Text widgets?

In Android SDK (as of Android O), you create a Font resource file and
pass it into the FontFamily param for your TextView.

In Flutter, place the font file in a folder and reference it in the
`pubspec.yaml` file, similar to how you import images.

{% prettify yaml %}
fonts:
   - family: MyCustomFont
     fonts:
       - asset: fonts/MyCustomFont.ttf
       - style: italic
{% endprettify %}

Then assign the font to your `Text` widget:

{% prettify dart %}
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text("Sample App"),
    ),
    body: Center(
      child: Text(
        'This is a custom font text',
        style: TextStyle(fontFamily: 'MyCustomFont'),
      ),
    ),
  );
}
{% endprettify %}

### How do I style my Text widgets?

Along with fonts, you can customize other styling elements on a `Text` widget.
The style parameter of a `Text` widget takes a `TextStyle` object, where you can
customize many parameters, such as:

* color
* decoration
* decorationColor
* decorationStyle
* fontFamily
* fontSize
* fontStyle
* fontWeight
* hashCode
* height
* inherit
* letterSpacing
* textBaseline
* wordSpacing

## Form input

For more information on using Forms, see
[Retrieve the value of a text field](/docs/cookbook/forms/retrieve-input),
from the [Flutter Cookbook](/docs/cookbook).

### What is the equivalent of a "hint" on an Input?

In Flutter, you can easily show a "hint" or a placeholder text for your input by
adding an InputDecoration object to the decoration constructor parameter for
the Text Widget.

{% prettify dart %}
body: Center(
  child: TextField(
    decoration: InputDecoration(hintText: "This is a hint"),
  )
)
{% endprettify %}

### How do I show validation errors?

Just as you would with a "hint", pass an InputDecoration object
to the decoration constructor for the Text widget.

However, you don't want to start off by showing an error.
Instead, when the user has entered invalid data,
update the state, and pass a new `InputDecoration` object.

{% prettify dart %}
import 'package:flutter/material.dart';

void main() {
  runApp(SampleApp());
}

class SampleApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  String _errorText;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Sample App"),
      ),
      body: Center(
        child: TextField(
          onSubmitted: (String text) {
            setState(() {
              if (!isEmail(text)) {
                _errorText = 'Error: This is not an email';
              } else {
                _errorText = null;
              }
            });
          },
          decoration: InputDecoration(hintText: "This is a hint", errorText: _getErrorText()),
        ),
      ),
    );
  }

  _getErrorText() {
    return _errorText;
  }

  bool isEmail(String em) {
    String emailRegexp =
        r'^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$';

    RegExp regExp = RegExp(emailRegexp);

    return regExp.hasMatch(em);
  }
}
{% endprettify %}


## Flutter plugins

### How do I access the GPS sensor?

Use the [`geolocator`]({{site.pub}}/packages/geolocator) community plugin.

### How do I access the camera?

The [`image_picker`]({{site.pub}}/packages/image_picker) plugin is popular
for accessing the camera.

### How do I log in with Facebook?

To Log in with Facebook, use the
[`flutter_facebook_login`]({{site.pub}}/packages/flutter_facebook_login) community plugin.

### How do I use Firebase features?

Most Firebase functions are covered by
[first party plugins]({{site.pub}}/flutter/packages?q=firebase).
These plugins are first-party integrations, maintained by the Flutter team:

 * [`firebase_admob`]({{site.pub}}/packages/firebase_admob) for Firebase AdMob
 * [`firebase_analytics`]({{site.pub}}/packages/firebase_analytics) for Firebase Analytics
 * [`firebase_auth`]({{site.pub}}/packages/firebase_auth) for Firebase Auth
 * [`firebase_database`]({{site.pub}}/packages/firebase_database) for Firebase RTDB
 * [`firebase_storage`]({{site.pub}}/packages/firebase_storage) for Firebase Cloud Storage
 * [`firebase_messaging`]({{site.pub}}/packages/firebase_messaging) for Firebase Messaging (FCM)
 * [`flutter_firebase_ui`]({{site.pub}}/packages/flutter_firebase_ui) for quick Firebase Auth integrations (Facebook, Google, Twitter and email)
 * [`cloud_firestore`]({{site.pub}}/packages/cloud_firestore) for Firebase Cloud Firestore

You can also find some third-party Firebase plugins on Pub that cover areas
not directly covered by the first-party plugins.

### How do I build my own custom native integrations?

If there is platform-specific functionality that Flutter or its community
Plugins are missing, you can build your own following the
[developing packages and plugins](/docs/development/packages-and-plugins/developing-packages) page.

Flutter's plugin architecture, in a nutshell, is much like using an Event bus in
Android: you fire off a message and let the receiver process and emit a result
back to you. In this case, the receiver is code running on the native side
on Android or iOS.

### How do I use the NDK in my Flutter application?

If you use the NDK in your current Android application and want your Flutter
application to take advantage of your native libraries then it's possible by
building a custom plugin.

Your custom plugin first talks to your Android app, where you call your
`native` functions over JNI. Once a response is ready,
send a message back to Flutter and render the result.

Calling native code directly from Flutter is currently not supported.

## Themes

### How do I theme my app?

Out of the box, Flutter comes with a beautiful implementation of Material
Design, which takes care of a lot of styling and theming needs that you would
typically do. Unlike Android where you declare themes in XML and then assign it
to your application using AndroidManifest.xml, in Flutter you declare themes
in the top level widget.

To take full advantage of Material Components in your app, you can declare a top
level widget `MaterialApp` as the entry point to your application. MaterialApp
is a convenience widget that wraps a number of widgets that are commonly
required for applications implementing Material Design. It builds upon a WidgetsApp by
adding Material specific functionality.

You can also use a `WidgetApp` as your app widget, which provides some of the
same functionality, but is not as rich as `MaterialApp`.

To customize the colors and styles of any child components, pass a
`ThemeData` object to the `MaterialApp` widget. For example, in the code below,
the primary swatch is set to blue and text selection color is red.

{% prettify dart %}
class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        textSelectionColor: Colors.red
      ),
      home: SampleAppPage(),
    );
  }
}
{% endprettify %}


## Databases and local storage

### How do I access Shared Preferences?

In Android, you can store a small collection of key-value pairs using
the SharedPreferences API.

In Flutter, access this functionality using the
[Shared_Preferences plugin]({{site.pub}}/packages/shared_preferences).
This plugin wraps the functionality of both Shared Preferences and
NSUserDefaults (the iOS equivalent).

{% prettify dart %}
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

void main() {
  runApp(
    MaterialApp(
      home: Scaffold(
        body: Center(
          child: RaisedButton(
            onPressed: _incrementCounter,
            child: Text('Increment Counter'),
          ),
        ),
      ),
    ),
  );
}

_incrementCounter() async {
  SharedPreferences prefs = await SharedPreferences.getInstance();
  int counter = (prefs.getInt('counter') ?? 0) + 1;
  print('Pressed $counter times.');
  prefs.setInt('counter', counter);
}

{% endprettify %}

### How do I access SQLite in Flutter?

In Android, you use SQLite to store structured data that you can query
using SQL.

In Flutter, access this functionality using the
[SQFlite]({{site.pub}}/packages/sqflite) plugin.

## Notifications

### How do I set up push notifications?

In Android, you use Firebase Cloud Messaging to setup push
notifications for your app.

In Flutter, access this functionality using the
[Firebase_Messaging]({{site.github}}/flutter/plugins/tree/master/packages/firebase_messaging)
plugin.
For more information on using the Firebase Cloud Messaging API, see the
[`firebase_messaging`]({{site.pub}}/packages/firebase_messaging)
plugin documentation.
