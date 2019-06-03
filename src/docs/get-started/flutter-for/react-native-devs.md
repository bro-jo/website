---
title: React Native 개발자를 위한 Flutter
description: React Native 개발자가 Flutter 앱을 만들 때 기존 지식을 활용하는 방법을 배워봅시다.
---

이 문서는기존 React Native 지식을 활용하여
Flutter로 모바일 앱을 만들려는 React Native(RN) 개발자를 위한 문서입니다.
RN 프레임워크의 핵심을 이해하고 있다면
이 문서를 Flutter 개발을 시작하는 방안으로 사용하실 수 있습니다.

필요한 부분에 가장 적합한 질문을 찾아내는 방식으로 이 문서를 
요리책(cookbook)처럼 활용하실 수도 있습니다.


## 자바스크립트 개발자를 위한 Dart 소개

React Native처럼, Flutter는 reactive-style 뷰를 사용합니다.
하지만, 네이티브 위젯으로 변환(transpile)하는 RN과 달리, 
Flutter는 모든 것을 네이티브 코드로 직접 컴파일합니다.
Flutter는 화면의 각 픽셀을 직접 제어하기 때문에, 자바스크립트 브리지로 인한 성능 문제가 발생하지 않습니다. 

Dart는 아래와 같은 기능을 제공하는 배우기 쉬운 언어입니다:

* 웹, 서버, 모바일 앱 개발이 가능한 확장성 있는 
  오픈 소스 프로그래밍 언어 제공
* AOT를 통해 네이티브로 컴파일되는 C 방식의 구문을 사용하는 
  객체 지향 단일 상속 언어를 제공 
* 자바스크립트로 변환(transcompile) 가능
* 인터페이스와 추상 클래스 지원

아래에 자바스크립트와 Dart의 몇 가지 차이점에 관한 
예시가 적혀있습니다.

### 진입점

자바스크립트에는 진입점 역할을 하는 함수가 미리 정의되어 있지 않습니다.

```js
// JavaScript
function startHere() {
  // Can be used as entry point
}
```
Dart에서는 최상단에 앱의 진입점인 `main()` 함수가 
반드시 있어야 합니다.

<!-- skip -->
```dart
// Dart
main() {
}
```

[DartPad]({{site.dartpad}}/0df636e00f348bdec2bc1c8ebc7daeb1)에서 실행해보세요.

### 콘솔에 출력하기

`print()`를 사용하여 Dart 콘솔에 출력해보세요.

```js
// JavaScript
console.log("Hello world!");
```

<!-- skip -->
```dart
// Dart
print('Hello world!');
```

[DartPad]({{site.dartpad}}/cf9e652f77636224d3e37d96dcf238e5)에서 
실행해보세요.

### 변수

Dart는 타입 검사를 하는(type safe) 언어입니다. 
Dart는 정적 타입 검사와 런타임 타입 검사를 동시에 사용하여, 
변수의 값이 변수의 정적 타입과 항상 일치하는지 검사합니다.
타입이 필수이긴 하지만, Dart가 타입 추론을 하기 때문에 일부 타입 표기는 생략해도 됩니다.

#### 변수 생성 및 할당

자바스크립트에서는, 변수의 타입을 선언할 수 없습니다.

[Dart]({{site.dart-site}}/dart-2)에서는, 명시적으로 변수의 타입을 선언하거나   
타입 시스템이 자동으로 적절한 변수의 타입을 추론할 수 있어야 합니다.

```js
// JavaScript
var name = "JavaScript";
```

<!-- skip -->
```dart
// Dart
String name = 'dart'; // 명시적으로 문자열 타입 선언.
var otherName = 'Dart'; // 문자열 타입으로 추론.
// Dart에서는 두 방법 다 가능.
```

[DartPad]({{site.dartpad}}/3f4625c16e05eec396d6046883739612)에서 시도해보세요.

더 많은 정보를 원하시면, [Dart의 타입 시스템]({{site.dart-site}}/guides/language/sound-dart)를
참조하세요.

#### 기본값

자바스크립트에서 초기화하지 않은 변수는 `undefined`입니다.

Dart에서 초기화하지 않은 변수는 `null` 값을 가집니다.
Dart에선 숫자도 객체이기 때문에, 
초기화하지 않은 숫자 타입 변수도 `null` 값을 가집니다. 

```js
// JavaScript
var name; // == undefined
```

<!-- skip -->
```dart
// Dart
var name; // == null
int x; // == null
```

[DartPad]({{site.dartpad}}/57ec21faa8b6fe2326ffd74e9781a2c7)에서 
시도해보세요.

