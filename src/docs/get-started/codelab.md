---
title: 첫 번째 Flutter 앱 작성하기 - 1부
short-title: 첫 번째 Flutter 앱 작성하기
prev:
  title: 시운전
  path: /docs/get-started/test-drive
next:
  title: 더 알아보기
  path: /docs/get-started/learn-more
diff2html: true
---

{% assign code-url = 'https://raw.githubusercontent.com/flutter/codelabs/master' -%}

{% asset get-started/startup-namer-part-1 alt="The app that you'll be building" class='site-image-right' %}

{%- comment %}
  Code highlights in this page are green, to match diff additions.
{% endcomment -%}
<style>pre .highlight { background-color: #dfd; }</style>

첫 번째 Flutter 앱을 만드는 방법을 안내합니다.
변수, 반복문, 조건문 등 기본 프로그래밍 개념과 객체지향에 친숙하다면, 이 튜토리얼을 완료할 수 있습니다.
다트 경험이나 모바일 프로그래밍 경험이 없어도 상관없습니다.

이 가이드는 코드랩 2부 중 1부입니다.
[2부]({{site.codelabs}}/codelabs/first-flutter-app-pt2)는
[Google Developers]({{site.codelabs}})에서 보실 수 있습니다.
[1부]({{site.codelabs}}/codelabs/first-flutter-app-pt1) 또한
[Google Developers]({{site.codelabs}})에서도 볼 수 있습니다.

## 1부에서 무엇을 만드는가
{:.no_toc}

스타트업 회사를 위해 이름을 생성하여 제안하는 간단한 모바일 앱을 구현할 것입니다.
사용자는 이름을 선택하거나 선택을 취소할 수 있으며, 가장 좋은 이름을 저장할 수 있습니다.
코드는 lazy하게 이름을 생성합니다.
사용자가 스크롤하면 더 많은 이름이 생성됩니다.
무한하게 스크롤 할 수 있습니다.

GIF 애니메이션은 1부를 완료하면 앱이 어떻게 동작하는지를 보여줍니다.

{{site.alert.secondary}}
  <h4 class="no_toc">1부에서 무엇을 배우는가</h4>

  * iOS와 Android 모두에서 자연스럽게 보이는 Flutter 앱을 작성하는 방법.
  * Flutter 앱의 기본 구조.
  * 패키지를 찾고 패키지를 사용하여 기능을 확장하기.
  * 더 빠른 개발 사이클을 위한 hot reload 사용.
  * Stateful 위젯을 구현하는 방법.
  * Lazy하게 로드하는 무한 리스트를 만드는 방법.

  코드랩 [2부]({{site.codelabs}}/codelabs/first-flutter-app-pt2)에서는
  상호작용을 추가하고, 앱 테마를 수정하며, 새 화면으로 이동하는 기능(Flutter에서 _route_ 라고 불립니다)을 추가합니다.
{{site.alert.end}}

{{site.alert.secondary}}
  <h4 class="no_toc">무엇을 사용하는가</h4>

  이 코드랩을 완료하기 위해 
  [Flutter SDK](/docs/get-started/install)와 [에디터](/docs/get-started/editor)가 필요합니다.
  이 코드랩은 안드로이드 스튜디오를 기반으로 되어있지만, 원한다면 에디터를 이용할 수도 있습니다. 

  이 코드랩을 아래 기기에서 실행할 수 있습니다:

  * 컴퓨터에 연결되어 있고 개발자 모드로 설정되어 있는 
    실제 기기 ([안드로이드](install/macos#set-up-your-android-device)
    또는 [iOS](install/macos#deploy-to-ios-devices)).
  * [iOS 시뮬레이터](install/macos#set-up-the-ios-simulator).
  * [Android 에뮬레이터](install/macos#set-up-the-android-emulator).
{{site.alert.end}}

## 1단계: Starter Flutter app 만들기 

<?code-excerpt path-base="codelabs/startup_namer/step1_base"?>

[첫 번째 Flutter 앱 시작하기](/docs/get-started/test-drive#create-app)에 있는 지침을 따라 
간단한 템플릿 기반 Flutter 앱을 만듭니다.
이름을 **startup_namer**로 지정합니다 (_myapp_ 대신).

{{site.alert.tip}}
  IDE에서 "New Flutter Project"가 보이지 않는다면,
  [Flutter와 다트 플러그인 설치](/docs/get-started/editor)가 설치되어 있는지 확인하세요.
{{site.alert.end}}

코드랩에서는 주로 다트 코드가 있는 **lib/main.dart**를 수정할 것입니다.

 1. `lib/main.dart`의 내용을 바꿉니다.<br>
    **lib/main.dart**의 모든 코드를 삭제합니다.
    화면 중앙에 "Hello World"를 표시하는 아래 코드로 대체합니다.

    <?code-excerpt "lib/main.dart" title?>
    ```dart
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
              child: Text('Hello World'),
            ),
          ),
        );
      }
    }
    ```

    {{site.alert.tip}}
      코드를 앱에 붙여 넣을 때 들여쓰기가 깨질 수 있습니다.
      Flutter 도구를 활용하여 자동으로 문제를 해결할 수 있습니다.

      * 안드로이드 스튜디오 / IntelliJ IDEA: 코드에서 오른쪽 버튼을 클릭한 후 
      **Reformat Code with dartfmt**를 선택하세요.
      * VS Code: 오른쪽 버튼을 클릭한 후 **Format Document**를 선택하세요.
      * 터미널: `flutter format <파일명>`을 실행하세요.
    {{site.alert.end}}

 2. IDE에서 녹색 화살표를 클릭하여 [앱을 실행](/docs/get-started/test-drive#androidstudio)하세요.  
    기기에 따라, 안드로이드 또는 iOS 화면에 글자가 표시되어야 합니다. 

    {% indent %}
      {% include android-ios-figure-pair.md image="hello-world.png" alt="Hello world app" %}
    {% endindent %}

    {{site.alert.tip}}
      실제 기기에서 처음 실행할 때는, 로딩에 시간이 걸릴 수 있습니다.
      이후, 빠른 업데이트를 위해 hot reload를 사용할 수 있습니다.
      앱이 실행 중일 때 **저장**을 하면 hot reload를 수행합니다.
    {{site.alert.end}}

### 관찰
{:.no_toc}

* 이 예제는 머터리얼 앱을 만듭니다. 
  [머터리얼]({{site.material}}/guidelines/)은
  모바일 및 웹에서 표준으로 사용되는 시각 디자인 언어입니다.
  Flutter는 다양한 머터리얼 위젯을 제공합니다. 
* `main()` 메서드는 화살표(`=>`) 표기법을 사용합니다.
  한 줄 함수 또는 메서드에 화살표 표기법을 사용하세요.
* 앱은 `StatelessWidget`을 상속받아 앱 자체를 위젯으로 만듭니다.
  Flutter에서는 정렬, 여백, 레이아웃 등 거의 모든것이 위젯입니다.
* 머터리얼 라이브러리의 `Scaffold` 위젯은
  홈 스크린의 위젯 트리를 구성하는
  app bar, title, body 속성을 기본으로 제공합니다.  
  위젯 하위 트리는 상당히 복잡할 수 있습니다. 
* 위젯의 주된 임무는 
  다른 하위 위젯을 어떻게 표현할 지를 설명하는
  `build()` 메서드를 제공하는 것입니다.  
* 이 예제는 자식 위젯으로 `Text`을 포함하는 `Center` 위젯으로 구성됩니다. 
  Center 위젯은 하위 위젯을 화면 중앙에 정렬합니다. 

## 2단계: 외부 패키지 이용하기

이 단계에서는 
가장 많이 사용되는 영어 단어 수천 개와 몇 가지 유틸리티 기능이 포함되어 있는
오픈 소스 패키지인 [english_words]({{site.pub}}/packages/english_words)를 이용할 것입니다.

다른 오픈 소스 패키지와 마찬가지로, 
[the Package site]({{site.pub}}/flutter)에서
`english_words` 패키지를 찾을 수 있습니다.

 1. Flutter 앱에서 디펜던시 및 asset 관리는 pubspec 파일이 담당합니다. 
    `pubspec.yaml`의 디펜던시 목록에 `english_words` (3.1.0 이상)를 추가하세요:  

    <?code-excerpt path-base="codelabs/startup_namer"?>
    <?code-excerpt "{step1_base,step2_use_package}/pubspec.yaml" diff-u="4" from="dependencies" to="english"?>
    ```diff
    --- step1_base/pubspec.yaml
    +++ step2_use_package/pubspec.yaml
    @@ -5,4 +5,5 @@
     dependencies:
       flutter:
         sdk: flutter
       cupertino_icons: ^0.1.2
    +  english_words: ^3.1.0
    ```

 2. 안드로이드 스튜디오의 편집기 화면에서 pubspec 파일을 보는 동안 나타나는 **Packages get**를 클릭하세요.
    이렇게 하면 해당 패키지를 프로젝트로 가져옵니다.
    아래와 같은 메시지를 콘솔에서 확인할 수 있습니다:  

    ```terminal
    $ flutter packages get
    Running "flutter packages get" in startup_namer...
    Process finished with exit code 0
    ```

    또한 `Packages get`을 수행하면 `pubspec.lock`
    프로젝트로 가져온 모든 패키지 목록과 버전 번호를 포함하고 있는 
    `pubspec.lock` 파일도 자동으로 생성됩니다.

 3. `lib/main.dart`에서 새 패키지를 가져오세요:

    <?code-excerpt path-base="codelabs/startup_namer/step2_use_package"?>
    <?code-excerpt "lib/main.dart" title retain="/^import/" replace="/import.*?english.*/[!$&!]/g" indent-by="2"?>
    ```dart
      import 'package:flutter/material.dart';
      [!import 'package:english_words/english_words.dart';!]
    ```

    입력 할 때, 안드로이드 스튜디오가 import 할만한 라이브러리를 추천해줍니다.
    그런 다음 import 문자열이 회색으로 나타내어 해당 라이브러리가 (지금까지는) 사용되지 않았음을 알려줍니다. 

 4. "Hello World" 문자열 대신 English words를 사용하여 텍스트를 생성하세요: 

    <?code-excerpt path-base="codelabs/startup_namer"?>
    <?code-excerpt "{step1_base,step2_use_package}/lib/main.dart" from="class"?>
    ```diff
    --- step1_base/lib/main.dart
    +++ step2_use_package/lib/main.dart
    @@ -5,6 +6,7 @@
     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
    +    final wordPair = WordPair.random();
         return MaterialApp(
           title: 'Welcome to Flutter',
           home: Scaffold(
    @@ -12,7 +14,7 @@
               title: Text('Welcome to Flutter'),
             ),
             body: Center(
    -          child: Text('Hello World'),
    +          child: Text(wordPair.asPascalCase),
             ),
           ),
         );
    ```

    {{site.alert.note}}
      "Pascal case" ("upper camel case"라고도 알려져있음)는
      문자열에서 맨 처음 단어를 포함한 모든 단어의 첫번째 글자가 대문자로 시작하는 형식입니다.
      그래서, "uppercamelcase"는 "UpperCamelCase"가 됩니다.  
    {{site.alert.end}}

 5. 앱이 실행중이면, hot reload 버튼<i class="material-icons align-bottom">offline_bolt</i>을 사용하여 
    실행중인 앱을 업데이트하세요. 
    실행중인 앱에서 hot reload를 클릭할 때마다 혹은 프로젝트를 저장할 때마다, 랜덤하게 선택된 다른 단어 쌍을 볼 수 있을 것입니다.
    왜냐하면 MaterialApp이 랜더링 될 때마다 혹은 또는 Flutter Inspector에서 플랫폼을 전환할 때마다
    실행되는 build 메서드 안에서 단어 쌍을 생성하고 있기 때문입니다.

    {% indent %}
      {% include android-ios-figure-pair.md image="step2.png" alt="App at completion of second step" %}
    {% endindent %}

### 문제가 있나요?
{:.no_toc}

앱이 올바르게 동작하지 않는다면, 오타를 확인해보세요.
필요하다면, 아래 코드를 사용하여 다시 올바르게 동작하게 하세요. 

* [pubspec.yaml]({{code-url}}/startup_namer/step2_use_package/pubspec.yaml)
* [lib/main.dart]({{code-url}}/startup_namer/step2_use_package/lib/main.dart)

## 3단계: Stateful 위젯 추가하기

<?code-excerpt path-base="codelabs/startup_namer/step3_stateful_widget"?>

State<em>less</em> 위젯은 변경불가능<sup>immutable</sup>합니다. 
속성을 변경할 수 없습니다&mdash;모든 값이 final입니다.

<sup>State<em>ful</em> 위젯은 
위젯의 수명동안 변경될 수 있는 상태를 유지합니다.
Stateful 위젯은 최소 두 개 이상 클래스가 필요합니다: 
1) StatefulWidget 클래스가 
2) State 클래스
의 인스턴스를 생성합니다. 
StatefulWidget 클래스 그자체는 변경불가능합니다. 
하지만 State 클래스가 위젯의 수명동안 상태를 유지합니다.

이 단계에서는, Stateful 위젯 `RandomWords`를 추가하고, 
그 위젯에서 `State` 클래스인 `RandomWordsState`를 생성할 것입니다.
그런 다음 `RandomWords`를 기존 Stateless 위젯 `MyApp`의 자식으로 사용할 것입니다.  

 1. 최소한의 상태를 가지는 클래스를 생성하세요. `main.dart`의 하단에 아래 코드를 추가하세요:

    <?code-excerpt "lib/main.dart (RandomWordsState)" title region="RWS-class-only" plaster="// TODO Add build() method" indent-by="2"?>
    ```dart
      class RandomWordsState extends State<RandomWords> {
        // TODO Add build() method
      }
    ```

    `State<RandomWords>` 선언을 눈여겨보세요. 
    `RandomWords`로 지정된 제네릭으로 [State]({{site.api}}/flutter/widgets/State-class.html) 클래스를 사용하고 있습니다. 
    대부분의 앱 로직과 상태는 여기서 유지됩니다&mdash;`RandomWords` 위젯을 위해 상태를 보관합니다.
    이 클래스에 생성된 단어 쌍이 저장됩니다.
    사용자가 스크롤함에 따라 단어 쌍이 무한히 증가하고, 
    사용자가 하트 아이콘 스위치를 눌러 
    가장 좋아하는 단어 쌍을 지정할 수 있습니다([part 2]({{site.codelabs}}/codelabs/first-flutter-app-pt2)에서). 

    `RandomWordsState`는 `RandomWords` 클래스에 의존적입니다. 아래에서 추가할 것입니다.

 2. Stateful 위젯 `RandomWords`를 `main.dart`에 추가하세요.
    `RandomWords` 위젯은 상태 클래스를 만드는 것 이외에 별다른 일을 하지 않습니다:

    <?code-excerpt "lib/main.dart (RandomWords)" title indent-by="2"?>
    ```dart
      class RandomWords extends StatefulWidget {
        @override
        RandomWordsState createState() => new RandomWordsState();
      }
    ```

    상태 클래스를 추가한 후, IDE가 클래스에 build 메서드가 없다고 경고합니다.
    다음으로, 기본 메서드 build를 추가하고 
    단어 생성 코드를 `MyApp`에서 `RandomWordsState`로 옮겨 단어 쌍을 생성하도록 하세요.

 3. `build()` 메서드를 `RandomWordsState`에 추가하세요:

    <?code-excerpt "lib/main.dart (RandomWordsState)" title indent-by="2" replace="/(\n  )(.*)/$1[!$2!]/g"?>
    ```dart
      class RandomWordsState extends State<RandomWords> {
        [!@override!]
        [!Widget build(BuildContext context) {!]
        [!  final wordPair = WordPair.random();!]
        [!  return Text(wordPair.asPascalCase);!]
        [!}!]
      }
    ```

 4. 아래 diff에 표시된 변경사항처럼 `MyApp`에서 단어 생성 코드를 삭제하세요:

    <?code-excerpt path-base="codelabs/startup_namer"?>
    <?code-excerpt "{step2_use_package,step3_stateful_widget}/lib/main.dart" to="}"?>
    ```diff
    --- step2_use_package/lib/main.dart
    +++ step3_stateful_widget/lib/main.dart
    @@ -6,7 +6,6 @@
     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
    -    final wordPair = WordPair.random();
         return MaterialApp(
           title: 'Welcome to Flutter',
           home: Scaffold(
    @@ -14,8 +13,8 @@
               title: Text('Welcome to Flutter'),
             ),
             body: Center(
    -          child: Text(wordPair.asPascalCase),
    +          child: RandomWords(),
             ),
           ),
         );
       }
    ```

 5. 앱을 재시작하세요.
    앱은 이전에 동작하던 방식과 마찬가지로 
    hot reload 하거나 저장할 때마다 단어 쌍을 계속 보여줘야 합니다.   

{{site.alert.tip}}
  Hot reload 할 때 아래와 같은 경고문구가 나온다면, 앱을 재시작하는 게 좋습니다. 

  **Reloading...<br>
  Some program elements were changed during reload but did not run when
  the view was reassembled; you may need to restart the app (by pressing "R")
  for the changes to have an effect.**

  메시지가 잘못 표시된 것일 수 있지만,
  재시작하면 변경 사항이 확실하게 앱 UI에 반영됩니다.
{{site.alert.end}}


### 문제가 있나요?
{:.no_toc}

앱이 제대로 실행되지 않을 경우, 
아래 링크에 있는 코드 활용하여 다시 올바르게 동작하게 할 수 있습니다.

* [lib/main.dart]({{code-url}}/startup_namer/step3_stateful_widget/lib/main.dart)

## 4단계: 무한 스크롤 ListView 생성하기

<?code-excerpt path-base="codelabs/startup_namer/step4_infinite_list"?>

이 단계에서는, `RandomWordsState`를 확장하여 단어 쌍 목록을 생성하고 표시합니다.
`ListView` 위젯 안에 표시되는 목록이 사용자가 스크롤할 때마다 무한하게 늘어납니다. 
`ListView`의 `builder` 팩토리 생성자를 사용하면 필요에 따라 lazy한 방식으로 목록을 만듭니다.  

 1. 제안된 단어 쌍을 저장하기 위해 `RandomWordsState` 클래스에 `_suggestions` 목록을 추가하세요. 
    또한, 글자 크기를 키우기 위해 `_biggerFont` 변수를 추가하세요. 

    <?code-excerpt "lib/main.dart" title region="RWS-var" indent-by="2" replace="/final .*/[!$&!]/g"?>
    ```dart
      class RandomWordsState extends State<RandomWords> {
        [!final _suggestions = <WordPair>[];!]
        [!final _biggerFont = const TextStyle(fontSize: 18.0);!]
        // ···
      }
    ```

    {{site.alert.note}}
      다트 언어에서는 식별자 앞에 밑줄을 붙이면 
      [프라이빗 적용]({{site.dart-site}}/guides/language/language-tour)이 됩니다. 
    {{site.alert.end}}

    다음으로, `RandomWordsState` 클래스에 `_buildSuggestions()` 함수를 추가하세요. 
    이 메서드는 제안된 단어 쌍을 표시하는 `ListView`를 만듭니다.

    `ListView` 클래스는 builder 속성인 `itemBuilder`를 제공합니다. 
    이 팩토리 빌더는 익명 함수 형태의 콜백 함수를 받습니다.
    두 인자가 함수에 전달됩니다; `BuildContext`와 행 반복자 `i`입니다.
    반복자는 0부터 시작되고 제안된 모든 단어 쌍에 대해 각각 한 번씩 함수가 호출될 때마다 증가합니다.
    이 방식을 사용하여 사용자가 스크롤을 할 때마다 목록이 무한하게 증가할 수 있게 할 수 있습니다.

 2. `RandomWordsState` 클래스에 `_buildSuggestions()` 함수를 추가하세요: 

    <?code-excerpt "lib/main.dart (_buildSuggestions)" title indent-by="2"?>
    ```dart
      Widget _buildSuggestions() {
        return ListView.builder(
            padding: const EdgeInsets.all(16.0),
            itemBuilder: /*1*/ (context, i) {
              if (i.isOdd) return Divider(); /*2*/

              final index = i ~/ 2; /*3*/
              if (index >= _suggestions.length) {
                _suggestions.addAll(generateWordPairs().take(10)); /*4*/
              }
              return _buildRow(_suggestions[index]);
            });
      }
    ```

    {:.numbered-code-notes}
     1. `itemBuilder` 콜백은 단어 쌍이 제안될 때마다 호출되고 
        각각을 `ListTile` 행에 배치합니다.
        짝수 행인 경우 `ListTile` 행에 단어 쌍을 추가합니다.
        홀수 행인 경우 시각적으로 각 항목을 구분하는 `Divider` 위젯을 추가합니다. 
        작은 기기에서는 구분선을 보기 어려울 수 있습니다.
     2. `ListView`의 각 행 앞에 1 픽셀 높이의 구분선 위젯을 추가하십시오. 
     3. `i ~/ 2` 표현식은 `i`를 2로 나눈 뒤 정수 결과를 반환합니다.
        예를 들어: 1, 2, 3, 4, 5는 0, 1, 1, 2, 2가 됩니다.
        이렇게 하면 구분선 위젯을 제외한 `ListView`에 있는 단어 쌍 수가 계산됩니다. 
     4. 가능한 단어 쌍을 모두 사용하고 나면, 10개를 더 생성하고 제안 목록에 추가합니다.   

    `_buildSuggestions()` 함수는 단어 쌍 마다 한 번 씩 `_buildRow()`를 호출합니다. 
    이 함수는 `ListTile`에서 각각 새로운 쌍을 표시하여 다음 단계에서 행을 더 매력적으로 만들 수 있게 합니다.  

 3. `RandomWordsState`에 `_buildRow()`를 추가하세요:

    <?code-excerpt "lib/main.dart (_buildRow)" title indent-by="2"?>
    ```dart
      Widget _buildRow(WordPair pair) {
        return ListTile(
          title: Text(
            pair.asPascalCase,
            style: _biggerFont,
          ),
        );
      }
    ```

 4. `RandomWordsState` 클래스에서 `build()` 메서드를 변경하여 
    단어 생성 라이브러리를 직접 호출하지 말고 `_buildSuggestions()`을 사용하도록 하세요, 
    ([Scaffold]({{site.api}}/flutter/material/Scaffold-class.html)는 
    기본적인 머터리얼 디자인 시각 레이아웃을 구현합니다.)
    메서드의 본문을 아래 강조 표시된 코드로 교체하세요:

    <?code-excerpt "lib/main.dart (build)" title region="RWS-build" replace="/(\n  )(return.*|  .*|\);)/$1[!$2!]/g" indent-by="2"?>
    ```dart
      @override
      Widget build(BuildContext context) {
        [!return Scaffold(!]
        [!  appBar: AppBar(!]
        [!    title: Text('Startup Name Generator'),!]
        [!  ),!]
        [!  body: _buildSuggestions(),!]
        [!);!]
      }
    ```

 5. `MyApp` 클래스에서 `build()` 메서드를 수정하세요. 
    title을 변경하고 home을 `RandomWords`으로 변경하세요:

    <?code-excerpt path-base="codelabs/startup_namer"?>
    <?code-excerpt "{step3_stateful_widget,step4_infinite_list}/lib/main.dart" diff-u="4" from="class MyApp" to="}"?>
    ```diff
    --- step3_stateful_widget/lib/main.dart
    +++ step4_infinite_list/lib/main.dart
    @@ -6,15 +6,8 @@
     class MyApp extends StatelessWidget {
       @override
       Widget build(BuildContext context) {
         return MaterialApp(
    -      title: 'Welcome to Flutter',
    -      home: Scaffold(
    -        appBar: AppBar(
    -          title: Text('Welcome to Flutter'),
    -        ),
    -        body: Center(
    -          child: RandomWords(),
    -        ),
    -      ),
    +      title: 'Startup Name Generator',
    +      home: RandomWords(),
         );
       }
    ```

 6. Restart the app. You should see a list of word pairings no matter how far
    you scroll.

    {% indent %}
      {% include android-ios-figure-pair.md image="step4-infinite-list.png" alt="App at completion of fourth step" %}
    {% endindent %}

### 문제가 있나요?
{:.no_toc}

앱이 제대로 실행되지 않을 경우, 
아래 링크에 있는 코드 활용하여 다시 올바르게 동작하게 할 수 있습니다.

* [lib/main.dart]({{code-url}}/startup_namer/step4_infinite_list/lib/main.dart)

## 다음 단계
{:.no_toc}

{% include app-figure.md class="site-image-right" img-class="border"
    image="get-started/startup-namer.gif" caption="2부에서 만들 앱" %}

축하합니다!

iOS와 Android 모두에서 작동하는 인터랙티브한 Flutter 앱을 작성해보았습니다.
이 코드랩에서:

* Flutter 앱을 처음부터 만들었습니다.
* 다트 코드를 작성했습니다.
* 외부 서드파티 라이브러리를 활용했습니다.
* 빠른 개발 사이클을 위해 hot reload를 사용했습니다.
* Stateful 위젯을 적용했습니다.
* Lazy한 방식 무한 스크롤 목록을 만들었습니다. 

이 앱을 확장하고 싶다면, 
[구글 개발자 코드랩]({{site.codelabs}}) 사이트에서,
[2부]({{site.codelabs}}/codelabs/first-flutter-app-pt2)를 진행하세요.
아래 기능을 추가하게 될 것입니다:

* 클릭 가능한 하트 아이콘을 추가하여
  가장 좋아하는 단어 쌍을 저장하고 앱을 인터랙티브하게 만들어봅니다. 
* 가장 좋아하는 단어를 보관하는 새로운 화면과 경로를 추가하고
  그 화면으로 이동하는 내비게이션 기능을 구현해봅니다. 
* 테마 색을 수정하여 흰색 앱을 만들어봅니다.