더 많은 정보를 원하시면, 
[변수]({{site.dart-site}}/guides/language/language-tour#variables)를
참조하세요.

### null 또는 0 체크

자바스크립트는 1이나 null이 아닌 객체도 true로 취급합니다.

```js
// JavaScript
var myNull = null;
if (!myNull) {
  console.log("null is treated as false");
}
var zero = 0;
if (!zero) {
  console.log("0 is treated as false");
}
```
Dart는 boolean 값 `true`만 true로 취급합니다.

<!-- skip -->
```dart
// Dart
var myNull = null;
if (myNull == null) {
  print('use "== null" to check null');
}
var zero = 0;
if (zero == 0) {
  print('use "== 0" to check zero');
}
```

[DartPad]({{site.dartpad}}/c85038ad677963cb6dc943eb1a0b72e6)에서 
시도해보세요.

### 함수

Dart와 자바스크립트 함수는 대체로 유사합니다. 
주된 차이점은 선언입니다.

```js
// JavaScript
function fn() {
  return true;
}
```

<!-- skip -->
```dart
// Dart
fn() {
  return true;
}
// can also be written as
bool fn() {
  return true;
}
```

[DartPad]({{site.dartpad}}/5454e8bfadf3000179d19b9bc6be9918)에서 
시도해보세요.

더 많은 정보를 원하시면, 
[함수]({{site.dart-site}}/guides/language/language-tour#functions)를 참조하세요.

### 비동기 프로그래밍

#### Futures

자바스크립트와 마찬가지로 Dart도 단일 스레드 실행을 지원합니다. 
자바스크립트에서 Promise 객체는 
비동기 작업의 최종 완료 (또는 실패)와 결과 값을 나타냅니다.

Dart는 이러한 비동기 처리를 위해서 
[`Future`]({{site.dart-site}}/tutorials/language/futures) 객체를 사용합니다.

```js
// JavaScript
_getIPAddress = () => {
  const url="https://httpbin.org/ip";
  return fetch(url)
    .then(response => response.json())
    .then(responseJson => {
      console.log(responseJson.origin);
    })
    .catch(error => {
      console.error(error);
    });
};
```

<!-- skip -->
```dart
// Dart
_getIPAddress() {
  final url = 'https://httpbin.org/ip';
  HttpRequest.request(url).then((value) {
      print(json.decode(value.responseText)['origin']);
  }).catchError((error) => print(error));
}
```

[DartPad]({{site.dartpad}}/5a0017d09b6823d0248d965b93133e2e)에서 
시도해보세요.

더 많은 정보를 원하시면, 
[Futures]({{site.dart-site}}/tutorials/language/futures)를 참조하세요.

#### `async`와 `await`

`async` 함수 선언으로 비동기 함수를 정의합니다.

자바스크립트에서는 `async` 함수가 `Promise`를 반환합니다.
`await` 연산자는 `Promise`를 기다리기 위해서 사용합니다다.

```js
// JavaScript
async _getIPAddress() {
  const url="https://httpbin.org/ip";
  const response = await fetch(url);
  const json = await response.json();
  const data = await json.origin;
  console.log(data);
}
```

Dart에서는 `async` 함수가 `Future`를 반환하고, 
함수의 구현된 내용은 나중에 실행되도록 예약됩니다.
`await` 연산자는 `Future`를 기다리기 위해서 사용됩니다.

<!-- skip -->
```dart
// Dart
_getIPAddress() async {
  final url = 'https://httpbin.org/ip';
  var request = await HttpRequest.request(url);
  String ip = json.decode(request.responseText)['origin'];
  print(ip);
}
```

[DartPad]({{site.dartpad}}/04bb4334985107cddcd021322398c918)에서 
시도해보세요.

더 많은 정보를 원하시면, [`async`와
`await`]({{site.dart-site}}/guides/language/language-tour#asynchrony-support)를 참조하세요.

## 기본
### Flutter 앱을 만드는 방법은?

React Native로 앱을 만드려면 커멘드라인에서 
`create-react-native-app`을 실행하면 됩니다.

{% prettify %}
$ create-react-native-app <projectname>
{% endprettify%}

Flutter에서 앱을 만들기 위해서는, 아래 방법 중 하나를 수행하면 됩니다:

* 커멘드라인에서 `flutter create` 명령을 실행하세요.
  Flutter SDK가 PATH에 들어있는지 확인이 필요합니다. 
* Flutter와 Dart 플러그인이 설치된 IDE를 이용하세요.

{% prettify %}
$ flutter create <projectname>
{% endprettify%}

더 많은 정보를 원하시면, [시작하기](/docs/get-started)를 참조하세요. 
버튼 클릭으로 카운터를 조정하는 앱을 만드는 과정을 안내 받을 수 있습니다.
Flutter 프로젝트를 만들 때 
Android 및 iOS 기기에서 샘플 앱을 실행하는 데 필요한 모든 파일이 생성됩니다.

### 앱을 실행하는 방법은?

React Native를 사용할 때는, 프로젝트 디렉토리에서 
`npm run`이나 `yarn run`으로 앱을 실행했을 것입니다.

 Flutter apps를 실행하는 몇가지 방법이 있습니다:

 * 프로젝트 최상위 디렉토리에서 `flutter run`을 사용하세요.
 * Flutter와 Dart 플러그인이 설치된 IDE에서 "run"을 실행하세요. 

 앱이 연결된 기기나, iOS 시뮬레이터 혹은 Android 에뮬레이터에서 실행될 것입니다.

더 많은 정보를 원하시면, [시작하기](/docs/get-started)를 참조하세요.

### 위젯을 import 하는 방법은?

React Native에서는 필요한 모든 컴포넌트를 각각 import 해야 합니다.

```js
//React Native
import React from "react";
import { StyleSheet, Text, View } from "react-native";
```

Flutter에서는 머티리얼 디자인 라이브러리에서 위젯을 사용하기 위해 `material.dart` 패키지를 import 합니다. iOS 스타일 위젯을 사용하기 위해 쿠퍼티노 라이브러리를 import 합니다. 더 많은 기본 위젯을 사용하고 싶다면, 위젯 라이브러리를 import 합니다. 아니면 직접 위젯 라이브러리를 작성하여 import 할 수도 있습니다.

<!-- skip -->
```dart
import 'package:flutter/material.dart';
import 'package:flutter/cupertino.dart';
import 'package:flutter/widgets.dart';
import 'package:flutter/my_widgets.dart';
```
어떤 위젯 패키지를 import 하건, 
Dart는 앱에서 사용되는 위젯만 가져옵니다.

더 많은 정보를 원하시면, [Flutter 위젯 카탈로그](/docs/development/ui/widgets)를 참조하세요.

### Flutter에서 React Native "Hello world!" 앱과 동일한 것은?

React Native에서는 `HelloWorldApp`이 `React.Component`를 상속 받고 
render 메서드가 view 컴포넌트를 반환하도록 구현합니다.

```js
// React Native
import React from "react";
import { StyleSheet, Text, View } from "react-native";

export default class App extends React.Component {
  render() {
    return (
      <View style={styles.container}>
        <Text>Hello world!</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#fff",
    alignItems: "center",
    justifyContent: "center"
  }
});
```

Flutter에서는, 기본 위젯 라이브러리의 `Center`와 `Text` 위젯을 활용하여 "Hello world!" 앱과 동일한 것을 만들 수 있습니다. `Center` 위젯을 최상위 위젯으로 하고, 자식으로 `Text` 위젯을 넣으면 됩니다. 

<!-- skip -->
```dart
// Flutter
import 'package:flutter/material.dart';

void main() {
  runApp(
    Center(
      child: Text(
        'Hello, world!',
        textDirection: TextDirection.ltr,
      ),
    ),
  );
}

```

아래 이미지는 기본 Flutter "Hello world!" 앱의 
Android와 iOS 화면을 보여줍니다. 

{% include android-ios-figure-pair.md image="react-native/hello-world-basic.png" alt="Hello world app" class="border" %}

이제 가장 기본적인 Flutter 앱을 봤고, 
다음 섹션에서는 Flutter의 풍부한 위젯 라이브러리를 활용하여 
현대적이고 세련된 앱을 만드는 방법을 확인할 수 있습니다.  

### 위젯을 사용하고 위젯 트리를 형성하는 방법은?

Flutter에서는 거의 모든 것이 위젯입니다.

위젯은 앱 UI의 기본 구성 요소 입니다.
위젯을 위젯 트리라고 불리는 계층 구조로 조합합니다. 
각각의 위젯은 부모 위젯 내부에 들어가게 되고, 부모로부터 속성을 상속 받습니다.
앱 객체 자체도 위젯입니다.
별도의 “application” 객체는 없습니다.
대신, 최상위 위젯이 그 역할을 담당합니다. 

위젯을 아래와 같이 정의할 수 있습니다:

* 버튼이나 메뉴와 같은 구조적 요소
* 글꼴이나 색 구성표와 같은 문체 요소
* 레이아웃과 같은 패딩 또는 정렬의 한 측면

아래 예제는 머티리얼 라이브러리의 위젯을 활용한 "Hello world!" 앱입니다.
이 예제에서 위젯 트리는 `MaterialApp` 위젯 내부에 위치하게 됩니다. 


<!-- skip -->
```dart
// Flutter
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(
          child: Text('Hello world'),
        ),
      ),
    );
  }
}

```


아래 이미지는 머티리얼 디자인 위젯으로 만든 "Hello world!"입니다. 기본적인 "Hello world!"보다 좀 더 많은 기능을 자유롭게 이용할 수 있습니다.

{% include android-ios-figure-pair.md image="react-native/hello-world.png" alt="Hello world app" %}

앱을 작성할 때, 2가지 종류의 위젯을 사용하게 될겁니다:
[StatelessWidget]({{site.api}}/flutter/widgets/StatelessWidget-class.html) 또는
 [StatefulWidget]({{site.api}}/flutter/widgets/StatefulWidget-class.html).
StatelessWidget은 위젯인데 상태가 없는 위젯입니다. 
StatelessWidget은 한 번 만들어지고 절대 변하지 않습니다. 
StatefulWidget은 사용자 입력이나 
데이터 수신이 있으면 상태를 동적으로 변경합니다.

상태가 없는 위젯과 상태가 있는 위젯의 중요한 차이점은 
StatefulWidgets이 상태 데이터를 저장하고 그것을 트리 재구성을 통해 전달하는 State 객체가 있고,
그렇기 때문에 손실되지 않는다는 점입니다. 

간단하거나 기본적인 앱에서 위젯을 감싸는 것은 쉽지만,
코드 베이스가 점차 커지고 앱이 복잡해지면, 
너무 많이 감싸져 있는 위젯을 함수로 빼거나 작은 클래스로 분리해야 합니다.
분리된 함수를 만들면 위젯을 앱 내에서 재사용할 수 있게 됩니다. 

### 재사용 가능한 컴포넌트를 만드는 방법은?

React Native에서는 재사용 가능한 컴포넌트를 만들기 위해 클래스를 정의한 다음 
`props` 메서드를 사용하여 선택한 요소의 속성과 값을 설정하거나 반환합니다. 
아래 예제에서 `CustomCard` 클래스를 정의하고, 부모 클래스 안에서 사용합니다. 

```js
// React Native
class CustomCard extends React.Component {
  render() {
    return (
      <View>
        <Text> Card {this.props.index} </Text>
        <Button
          title="Press"
          onPress={() => this.props.onPress(this.props.index)}
        />
      </View>
    );
  }
}

// Usage
<CustomCard onPress={this.onPress} index={item.key} />
```

Flutter에서는 커스텀 위젯을 만들기 위해 클래스를 정의하면 그 위젯을 재사용할 수 있습니다.
또는 아래 예제와 같은 `build` 함수에서 
재사용 가능한 위젯을 정의하고 호출할 수 있습니다.

{% prettify dart %}

// Flutter
class CustomCard extends StatelessWidget {
  [[highlight]]CustomCard({@required this.index, @required [[/highlight]]
     [[highlight]]this.onPress});[[/highlight]]

  final index;
  final Function onPress;

  @override
  Widget build(BuildContext context) {
    return Card(
      child: Column(
        children: <Widget>[
          Text('Card $index'),
          FlatButton(
            child: const Text('Press'),
            onPressed: this.onPress,
          ),
        ],
      )
    );
  }
}
    ...
// Usage
CustomCard(
  [[highlight]]index: index,[[/highlight]]
  [[highlight]]onPress: () { [[/highlight]]
    print('Card $index');
  },
)
    ...

{% endprettify %}

이전 예제에서는 `CustomCard` 클래스의 생성자가
[optional parameters]({{site.dart-site}}/guides/language/language-tour#optional-parameters)를
표현하기 위해 Dart의 중괄호 구문 `{ }`을 사용했습니다.

이 필드를 필수로 만들고 싶다면, 
생성자에서 중괄호를 지우거나 `@required`를 추가하면 됩니다.  


다음 스크린샷은 재사용할 수 있는 CustomCard 클래스의 예를 보여줍니다.

{% include android-ios-figure-pair.md image="react-native/custom-cards.png" alt="Custom cards" class="border" %}


## 프로젝트 구조 및 리소스

### 어디서부터 코드 작성을 시작해야 하나요?

`main.dart` 파일에서 시작하세요.
Flutter 앱을 만들면 `main.dart`가 자동 생성됩니다.  

<!-- skip -->
```dart
// Dart
void main(){
 print("Hello, this is the main function.");
}
```

Flutter에서 진입점은 `’projectname’/lib/main.dart` 파일이고, 
`main` 함수부터 실행을 시작합니다.  

### Flutter 앱에서 파일은 어떻게 구성되어 있습니까?

Flutter 프로젝트를 새롭게 만들면, 아래와 같은 디렉토리 구조가 생성됩니다.
나중에 구조를 변경할 수는 있지만, 일단은 아래와 같은 상태에서 시작합니다. 

```
┬
└ projectname
  ┬
  ├ android      - 안드로이드 관련 파일 포함.
  ├ build        - iOS 및 Android 빌드 파일 저장.
  ├ ios          - iOS 관련 파일 포함.
  ├ lib          - 외부에서 접근할 수 있는 Dart 소스 파일 포함.
    ┬
    └ src        - 추가적인 소스 파일 포함.
    └ main.dart  - Flutter 진입점이며 새로운 앱의 시작.
                   Flutter 프로젝트를 만들 때 자동으로 생성.
                   Dart코드 작성을 시작하는 부분임.
  ├ test         - 자동화 테스트 파일 포함.
  └ pubspec.yaml - Flutter 앱의 메타데이터 포함.
                   React Native의 package.json 파일과 동일함.
```


### 리소스와 asset은 어디에 위치 시키고, 어떻게 사용하나요?

Flutter 리소스 또는 asset은 앱과 함께 묶여서 
배포되며 런타임에 접근할 수 있는 파일입니다. 
Flutter 앱은 아래와 같은 asset 유형을 포함할 수 있습니다:
* JSON 파일 같은 정적 데이터
* 설정 파일
* 아이콘과 이미지 (JPEG, PNG, GIF, Animated GIF, WebP, Animated WebP, BMP,
  and WBMP)

Flutter는 프로젝트 최상위에 위치한 
`pubspec.yaml` 파일을 사용하여 앱에 필요한 assets을 식별합니다.

```yaml
flutter:
  assets:
    - assets/my_icon.png
    - assets/background.png
```

`assets` 부분은 앱에 포함시켜야하는 파일을 지정합니다.
각 asset은 `pubspec.yaml`에 명시된 상대 경로에 의해서
 asset 파일이 어디에 있는지 구분됩니다.
asset이 정의된 순서는 중요하지 않습니다.
실제 디렉토리 이름(위 경우에는 `assets`)도 중요하지 않습니다. 
어떤 디렉토리에도 asset을 배치할 수는 있지만, `assets` 디렉토리에 배치하기를 권합니다.

빌드 중, Flutter는 런타임 때에 앱이 읽어올 *asset bundle*이라 불리는 
특수한 아카이브에 asset을 저장합니다.
`pubspec.yaml`에 asset의 경로가 지정되면, 빌드 프로세스는 인접한 디렉토리에서 같은 이름의 파일을 찾습니다. 
이 파일들은 지정된 asset과 함께 asset bundle에도 포함됩니다. 
Flutter는 앱에 적합한 해상도의 이미지를 선택할 때 
asset variants를 사용합니다.

React Native에서는 이미지 파일을 소스 코드 디렉토리에 놓은 뒤 
경로를 지정하여 정적 이미지를 추가할 수 있습니다.

```js
<Image source={require("./my-icon.png")} />
```

Flutter에서는 위젯의 build 메서드 안에서 `AssetImage` 클래스를 사용하여 
정적 이미지를 추가할 수 있습니다.

<!-- skip -->
```dart
image: AssetImage('assets/background.png'),
```

더 많은 정보를 원하시면 [Flutter에서 
Asset과 이미지 파일 추가하기](/docs/development/ui/assets-and-images)를 참조하세요.

### 네트워크를 통해 이미지를 가져오는 방법은?

React Native에서는 `Image` 컴포넌트의 `source` 속성에서 `uri`을 지정하고, 
필요하다면 크기도 지정합니다.

Flutter에서는 `Image.network` 생성자를 사용하여 해당 URL에서 이미지를 가져옵니다.

<!-- skip -->
```dart
// Flutter
body: Image.network(
          'https://flutter-io.kr/images/owl.jpg',
```

### 패키지와 패키지 플러그인을 설치하는 방법은?

Flutter에서 Flutter와 Dart 생태계의 
다른 개발자들이 만든 공유 패키지를 사용할 수 있습니다.
이를 통해 일일이 모든 것을 개발할 필요없이 빠르게 앱을 개발할 수 있습니다.
패키지 플러그인은 플랫폼별 코드가 포함된 패키지입니다.

React Native에서는 커멘드라인 `yarn add {package-name}`나 
`npm install --save {package-name}` 명령을 사용해서 패키지를 설치합니다. 

Flutter에서는 아래와 같은 방법으로 패키지를 설치합니다:

1. `pubspec.yaml`의 dependencies 부분에 패키지 이름과 버전을 추가합니다. 
아래 예제는 `pubspec.yaml` 파일에 `google_sign_in` Dart 패키지를 
추가하는 방법을 보여줍니다. 
**공백이 문제가 될 수 있기 때문에** YAML file의 공백을 잘 확인하세요! 

```yaml
dependencies:
  flutter:
    sdk: flutter
  google_sign_in: ^3.0.3
```

2. 커멘드라인에서 `flutter pub get`을 사용해서 패키지를 설치하세요.
   IDE를 이용하고 있으면, IDE가 종종 `flutter pub get`을 실행해주거나, 
   실행하도록 유도할 것입니다.
3. 아래 코드와 같은 패키지를 import 하세요:

<!-- skip -->
```dart
import 'package:flutter/cupertino.dart';
```

더 많은 정보를 원하시면, 
[패키지 사용하기](/docs/development/packages-and-plugins/using-packages)와
[패키지 & 플러그인 개발](/docs/development/packages-and-plugins/developing-packages)를
참조하세요.

[Pub site]({{site.pub}})의 
[Flutter Packages]({{site.pub}}/flutter/) 섹션에서
Flutter 개발자들이 공유한 많은 패키지를 만날 수 있습니다.

## Flutter 위젯

Flutter에서는 UI를 만들 때 현재 상태와 설정에 따라 어떻게 보일지에 대한 
정보를 담고 있는 위젯을 사용합니다.

위젯은 종종 강력한 효과를 내기 위해 
다수의 중첩된 작은 단일 목적의 위젯으로 구성됩니다.
예를 들어, Container 위젯은 레이아웃, 그리기, 위치, 크기를 담당하는 여러 위젯으로 구성됩니다.
구체적으로 말하자면, `Container` 위젯은 `LimitedBox`와 `ConstrainedBox`, 
`Align`, `Padding`, `DecoratedBox`, `Transform` 위젯을 포함하고 있습니다.
원하는 효과를 내기 위해 `Container` 를 서브클래스로 만드는 대신, 
여러 심플한 위젯들을 새롭고 독특한 방식으로 구성할 수 있습니다.

`Center` 위젯은 레이아웃을 어떻게 조절할 수 있는지를 보여주는 또 다른 예입니다.
위젯을 가운데 정렬 하기 위해, `Center` 위젯으로 감싼 다음
정렬, 행, 그리드를 배치하는 위젯을 사용하세요.
이러한 배치 위젯은 시각적으로 자기 자신을 나타내지는 않습니다.
대신, 그것들의 유일한 목적은 다른 위젯 레이아웃을 제어하는 것입니다.
왜 위젯이 이런 방식으로 그려지는지 이해하기 위해,
인접한 위젯을 조사하는 것이 종종 도움이 됩니다.

더 많은 정보는
[Flutter 기술 개요](/docs/resources/technical-overview)를
참조하세요.

위젯 패키지의 핵심 위젯에 대한 자세한 정보는, 
[Flutter 기본 위젯](/docs/development/ui/widgets/basics), 
[Flutter 위젯 카탈로그](/docs/development/ui/widgets), 
[Flutter 위젯 색인](/docs/reference/widgets)을 
참조하세요.

## Views

### `View` 컨테이너와 동일한 것은?

React Native에서는 `View`가 컨테이너이고, `Flexbox`를 이용한 레이아웃, 
스타일, 터치 핸들링, 접근성 제어를 지원합니다.

Flutter에서는 
[Container]({{site.api}}/flutter/widgets/Container-class.html)나
[Column]({{site.api}}/flutter/widgets/Column-class.html),
[Row]({{site.api}}/flutter/widgets/Row-class.html),
[Center]({{site.api}}/flutter/widgets/Center-class.html) 같은
위젯 라이브러리의 핵심 레이아웃 위젯을 사용할 수 있습니다.

더 많은 정보를 원하시면, [레이아웃 위젯](/docs/development/ui/widgets/layout) 카탈로그를 참조하세요.

### `FlatList`나 `SectionList`와 동일한 것은?

`List`는 수직으로 배열된 구성 요소의 스크롤 가능한 목록입니다.

React Native에서는 `FlatList` 혹은 `SectionList`를 사용하여 
단순 혹은 섹션 목록을 그립니다.

```js
// React Native
<FlatList
  data={[ ... ]}
  renderItem={({ item }) => <Text>{item.key}</Text>}
/>
```

[`ListView`]({{site.api}}/flutter/widgets/ListView-class.html)는 
가장 많이 사용되는 스크롤 위젯입니다.
`ListView`는 기본 생성자를 통해 명시적으로 자식 목록을 받습니다. 
[`ListView`]({{site.api}}/flutter/widgets/ListView-class.html)는
목록의 수가 적은 경우에 가장 적합합니다.
큰 목록 혹은 무한스크롤 목록을 위해서는, `ListView.builder`를 사용하세요. 
자식들을 필요할 때만 빌드하고, 자식들이 화면에 나타내야 할 자식들만 빌드합니다. 


<!-- skip -->
```dart
// Flutter
var data = [ ... ];
ListView.builder(
  itemCount: data.length,
  itemBuilder: (context, int index) {
    return Text(
      data[index],
    );
  },
)
```

{% include android-ios-figure-pair.md image="react-native/flatlist.gif" alt="Flat list" class="border" %}

무한 스크롤을 만드는 방법을 배우고 싶다면, 
[첫 Flutter 앱 만들기, part 1]({{site.codelabs}}/codelabs/first-flutter-app-pt1) 코드랩을
참조하세요.

### Canvas를 사용하여 그리거나 색을 입히는 방법은?

React Native에서는 캔버스 컴포넌트가 없기 때문에, `react-native-canvas` 같은 서드 파티 라이브러리를 사용합니다. 

```js
// React Native
handleCanvas = canvas => {
  const ctx = canvas.getContext("2d");
  ctx.fillStyle = "skyblue";
  ctx.beginPath();
  ctx.arc(75, 75, 50, 0, 2 * Math.PI);
  ctx.fillRect(150, 100, 300, 300);
  ctx.stroke();
};

render() {
  return (
    <View>
      <Canvas ref={this.handleCanvas} />
    </View>
  );
}
```
Flutter에서는 
[`CustomPaint`]({{site.api}}/flutter/widgets/CustomPaint-class.html)와
[`CustomPainter`]({{site.api}}/flutter/rendering/CustomPainter-class.html) 클래스를
사용하여 캔버스에 그릴 수 있습니다.

아래 예시는 `CustomPaint` 위젯을 사용해서 페인트 단계에서 그리는 방법을 보여줍니다.
추상 클래스인 CustomPainter를 구현하고, 
이를 CustomPaint의 painter 속성에 전달합니다.
CustomPaint 서브 클래스는 `paint`와 `shouldRepaint`를 꼭 구현해야 합니다.

<!-- skip -->
```dart
// Flutter
class MyCanvasPainter extends CustomPainter {

  @override
  void paint(Canvas canvas, Size size) {
    Paint paint = Paint();
    paint.color = Colors.amber;
    canvas.drawCircle(Offset(100.0, 200.0), 40.0, paint);
    Paint paintRect = Paint();
    paintRect.color = Colors.lightBlue;
    Rect rect = Rect.fromPoints(Offset(150.0, 300.0), Offset(300.0, 400.0));
    canvas.drawRect(rect, paintRect);
  }

  bool shouldRepaint(MyCanvasPainter oldDelegate) => false;
  bool shouldRebuildSemantics(MyCanvasPainter oldDelegate) => false;
}
class _MyCanvasState extends State<MyCanvas> {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: CustomPaint(
        painter: MyCanvasPainter(),
      ),
    );
  }
}
```

{% include android-ios-figure-pair.md image="react-native/canvas.png" alt="Canvas" class="border" %}

## Layouts

### How do I use widgets to define layout properties?

In React Native, most of the layout can be done with the props that are passed
to a specific component. For example, you could use the `style` prop on the
`View` component in order to specify the flexbox properties. To arrange your
components in a column, you would specify a prop such as:
`flexDirection: “column”`.

```js
// React Native
<View
  style={%raw%}{{
    flex: 1,
    flexDirection: "column",
    justifyContent: "space-between",
    alignItems: "center"
  }}{%endraw%}
>
```

In Flutter, the layout is primarily defined by widgets specifically designed to
 provide layout, combined with control widgets and their style properties.

For example, the
[Column]({{site.api}}/flutter/widgets/Column-class.html) and
[Row]({{site.api}}/flutter/widgets/Row-class.html) widgets
take an array of children and align them vertically and horizontally respectively.
A [Container]({{site.api}}/flutter/widgets/Container-class.html)
widget takes a combination of layout and styling properties, and a
[`Center`]({{site.api}}/flutter/widgets/Center-class.html) widget centers
its child widgets.

<!-- skip -->
```dart
// Flutter
Center(
  child: Column(
    children: <Widget>[
      Container(
        color: Colors.red,
        width: 100.0,
        height: 100.0,
      ),
      Container(
        color: Colors.blue,
        width: 100.0,
        height: 100.0,
      ),
      Container(
        color: Colors.green,
        width: 100.0,
        height: 100.0,
      ),
    ],
  ),
)
```

Flutter provides a variety of layout widgets in its core widget library.
For example, [`Padding`]({{site.api}}/flutter/widgets/Padding-class.html),
[`Align`]({{site.api}}/flutter/widgets/Align-class.html),
and [`Stack`]({{site.api}}/flutter/widgets/Stack-class.html).

For a complete list, see [Layout Widgets](/docs/development/ui/widgets/layout).

{% include android-ios-figure-pair.md image="react-native/basic-layout.gif" alt="Layout" class="border" %}

### How do I layer widgets?

In React Native, components can be layered using `absolute` positioning.

Flutter uses the
[`Stack`]({{site.api}}/flutter/widgets/Stack-class.html)
widget to arrange children widgets in layers.
The widgets can entirely or partially overlap the base widget.

The `Stack` widget positions its children relative to the edges of its box.
This class is useful if you simply want to overlap several children widgets.

<!-- skip -->
```dart
// Flutter
Stack(
  alignment: const Alignment(0.6, 0.6),
  children: <Widget>[
    CircleAvatar(
      backgroundImage: NetworkImage(
        "https://avatars3.githubusercontent.com/u/14101776?v=4"),
    ),
    Container(
      decoration: BoxDecoration(
          color: Colors.black45,
      ),
      child: Text('Flutter'),
    ),
  ],
)
```

The previous example uses `Stack` to overlay a Container (that displays its `Text`
on a translucent black background) on top of a `CircleAvatar`. The Stack offsets
the text using the alignment property and Alignment coordinates.

{% include android-ios-figure-pair.md image="react-native/stack.png" alt="Stack" class="border" %}

For more information, see the
[Stack]({{site.api}}/flutter/widgets/Stack-class.html) class documentation.

## Styling

### How do I style my components?

In React Native, inline styling and `stylesheets.create` are used to style
components.

```js
// React Native
<View style={styles.container}>
  <Text style={%raw%}{{ fontSize: 32, color: "cyan", fontWeight: "600" }}{%endraw%}>
    This is a sample text
  </Text>
</View>

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#fff",
    alignItems: "center",
    justifyContent: "center"
  }
});
```

In Flutter, a `Text` widget can take a `TextStyle` class for its style property.
If you want to use the same text style in multiple places, you can create a
[`TextStyle`]({{site.api}}/flutter/dart-ui/TextStyle-class.html)
class and use it for multiple `Text` widgets.

<!-- skip -->
```dart
// Flutter
var textStyle = TextStyle(fontSize: 32.0, color: Colors.cyan, fontWeight:
   FontWeight.w600);
	...
Center(
  child: Column(
    children: <Widget>[
      Text(
        'Sample text',
        style: textStyle,
      ),
      Padding(
        padding: EdgeInsets.all(20.0),
        child: Icon(Icons.lightbulb_outline,
          size: 48.0, color: Colors.redAccent)
      ),
    ],
  ),
)
```

{% include android-ios-figure-pair.md image="react-native/flutterstyling.gif" alt="Styling" class="border" %}

### How do I use `Icons` and `Colors`?

React Native doesn't include support for icons so third party libraries are used.

In Flutter, importing the Material library also pulls in the rich set of
[Material icons]({{site.api}}/flutter/material/Icons-class.html)
and [colors]({{site.api}}/flutter/material/Colors-class.html).

<!-- skip -->
```dart
Icon(Icons.lightbulb_outline, color: Colors.redAccent)
```

When using the `Icons` class, make sure to set `uses-material-design: true` in
the project's `pubspec.yaml` file. This ensures that
the `MaterialIcons` font, which displays the icons, is included in your app.
{% prettify dart %}
name: my_awesome_application
flutter: [[highlight]]uses-material-design: true[[/highlight]]
{% endprettify %}

Flutter's [Cupertino (iOS-style)](/docs/development/ui/widgets/cupertino) package provides high
fidelity widgets for the current iOS design language. To use the `CupertinoIcons`
font, add a dependency for `cupertino_icons` in your project's  `pubspec.yaml` file.

```yaml
name: my_awesome_application
dependencies:
  cupertino_icons: ^0.1.0
```

To globally customize the colors and styles of components, use `ThemeData`
to specify default colors for various aspects of the theme. Set the theme
property in `MaterialApp` to the `ThemeData` object. The
[`Colors`]({{site.api}}/flutter/material/Colors-class.html)
class provides colors from the Material Design [color
palette]({{site.material}}/guidelines/style/color.html).

The following example sets the primary swatch to `blue` and the text
selection to `red`.

<!-- skip -->
{% prettify dart %}
class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        [[highlight]]primarySwatch: Colors.blue,[[/highlight]]
        [[highlight]]textSelectionColor: Colors.red[[/highlight]]
      ),
      home: SampleAppPage(),
    );
  }
}
{% endprettify %}

### How do I add style themes?

In React Native, common themes are defined for components in stylesheets and
then used in components.

In Flutter, create uniform styling for almost everything by defining the
 styling in the
[`ThemeData`]({{site.api}}/flutter/material/ThemeData-class.html)
class and passing it to the theme property in the
[`MaterialApp`]({{site.api}}/flutter/material/MaterialApp-class.html)
widget.

<!-- skip -->
```dart
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(
        primaryColor: Colors.cyan,
        brightness: Brightness.dark,
      ),
      home: StylingPage(),
    );
  }
```

A `Theme` can be applied even without using the `MaterialApp` widget. The
[`Theme`]({{site.api}}/flutter/material/Theme-class.html)
widget takes a `ThemeData` in its `data` parameter and applies the
`ThemeData` to all of its children widgets.

<!-- skip -->
```dart
 @override
  Widget build(BuildContext context) {
    return Theme(
      data: ThemeData(
        primaryColor: Colors.cyan,
        brightness: brightness,
      ),
      child: Scaffold(
         backgroundColor: Theme.of(context).primaryColor,
              ...
              ...
      ),
    );
  }
```

## State Management

State is information that can be read synchronously when a widget is built
or information that might change during the lifetime of a widget.
To manage app state in Flutter, use a
[StatefulWidget]({{site.api}}/flutter/widgets/StatefulWidget-class.html)
paired with a State object.

### The StatelessWidget

A `StatelessWidget` in Flutter is a widget that doesn't require a state
change&mdash;it has no internal state to manage.

Stateless widgets are useful when the part of the user interface you are
describing does not depend on anything other than the configuration information
in the object itself and the
[`BuildContext`]({{site.api}}/flutter/widgets/BuildContext-class.html)
in which the widget is inflated.

[AboutDialog]({{site.api}}/flutter/material/AboutDialog-class.html),
[CircleAvatar]({{site.api}}/flutter/material/CircleAvatar-class.html),
and [Text]({{site.api}}/flutter/widgets/Text-class.html) are examples
of stateless widgets which subclass
[StatelessWidget]({{site.api}}/flutter/widgets/StatelessWidget-class.html).


<!-- skip -->
```dart
// Flutter
import 'package:flutter/material.dart';

void main() => runApp(MyStatelessWidget(text: "StatelessWidget Example to show immutable data"));

class MyStatelessWidget extends StatelessWidget {
  final String text;
  MyStatelessWidget({Key key, this.text}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text(
        text,
        textDirection: TextDirection.ltr,
      ),
    );
  }
}
```

In the previous example, you used the constructor of the `MyStatelessWidget` class
to pass the `text`, which is marked as `final`. This class extends
 `StatelessWidget`&mdash;it contains immutable data.

The `build` method of a stateless widget is typically called in only three
 situations:

* When the widget is inserted into a tree
* When the widget's parent changes its configuration
* When an
  [`InheritedWidget`]({{site.api}}/flutter/widgets/InheritedWidget-class.html)
  it depends on, changes

### The StatefulWidget

A [StatefulWidget]({{site.api}}/flutter/widgets/StatefulWidget-class.html)
is a widget that changes state. Use the `setState` method to manage the
state changes for a `StatefulWidget`. A call to `setState` tells the Flutter
framework that something has changed in a state, which causes an app to
rerun the `build` method so that the app can reflect the change.

State is information that can be read synchronously when a widget is built and
might change during the lifetime of the widget. It's the responsibility of the
widget implementer to ensure that the state is promptly notified when the state
changes. Use `StatefulWidget` when a widget can change dynamically.
For example, the state of the widget changes by typing into a form,
or moving a slider. Or, it can change over time—perhaps a data feed updates the UI.

[Checkbox]({{site.api}}/flutter/material/Checkbox-class.html),
[Radio]({{site.api}}/flutter/material/Radio-class.html),
[Slider]({{site.api}}/flutter/material/Slider-class.html),
[InkWell]({{site.api}}/flutter/material/InkWell-class.html),
[Form]({{site.api}}/flutter/widgets/Form-class.html),
and [TextField]({{site.api}}/flutter/material/TextField-class.html)
are examples of stateful widgets, that subclass
[StatefulWidget]({{site.api}}/flutter/widgets/StatefulWidget-class.html).

The following example declares a `StatefulWidget` which requires a `createState()`
method. This method creates the state object that manages the widget's state,
`_MyStatefulWidgetState`.

<!-- skip -->
```dart
class MyStatefulWidget extends StatefulWidget {
  MyStatefulWidget({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _MyStatefulWidgetState createState() => _MyStatefulWidgetState();
}
```

The following state class, `_MyStatefulWidgetState`, implements the `build()`
method for the widget. When the state changes, for example, when the user toggles
the button, `setState` is called with the new toggle value. This causes the
framework to rebuild this widget in the UI.

<!-- skip -->
```dart
class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  bool showtext=true;
  bool toggleState=true;
  Timer t2;

  void toggleBlinkState(){
    setState((){
      toggleState=!toggleState;
    });
    var twenty = const Duration(milliseconds: 1000);
    if(toggleState==false) {
      t2 = Timer.periodic(twenty, (Timer t) {
        toggleShowText();
      });
    } else {
      t2.cancel();
    }
  }

  void toggleShowText(){
    setState((){
      showtext=!showtext;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          children: <Widget>[
            (showtext
              ?(Text('This execution will be done before you can blink.'))
              :(Container())
            ),
            Padding(
              padding: EdgeInsets.only(top: 70.0),
              child: RaisedButton(
                onPressed: toggleBlinkState,
                child: (toggleState
                  ?( Text('Blink'))
                  :(Text('Stop Blinking'))
                )
              )
            )
          ],
        ),
      ),
    );
  }
}
```

### What are the StatefulWidget and StatelessWidget best practices?

Here are a few things to consider when designing your widget.

#### 1. Determine whether a widget should be a StatefulWidget or a StatelessWidget


In Flutter, widgets are either Stateful or Stateless—depending on whether
they depend on a state change.

* If a widget changes—the user interacts with it or a data feed interrupts
  the UI, then it’s Stateful.
* If a widget is final or immutable, then it's Stateless.

#### 2. Determine which object manages the widget’s state (for a StatefulWidget)

In Flutter, there are three primary ways to manage state:

* The widget manages its own state
* The parent widget manages the widget’s state
* A mix-and-match approach

When deciding which approach to use, consider the following principles:

* If the state in question is user data, for example the checked or unchecked
  mode of a checkbox, or the position of a slider, then the state is best managed
  by the parent widget.
* If the state in question is aesthetic, for example an animation, then the
  widget itself best manages the state.
* When in doubt, let the parent widget manage the child widget's state.

#### 3. Subclass StatefulWidget and State

The `MyStatefulWidget` class manages its own state—it extends
`StatefulWidget`, it overrides the `createState()` method to create the State
object, and the framework calls `createState()` to build the widget. In this
example, `createState()` creates an instance of `_MyStatefulWidgetState`, which
is implemented in the next best practice.

<!-- skip -->
```dart
class MyStatefulWidget extends StatefulWidget {
  MyStatefulWidget({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _MyStatefulWidgetState createState() => _MyStatefulWidgetState();
}

class _MyStatefulWidgetState extends State<MyStatefulWidget> {

  @override
  Widget build(BuildContext context) {
    ...
  }
}
```

#### 4. Add the StatefulWidget into the widget tree

Add your custom `StatefulWidget` to the widget tree in the app’s build method.

<!-- skip -->
```dart
class MyStatelessWidget extends StatelessWidget {
  // This widget is the root of your application.

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyStatefulWidget(title: 'State Change Demo'),
    );
  }
}
```

{% include android-ios-figure-pair.md image="react-native/state-change.gif" alt="State change" class="border" %}

## Props

In React Native, most components can be customized when they are created with
different parameters or properties, called `props`. These
parameters can be used in a child component using `this.props`.

```js
// React Native
class CustomCard extends React.Component {
  render() {
    return (
      <View>
        <Text> Card {this.props.index} </Text>
        <Button
          title="Press"
          onPress={() => this.props.onPress(this.props.index)}
        />
      </View>
    );
  }
}
class App extends React.Component {

  onPress = index => {
    console.log("Card ", index);
  };

  render() {
    return (
      <View>
        <FlatList
          data={[ ... ]}
          renderItem={({ item }) => (
            <CustomCard onPress={this.onPress} index={item.key} />
          )}
        />
      </View>
    );
  }
}
```

In Flutter, you assign a local variable or function marked `final` with the
 property received in the parameterized constructor.

<!-- skip -->
```dart
// Flutter
class CustomCard extends StatelessWidget {

  CustomCard({@required this.index, @required this.onPress});
  final index;
  final Function onPress;

  @override
  Widget build(BuildContext context) {
  return Card(
    child: Column(
      children: <Widget>[
        Text('Card $index'),
        FlatButton(
          child: const Text('Press'),
          onPressed: this.onPress,
        ),
      ],
    ));
  }
}
    ...
//Usage
CustomCard(
  index: index,
  onPress: () {
    print('Card $index');
  },
)
```

{% include android-ios-figure-pair.md image="react-native/modular.png" alt="Cards" class="border" %}

## Local storage

If you don't need to store a lot of data and it doesn't require
structure, you can use `shared_preferences` which allows you to
read and write persistent key-value pairs of primitive data
types: booleans, floats, ints, longs, and strings.

### How do I store persistent key-value pairs that are global to the app?

In React Native, you use the `setItem` and `getItem` functions of the
`AsyncStorage` component to store and retrieve data that is persistent and
global to the app.

```js
// React Native
await AsyncStorage.setItem( "counterkey", json.stringify(++this.state.counter));
AsyncStorage.getItem("counterkey").then(value => {
  if (value != null) {
    this.setState({ counter: value });
  }
});
```

In Flutter, use the
[`shared_preferences`]({{site.github}}/flutter/plugins/tree/master/packages/shared_preferences)
plugin to store and retrieve key-value data that is persistent and global
to the app. The `shared_preferences` plugin wraps `NSUserDefaults` on iOS
and `SharedPreferences` on Android, providing a persistent store for simple data.
To use the plugin, add `shared_preferences` as a dependency in the `pubspec.yaml`
file then import the package in your Dart file.

```yaml
dependencies:
  flutter:
    sdk: flutter
  shared_preferences: ^0.4.3
```

<!-- skip -->
```dart
// Dart
import 'package:shared_preferences/shared_preferences.dart';
```

To implement persistent data, use the setter methods provided by the
`SharedPreferences` class. Setter methods are available for various primitive
types, such as `setInt`, `setBool`, and `setString`. To read data, use the
appropriate getter method provided by the `SharedPreferences` class. For each
setter there is a corresponding getter method, for example, `getInt`, `getBool`,
and `getString`.


<!-- skip -->
```dart
SharedPreferences prefs = await SharedPreferences.getInstance();
_counter = prefs.getInt('counter');
prefs.setInt('counter', ++_counter);
setState(() {
  _counter = _counter;
});
```


## Routing

Most apps contain several screens for displaying different types of information.
For example, you might have a product screen that displays images where users
could tap on a product image to get more information about the product on a new
screen.

In Android, new screens are new Activities. In iOS, new screens are new
ViewControllers. In Flutter, screens are just Widgets! And to navigate to new
screens in Flutter, use the Navigator widget.

### How do I navigate between screens?

In React Native, there are three main navigators: StackNavigator, TabNavigator,
and DrawerNavigator. Each provides a way to configure and define the screens.

```js
// React Native
const MyApp = TabNavigator(
  { Home: { screen: HomeScreen }, Notifications: { screen: tabNavScreen } },
  { tabBarOptions: { activeTintColor: "#e91e63" } }
);
const SimpleApp = StackNavigator({
  Home: { screen: MyApp },
  stackScreen: { screen: StackScreen }
});
export default (MyApp1 = DrawerNavigator({
  Home: {
    screen: SimpleApp
  },
  Screen2: {
    screen: drawerScreen
  }
}));
```

In Flutter, there are two main widgets used to navigate between screens:
* A [Route]({{site.api}}/flutter/widgets/Route-class.html)
  is an abstraction for an app screen or page.
* A [Navigator]({{site.api}}/flutter/widgets/Navigator-class.html)
  is a widget that manages routes.

A `Navigator` is defined as a widget that manages a set of child widgets with a
stack discipline. The navigator manages a stack of `Route` objects and provides
methods for managing the stack, like
[`Navigator.push`]({{site.api}}/flutter/widgets/Navigator/push.html)
and [`Navigator.pop`]({{site.api}}/flutter/widgets/Navigator/pop.html).
A list of routes might be specified in the
[`MaterialApp`]({{site.api}}/flutter/material/MaterialApp-class.html)
widget, or they might be built on the fly, for example, in hero animations.
The following example specifies named routes in the `MaterialApp` widget.

<!-- skip -->
```dart
// Flutter
class NavigationApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
            ...
      routes: <String, WidgetBuilder>{
        '/a': (BuildContext context) => usualNavscreen(),
        '/b': (BuildContext context) => drawerNavscreen(),
      }
            ...
  );
  }
}
```

To navigate to a named route, the
[of]({{site.api}}/flutter/widgets/Navigator/of.html)
method of the `Navigator` widget is used to specify the `BuildContext`
(a handle to the location of a widget in the widget tree).
The name of the route is passed to the `pushNamed` function to
navigate to the specified route.

<!-- skip -->
```dart
Navigator.of(context).pushNamed('/a');
```

You can also use the push method of `Navigator` which adds the given
[`route`]({{site.api}}/flutter/widgets/Route-class.html)
to the history of the navigator that most tightly encloses the given
[`context`]({{site.api}}/flutter/widgets/BuildContext-class.html),
and transitions to it. In the following example, the
[`MaterialPageRoute`]({{site.api}}/flutter/material/MaterialPageRoute-class.html)
widget is a modal route that replaces the entire screen with a platform-adaptive
transition. It takes a
[`WidgetBuilder`]({{site.api}}/flutter/widgets/WidgetBuilder.html)
as a required parameter.

<!-- skip -->
```dart
Navigator.push(context, MaterialPageRoute(builder: (BuildContext context)
 => UsualNavscreen()));
```

### How do I use tab navigation and drawer navigation?

In Material Design apps, there are two primary options for Flutter navigation:
tabs and drawers. When there is insufficient space to support tabs, drawers
provide a good alternative.


#### Tab navigation

In React Native, `createBottomTabNavigator` and `TabNavigation`  are used to
show tabs and for tab navigation.

```js
// React Native
import { createBottomTabNavigator } from 'react-navigation';

const MyApp = TabNavigator(
  { Home: { screen: HomeScreen }, Notifications: { screen: tabNavScreen } },
  { tabBarOptions: { activeTintColor: "#e91e63" } }
);
```

Flutter provides several specialized widgets for drawer and tab navigation:
* [TabController]({{site.api}}/flutter/material/TabController-class.html)&mdash;Coordinates
  the tab selection between a TabBar and a TabBarView.
* [TabBar]({{site.api}}/flutter/material/TabBar-class.html)&mdash;Displays
  a horizontal row of tabs.
* [Tab]({{site.api}}/flutter/material/Tab-class.html)&mdash;Creates
  a material design TabBar tab.
* [TabBarView]({{site.api}}/flutter/material/TabBarView-class.html)&mdash;Displays
  the widget that corresponds to the currently selected tab.


<!-- skip -->
```dart
// Flutter
TabController controller=TabController(length: 2, vsync: this);

TabBar(
  tabs: <Tab>[
    Tab(icon: Icon(Icons.person),),
    Tab(icon: Icon(Icons.email),),
  ],
  controller: controller,
),

```


A `TabController` is required to coordinate the tab selection between a `TabBar`
and a `TabBarView`. The `TabController` constructor `length` argument is the total
number of tabs. A `TickerProvider` is required to trigger the notification whenever
a frame triggers a state change. The `TickerProvider` is `vsync`. Pass the
`vsync: this` argument to the `TabController` constructor whenever you create
a new `TabController`.

The [TickerProvider]({{site.api}}/flutter/scheduler/TickerProvider-class.html)
is an interface implemented by classes that can vend
[`Ticker`]({{site.api}}/flutter/scheduler/Ticker-class.html)
objects. Tickers can be used by any object that must be notified whenever a
frame triggers, but they're most commonly used indirectly via an
[`AnimationController`]({{site.api}}/flutter/animation/AnimationController-class.html).
`AnimationControllers` need a `TickerProvider` to obtain their `Ticker`.
If you are creating an AnimationController from a State, then you can use the
[`TickerProviderStateMixin`]({{site.api}}/flutter/widgets/TickerProviderStateMixin-mixin.html)
or [`SingleTickerProviderStateMixin`]({{site.api}}/flutter/widgets/SingleTickerProviderStateMixin-mixin.html)
classes to obtain a suitable `TickerProvider`.

The [`Scaffold`]({{site.api}}/flutter/material/Scaffold-class.html)
widget wraps a new `TabBar` widget and creates two tabs. The `TabBarView` widget
is passed as the `body` parameter of the `Scaffold` widget. All screens
corresponding to the `TabBar` widget’s tabs are children to the `TabBarView`
widget along with the same `TabController`.


<!-- skip -->
```dart
// Flutter

class _NavigationHomePageState extends State<NavigationHomePage> with SingleTickerProviderStateMixin {
  TabController controller=TabController(length: 2, vsync: this);
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      bottomNavigationBar: Material (
        child: TabBar(
          tabs: <Tab> [
            Tab(icon: Icon(Icons.person),)
            Tab(icon: Icon(Icons.email),),
          ],
          controller: controller,
        ),
        color: Colors.blue,
      ),
      body: TabBarView(
        children: <Widget> [
          home.homeScreen(),
          tabScreen.tabScreen()
        ],
        controller: controller,
      )
    );
  }
}
```

#### Drawer navigation

In React Native, import the needed react-navigation packages and then use
`createDrawerNavigator` and `DrawerNavigation`.

```js
// React Native
export default (MyApp1 = DrawerNavigator({
  Home: {
    screen: SimpleApp
  },
  Screen2: {
    screen: drawerScreen
  }
}));
```

In Flutter, we can use the `Drawer` widget in combination with a `Scaffold` to
create a layout with a Material Design drawer. To add a `Drawer` to an app,
wrap it in a `Scaffold` widget. The `Scaffold` widget provides a consistent
visual structure to apps that follow the
[Material Design]({{site.material}}/design) guidelines. It also supports
special Material Design components, such as `Drawers`, `AppBars`, and `SnackBars`.

The `Drawer` widget is a Material Design panel that slides in horizontally from
the edge of a `Scaffold` to show navigation links in an application. You can
provide a [`Button`]({{site.api}}/flutter/material/RaisedButton-class.html),
a [`Text`]({{site.api}}/flutter/widgets/Text-class.html) widget,
or a list of items to display as the child to the `Drawer` widget.
In the following example, the
[`ListTile`]({{site.api}}/flutter/material/ListTile-class.html)
widget provides the navigation on tap.

<!-- skip -->
```dart
// Flutter
Drawer(
  child:ListTile(
    leading: Icon(Icons.change_history),
    title: Text('Screen2'),
    onTap: () {
      Navigator.of(context).pushNamed("/b");
    },
  ),
  elevation: 20.0,
),
```

The `Scaffold` widget also includes an `AppBar` widget that automatically
displays an appropriate IconButton to show the `Drawer` when a Drawer is
available in the `Scaffold`. The `Scaffold` automatically handles the
edge-swipe gesture to show the `Drawer`.

<!-- skip -->
```dart
// Flutter
@override
Widget build(BuildContext context) {
  return Scaffold(
    drawer: Drawer(
      child: ListTile(
        leading: Icon(Icons.change_history),
        title: Text('Screen2'),
        onTap: () {
          Navigator.of(context).pushNamed("/b");
        },
      ),
      elevation: 20.0,
    ),
    appBar: AppBar(
      title: Text("Home"),
    ),
    body: Container(),
  );
}
```

{% include android-ios-figure-pair.md image="react-native/navigation.gif" alt="Navigation" class="border" %}

## Gesture detection and touch event handling

To listen for and respond to gestures, Flutter supports taps, drags, and
scaling. The gesture system in Flutter has two separate layers. The first layer
includes raw pointer events, which describe the location and movement of
pointers, (such as touches, mice, and styli movements), across the screen. The
second layer includes gestures, which describe semantic actions that consist of
one or more pointer movements.

### How do I add a click or press listeners to a widget?

In React Native, listeners are added to components using `PanResponder` or
the `Touchable` components.

```js
// React Native
<TouchableOpacity
  onPress={() => {
    console.log("Press");
  }}
  onLongPress={() => {
    console.log("Long Press");
  }}
>
  <Text>Tap or Long Press</Text>
</TouchableOpacity>
```

For more complex gestures and combining several touches into a single gesture,
[`PanResponder`](https://facebook.github.io/react-native/docs/panresponder.html)
is used.

```js
// React Native
class App extends Component {

  componentWillMount() {
    this._panResponder = PanResponder.create({
      onMoveShouldSetPanResponder: (event, gestureState) =>
        !!getDirection(gestureState),
      onPanResponderMove: (event, gestureState) => true,
      onPanResponderRelease: (event, gestureState) => {
        const drag = getDirection(gestureState);
      },
      onPanResponderTerminationRequest: (event, gestureState) => true
    });
  }

  render() {
    return (
      <View style={styles.container} {...this._panResponder.panHandlers}>
        <View style={styles.center}>
          <Text>Swipe Horizontally or Vertically</Text>
        </View>
      </View>
    );
  }
}
```

In Flutter, to add a click (or press) listener to a widget, use a button
or a touchable widget that has an `onPress: field`. Or, add gesture detection
to any widget by wrapping it in a
[`GestureDetector`]({{site.api}}/flutter/widgets/GestureDetector-class.html).

<!-- skip -->
```dart
// Flutter
GestureDetector(
  child: Scaffold(
    appBar: AppBar(
      title: Text("Gestures"),
    ),
    body: Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          Text('Tap, Long Press, Swipe Horizontally or Vertically '),
        ],
      )
    ),
  ),
  onTap: () {
    print('Tapped');
  },
  onLongPress: () {
    print('Long Pressed');
  },
  onVerticalDragEnd: (DragEndDetails value) {
    print('Swiped Vertically');
  },
  onHorizontalDragEnd: (DragEndDetails value) {
    print('Swiped Horizontally');
  },
);
```
For more information, including a list of Flutter `GestureDetector` callbacks,
see the [GestureDetector class][].

[GestureDetector class]: {{site.api}}/flutter/widgets/GestureDetector-class.html#instance-properties

{% include android-ios-figure-pair.md image="react-native/flutter-gestures.gif" alt="Gestures" class="border" %}

## Making HTTP network requests

Fetching data from the internet is common for most apps. And in Flutter,
the `http` package provides the simplest way to fetch data from the internet.

### How do I fetch data from API calls?

React Native provides the Fetch API for networking—you make a fetch request
and then receive the response to get the data.

```js
// React Native
_getIPAddress = () => {
  fetch("https://httpbin.org/ip")
    .then(response => response.json())
    .then(responseJson => {
      this.setState({ _ipAddress: responseJson.origin });
    })
    .catch(error => {
      console.error(error);
    });
};
```

Flutter uses the `http` package. To install the `http` package, add it to
the dependencies section of our pubspec.yaml.

```yaml
dependencies:
  flutter:
    sdk: flutter
  http: <latest_version>
```

Flutter uses the
[`dart:io`]({{site.api}}/flutter/dart-io/dart-io-library.html)
core HTTP support client. To create an HTTP Client, import `dart:io`.

<!-- skip -->
```dart
import 'dart:io';
```

The client supports the following HTTP operations: GET, POST, PUT, and DELETE.

<!-- skip -->
```dart
// Flutter
final url = Uri.https('httpbin.org', 'ip');
final httpClient = HttpClient();
_getIPAddress() async {
  var request = await httpClient.getUrl(url);
  var response = await request.close();
  var responseBody = await response.transform(utf8.decoder).join();
  String ip = json.decode(responseBody)['origin'];
  setState(() {
    _ipAddress = ip;
  });
}
```

{% include android-ios-figure-pair.md image="react-native/api-calls.gif" alt="API calls" class="border" %}

## Form input

Text fields allow users to type text into your app so they can be used to build
forms, messaging apps, search experiences, and more. Flutter provides two
core text field widgets:
[TextField]({{site.api}}/flutter/material/TextField-class.html) and
[TextFormField]({{site.api}}/flutter/material/TextFormField-class.html).

### How do I use text field widgets?

In React Native, to enter text you use a `TextInput` component to show a text
input box and then use the callback to store the value in a variable.

```js
// React Native
<TextInput
  placeholder="Enter your Password"
  onChangeText={password => this.setState({ password })}
 />
<Button title="Submit" onPress={this.validate} />
```

In Flutter, use the
[`TextEditingController`]({{site.api}}/flutter/widgets/TextEditingController-class.html)
class to manage a `TextField` widget. Whenever the text field is modified,
the controller notifies its listeners.

Listeners read the text and selection properties to learn what the user typed
into the field. You can access the text in `TextField` by the `text` property of
the controller.

<!-- skip -->
```dart
// Flutter
final TextEditingController _controller = TextEditingController();
      ...
TextField(
  controller: _controller,
  decoration: InputDecoration(
    hintText: 'Type something', labelText: "Text Field "
  ),
),
RaisedButton(
  child: Text('Submit'),
  onPressed: () {
    showDialog(
      context: context,
        child: AlertDialog(
          title: Text('Alert'),
          content: Text('You typed ${_controller.text}'),
        ),
     );
   },
 ),
)
```

In this example, when a user clicks on the submit button an alert dialog
displays the current text entered in the text field. This is achieved using an
[`alertDialog`]({{site.api}}/flutter/material/AlertDialog-class.html)
widget that displays the alert message, and the text from
the `TextField` is accessed by the `text` property of the
[TextEditingController]({{site.api}}/flutter/widgets/TextEditingController-class.html).

### How do I use Form widgets?

In Flutter, use the
[`Form`]({{site.api}}/flutter/widgets/Form-class.html)
widget where
[`TextFormField`]({{site.api}}/flutter/material/TextFormField-class.html)
widgets along with the submit button are passed as children.
The `TextFormField` widget has a parameter called
[`onSaved`]({{site.api}}/flutter/widgets/FormField/onSaved.html)
which takes a callback and executes when the form is saved. A `FormState`
object is used to save, reset, or validate
each `FormField` that is a descendant of this `Form`. To obtain the `FormState`,
you can use  `Form.of` with a context whose ancestor is the Form, or pass a
`GlobalKey` to the Form constructor and call `GlobalKey.currentState`.

<!-- skip -->
```dart
final formKey = GlobalKey<FormState>();

...

Form(
  key:formKey,
  child: Column(
    children: <Widget>[
      TextFormField(
        validator: (value) => !value.contains('@') ? 'Not a valid email.' : null,
        onSaved: (val) => _email = val,
        decoration: const InputDecoration(
          hintText: 'Enter your email',
          labelText: 'Email',
        ),
      ),
      RaisedButton(
        onPressed: _submit,
        child: Text('Login'),
      ),
    ],
  ),
)
```

The following example shows how `Form.save()` and `formKey` (which is a
`GlobalKey`) are used to save the form on submit.

<!-- skip -->
```dart
void _submit() {
  final form = formKey.currentState;
  if (form.validate()) {
    form.save();
    showDialog(
      context: context,
      child: AlertDialog(
        title: Text('Alert'),
        content: Text('Email: $_email, password: $_password'),
      )
    );
  }
}
```

{% include android-ios-figure-pair.md image="react-native/input-fields.gif" alt="Input" class="border" %}

## Platform-specific code

When building a cross-platform app, you want to re-use as much code as
possible across platforms. However, scenarios may arise where it makes sense for
the code to be different depending on the OS. This requires a separate
implementation by declaring a specific platform.

In React Native, the following implementation would be used:

```js
// React Native
if (Platform.OS === "ios") {
  return "iOS";
} else if (Platform.OS === "android") {
  return "android";
} else {
  return "not recognised";
}
```
In Flutter, use the following implementation:
<!-- skip -->
```dart
// Flutter
if (Theme.of(context).platform == TargetPlatform.iOS) {
  return "iOS";
} else if (Theme.of(context).platform == TargetPlatform.android) {
  return "android";
} else if (Theme.of(context).platform == TargetPlatform.fuchsia) {
  return "fuchsia";
} else {
  return "not recognised ";
}
```

## Debugging

Before running your applications, verify your code with `flutter analyze`. The
Flutter analyzer (which is a wrapper around the `dartanalyzer` tool) examines
your code and helps identify possible issues. If you’re using a Flutter-enabled
IDE, this occurs automatically.

### How do I access the in-app developer menu?

In React Native, the developer menu can be accessed by shaking your device: ⌘D
for the iOS Simulator or ⌘M for Android emulator.

In Flutter, if you are using an IDE, you can use the IDE tools. If you start
your application using `flutter run` you can also access the menu by typing `h`
in the terminal window, or type the following shortcuts:

<div class="table-wrapper" markdown="1">
| Action| Terminal Shortcut| Debug functions and properties|
| :------- | :------: | :------ |
| Widget hierarchy of the app| `w`| debugDumpApp()|
| Rendering tree of the app | `t`| debugDumpRenderTree()|
| Layers| `L`| debugDumpLayerTree()|
| Accessibility | `S` (traversal order) or<br>`U` (inverse hit test order)|debugDumpSemantics()|
| To toggle the widget inspector | `i` | WidgetsApp. showWidgetInspectorOverride|
| To toggle the display of construction lines| `p` | debugPaintSizeEnabled|
| To simulate different operating systems| `o` | defaultTargetPlatform|
| To display the performance overlay | `P` | WidgetsApp. showPerformanceOverlay|
| To save a screenshot to flutter. png| `s` ||
| To quit| `q` ||
{:.table.table-striped}
</div>

### How do I perform a hot reload?

Flutter’s hot reload feature helps you quickly and easily experiment, build UIs,
add features, and fix bugs. Instead of recompiling your app every time you make
a change, you can hot reload your app instantly. The app is updated to reflect
your change, and the current state of the app is preserved.

In React Native, the shortcut is ⌘R for the iOS Simulator and tapping R twice on
Android emulators.

In Flutter, If you are using IntelliJ IDE or Android Studio, you can select Save
All (⌘s/ctrl-s), or you can click the Hot Reload button on the toolbar. If you
are running the app at the command line using `flutter run`, type `r` in the
Terminal window. You can also perform a full restart by typing `R` in the
Terminal window.

### What tools can I use to debug my app in Flutter?

There are several options and tools you can use when you need to debug your
Flutter app.

In addition to the Flutter analyzer, the
[`Dart Observatory`](https://dart-lang.github.io/observatory/) is a tool used to
profile and debug your Dart applications. If you started your application using
`flutter run` in Terminal, you can open the web page at the Observatory URL
printed to the terminal window, for example:  `http://127.0.0.1:8100/`.

The Observatory includes support for profiling, examining the heap, observing
executed lines of code, debugging memory leaks and memory fragmentation. For
more information, see the
[Observatory documentation](https://dart-lang.github.io/observatory/).
Observatory is included for free when you download and install the Dart SDK.

If you're using an IDE, you can debug your application using the IDE debugger.

If you're using IntelliJ and Android Studio, you can use the Flutter Inspector.
 The Flutter Inspector makes it much easier to understand why your application
  is rendering the way it does. It allows you to:
* View the UI structure of your app as a tree of widgets
* Select a point on your device or simulator and find the corresponding widget
 that rendered those pixels
* View properties for individual widgets
* Quickly identify layout issues and determine their cause

The Flutter Inspector view can be opened from View > Tool Windows > Flutter
Inspector. Content is shown only when an app is running.

To inspect a specific widget, select the **Toggle inspect mode** action in the
toolbar, then click on the desired widget on an attached phone or simulator. The
widget is highlighted in your app’s UI. You’ll see the widget in the widget
hierarchy in IntelliJ and the individual properties for that widget.

For more information, see
[Debugging Flutter Apps](/docs/testing/debugging).

## Animation

Well-designed animation makes a UI feel intuitive, contributes to the look and
feel of a polished app, and improves the user experience. Flutter’s animation
support makes it easy to implement simple and complex animations. The Flutter
SDK includes many Material Design widgets that include standard
motion effects and you can easily customize these effects to personalize your
app.

In React Native, Animated APIs are used to create animations.

In Flutter, use the
[`Animation`]({{site.api}}/flutter/animation/Animation-class.html)
class and the
[`AnimationController`]({{site.api}}/flutter/animation/AnimationController-class.html)
class.  `Animation` is an abstract class that understands its current value and
its state (completed or dismissed). The `AnimationController` class lets you
play an animation forward or in reverse, or stop animation and set the animation
to a specific value to customize the motion.

### How do I add a simple fade-in animation?

In the React Native example below, an animated component, `FadeInView` is
created using the Animated API. The initial opacity state, final state, and the
duration over which the transition occurs are defined. The animation component
is added inside the `Animated` component, the opacity state `fadeAnim` is mapped
to the opacity of the Text component that we want to animate, and then,
`start()` is called to start the animation.

```js
// React Native
class FadeInView extends React.Component {
  state = {
    fadeAnim: new Animated.Value(0) // Initial value for opacity: 0
  };
  componentDidMount() {
    Animated.timing(this.state.fadeAnim, {
      toValue: 1,
      duration: 10000
    }).start();
  }
  render() {
    return (
      <Animated.View style={%raw%}{{...this.props.style, opacity: this.state.fadeAnim }}{%endraw%} >
        {this.props.children}
      </Animated.View>
    );
  }
}
    ...
<FadeInView>
  <Text> Fading in </Text>
</FadeInView>
    ...
```

To create the same animation in Flutter, create an
[`AnimationController`]({{site.api}}/flutter/animation/AnimationController-class.html)
object named `controller` and specify the duration. By default, an
`AnimationController` linearly produces values that range from 0.0 to 1.0,
during a given duration. The animation controller generates a new value
whenever the device running your app is ready to display a new frame.
Typically, this rate is around 60 values per second.

When defining an `AnimationController`, you must pass in a `vsync` object. The
presence of `vsync` prevents offscreen animations from consuming unnecessary
resources. You can use your stateful object as the `vsync` by adding
`TickerProviderStateMixin` to the class definition. An `AnimationController`
needs a TickerProvider, which is configured using the `vsync` argument on the
constructor.

A [`Tween`]({{site.api}}/flutter/animation/Tween-class.html)
describes the interpolation between a beginning and ending value
or the mapping from an input range to an output range. To use a `Tween` object
with an animation, call the `Tween` object's `animate` method and pass it the
`Animation` object that you want to modify.

For this example, a
[`FadeTransition`]({{site.api}}/flutter/widgets/FadeTransition-class.html)
widget is used and the `opacity` property is mapped to the `animation` object.

To start the animation, use `controller.forward()`. Other operations can also be
performed using the controller such as `fling()` or `repeat()`. For this
example, the
[`FlutterLogo`]({{site.api}}/flutter/material/FlutterLogo-class.html)
widget is used inside the `FadeTransition` widget.

<!-- skip -->
```dart

// Flutter
import 'package:flutter/material.dart';

void main() {
  runApp(Center(child: LogoFade()));
}

class LogoFade extends StatefulWidget {
  _LogoFadeState createState() => _LogoFadeState();
}

class _LogoFadeState extends State<LogoFade> with TickerProviderStateMixin {
  Animation animation;
  AnimationController controller;

  initState() {
    super.initState();
    controller = AnimationController(
        duration: const Duration(milliseconds: 3000), vsync: this);
    final CurvedAnimation curve =
    CurvedAnimation(parent: controller, curve: Curves.easeIn);
    animation = Tween(begin: 0.0, end: 1.0).animate(curve);
    controller.forward();
  }

  Widget build(BuildContext context) {
    return FadeTransition(
      opacity: animation,
      child: Container(
        height: 300.0,
        width: 300.0,
        child: FlutterLogo(),
      ),
    );
  }

  dispose() {
    controller.dispose();
    super.dispose();
  }
}
```

{% include android-ios-figure-pair.md image="react-native/flutter-fade.gif" alt="Flutter fade" class="border" %}

### How do I add swipe animation to cards?

In React Native, either the `PanResponder` or third-party libraries are used for
swipe animation.

In Flutter, to add a swipe animation, use the
[`Dismissible`]({{site.api}}/flutter/widgets/Dismissible-class.html)
widget and nest the child widgets.

<!-- skip -->
```dart
child: Dismissible(
  key: key,
  onDismissed: (DismissDirection dir) {
    cards.removeLast();
  },
  child: Container(
    ...
  ),
),
```

{% include android-ios-figure-pair.md image="react-native/card-swipe.gif" alt="Card swipe" class="border" %}

## React Native and Flutter Widget equivalent components

The following table lists commonly-used React Native components mapped to the
corresponding Flutter widget and common widget properties.

<div class="table-wrapper" markdown="1">
| React Native Component                                                                    | Flutter Widget                                                                                             | Description                                                                                                                            |
| ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| [Button](https://facebook.github.io/react-native/docs/button.html)                        | [Raised Button]({{site.api}}/flutter/material/RaisedButton-class.html)                           | A basic raised button.                                                                              |
|                                                                                           |  onPressed [required]                                                                                        | The callback when the button is tapped or otherwise activated.                                                          |
|                                                                                           | Child                                                                              | The button's label.                                                                                                      |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [Button](https://facebook.github.io/react-native/docs/button.html)                        | [Flat Button]({{site.api}}/flutter/material/FlatButton-class.html)                               | A basic flat button.                                                                                                         |
|                                                                                           |  onPressed [required]                                                                                        | The callback when the button is tapped or otherwise activated.                                                            |
|                                                                                           | Child                                                                              | The button's label.                                                                                                      |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [ScrollView](https://facebook.github.io/react-native/docs/scrollview.html)                | [ListView]({{site.api}}/flutter/widgets/ListView-class.html)                                    | A scrollable list of widgets arranged linearly.|
||        children                                                                              | 	( <Widget\> [ ])  List of child widgets to display.
||controller |[ [Scroll Controller]({{site.api}}/flutter/widgets/ScrollController-class.html) ] An object that can be used to control a scrollable widget.
||itemExtent|[ double ] If non-null, forces the children to have the given extent in the scroll direction.
||scroll Direction|[ [Axis]({{site.api}}/flutter/painting/Axis-class.html) ] The axis along which the scroll view scrolls.
||                                                                                                            |                                                                                                                                        |
| [FlatList](https://facebook.github.io/react-native/docs/flatlist.html)                    | [ListView. builder()]({{site.api}}/flutter/widgets/ListView/ListView.builder.html)               | The constructor for a linear array of widgets that are created on demand.
||itemBuilder [required] |[[ Indexed Widget Builder]({{site.api}}/flutter/widgets/IndexedWidgetBuilder.html)] helps in building the children on demand. This callback is called only with indices greater than or equal to zero and less than the itemCount.
||itemCount |[ int ] improves the ability of the ListView to estimate the maximum scroll extent.
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [Image]({{site.api}}/flutter/widgets/Image-class.html)                         | [Image](https://facebook.github.io/react-native/docs/image.html)                                           | A widget that displays an image.                                                                                                       |
|                                                                                           |  image [required]                                                                                          | The image to display.                                                                                                                  |
|                                                                                           | Image. asset                                                                                                | Several constructors are provided for the various ways that an image can be specified.                                                 |
|                                                                                           | width, height, color, alignment                                                                            | The style and layout for the image.                                                                                                         |
|                                                                                           | fit                                                                                                        | Inscribing the image into the space allocated during layout.                                                                           |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [Modal](https://facebook.github.io/react-native/docs/modal.html)                          | [ModalRoute]({{site.api}}/flutter/widgets/ModalRoute-class.html)                                | A route that blocks interaction with previous routes.                                                                                  |
|                                                                                           | animation                                                                                                  | The animation that drives the route's transition and the previous route's forward transition.                                          |
|                                                                                           |                                                                                                            |                                                                                                                                        |
|  [Activity Indicator](https://facebook.github.io/react-native/docs/activityindicator.html) | [Circular Progress Indicator]({{site.api}}/flutter/material/CircularProgressIndicator-class.html) | A widget that shows progress along a circle.                                                                                           |
|                                                                                           | strokeWidth                                                                                                | The width of the line used to draw the circle.                                                                                         |
|                                                                                           | backgroundColor                                                                                            | The progress indicator's background color. The current theme's `ThemeData.backgroundColor` by default.                                   |
|                                                                                           |                                                                                                            |                                                                                                                                        |
|  [Activity Indicator](https://facebook.github.io/react-native/docs/activityindicator.html) | [Linear Progress Indicator]({{site.api}}/flutter/material/LinearProgressIndicator-class.html)     | A widget that shows progress along a circle.                                                                                           |
|                                                                                           | value                                                                                                      | The value of this progress indicator.                                                                                                   |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [Refresh Control](https://facebook.github.io/react-native/docs/refreshcontrol.html)        | [Refresh Indicator]({{site.api}}/flutter/material/RefreshIndicator-class.html)                   | A widget that supports the Material "swipe to refresh" idiom.                                                                          |
|                                                                                           | color                                                                                                      | The progress indicator's foreground color.                                                                                             |
|                                                                                           | onRefresh                                                                                                  | A function that's called when a user drags the refresh indicator far enough to demonstrate that they want the app to refresh.  |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [View](https://facebook.github.io/react-native/docs/view.html)                            | [Container]({{site.api}}/flutter/widgets/Container-class.html)                                  | A widget that surrounds a child widget.                                                                                                                |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [View](https://facebook.github.io/react-native/docs/view.html)                            | [Column]({{site.api}}/flutter/widgets/Column-class.html)                                        | A widget that displays its children in a vertical array.                                                                                              |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [View](https://facebook.github.io/react-native/docs/view.html)                            | [Row]({{site.api}}/flutter/widgets/Row-class.html)                                              | A widget that displays its children in a horizontal array.                                                                                            |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [View](https://facebook.github.io/react-native/docs/view.html)                            | [Center]({{site.api}}/flutter/widgets/Center-class.html)                                        | A widget that centers its child within itself.                                                                                                       |
|                                                                                           |                                                                                                            |                                                                                                                                        |
| [View](https://facebook.github.io/react-native/docs/view.html)                            | [Padding]({{site.api}}/flutter/widgets/Padding-class.html)                                      | A widget that insets its child by the given padding.                                                                                                 |
|                                                                                           | padding [required]                                                                                         | [ EdgeInsets ] The amount of space to inset the child.
|||
| [Touchable Opacity](https://facebook.github.io/react-native/docs/touchableopacity.html)    | [Gesture Detector]({{site.api}}/flutter/widgets/GestureDetector-class.html)                      | A widget that detects gestures.                                                                                                                       |
|                                                                                           | onTap                                                                                                      | A callback when a tap occurs.                                                                                                               |
|                                                                                           | onDoubleTap                                                                                                | A callback when a tap occurs at the same location twice in quick succession.
|||
| [Text Input]({{site.api}}/flutter/services/TextInput-class.html)                | [Text Input](https://facebook.github.io/react-native/docs/textinput.html)                                   | The interface to the system's text input control.                                                                                           |
|                                                                                           | controller                                                                                                 | [ [Text Editing Controller]({{site.api}}/flutter/widgets/TextEditingController-class.html) ] used to access and modify text.
|||
| [Text](https://facebook.github.io/react-native/docs/text.html)                          | [Text]({{site.api}}/flutter/widgets/Text-class.html)                                            | The Text widget that displays a string of text with a single style.                                                                                                                                                                           |
|                                                                                         | data                                                                                                      | [ String ] The text to display.                                                                                                                                                                              |
|                                                                                         | textDirection                                                                                             | [ [Text Align]({{site.api}}/flutter/dart-ui/TextAlign-class.html) ] The direction in which the text flows.                                                                                     |
|                                                                                         |                                                                                                           |                                                                                                                                                                                                              |
| [Switch](https://facebook.github.io/react-native/docs/switch.html)                      | [Switch]({{site.api}}/flutter/material/Switch-class.html)                                      | A material design switch.                                                                                                                                                                                    |
|                                                                                         | value [required]                                                                                          | [ boolean ] Whether this switch is on or off.                                                                                                                                                                 |
|                                                                                         | onChanged [required]                                                                                      | [ callback ] Called when the user toggles the switch on or off.                                                                                                                                               |
|                                                                                         |                                                                                                           |                                                                                                                                                                                                              |
| [Slider](https://facebook.github.io/react-native/docs/slider.html)                      | [Slider]({{site.api}}/flutter/material/Slider-class.html)                                      | Used to select from a range of values.                                                                                                                                                                       |
|                                                                                         | value [required]                                                                                          | [ double ] The current value of the slider.                                                                                                                                                                           |
|                                                                                         | onChanged [required]                                                                                      | Called when the user selects a new value for the slider.                                                                                                                                                      |
{:.table.table-striped}
</div>
