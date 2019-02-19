---
title: 플랫폼 별 코드 작성
short-title: 플랫폼 별 코드
---

이 가이드는 플랫폼 별 코드를 어떻게 작성하는지 설명합니다. 어떤 플랫폼 별 기능은
이미 있는 패키지를 통해 사용이 가능합니다. [using packages](/docs/development/packages-and-plugins/using-packages) 를 참조하세요.

Flutter는 안드로이드에서의 Java와 Kotlin, iOS에서의 ObjectiveC 와 Swift 에서 가능한 플랫폼 별 api를
사용하게 해주는 유연한 시스템을 사용합니다.

Fluttr의 플랫폼 별 API는 코드 생성에 의존하고 있지 않고, 유연한 메시지 전달 스타일을 사용합니다.


* 앱의 Flutter 부분은 플랫폼 채널을 통해서 iOS 나 Android 가 될 수 있는 *호스트* 에게 메시지를 보냅니다.

* *호스트* 는 플랫폼 채널에서 메시지를 받습니다. 그리고 플랫폼 네이티브 언어를 사용해서
몇 개 던지 플랫폼 별 APIs 를 호출하고, Flutter 부분인 *클라이언트* 에게 응답을 보냅니다.

## 아키텍쳐 훝어 보기: 플랫폼 채널 {#architecture}

클라이언트(UI)와 호스트(플랫폼)이 플랫폼 채널에서 사용하는 메시지 형태는 아래 다이어그램에 그려져있습니다.

![Platform channels architecture](/images/PlatformChannels.png)

메시지와 응답은 반응성 좋은 사용자 인터페이스를 위해 비동기적으로 전달됩니다.

클라이언트 쪽에서는, `MethodChannel` ([API][MethodChannel])이 메시지를 그에 상응하는
메소드로 보낼 수 있도록 해줍니다. 플랫폼 쪽에서는, 안드로이드는 `MethodChannel`([API][MethodChannelAndroid]), iOS는 `FlutterMethodChannel`
([API][MethodChanneliOS])들이 메시지를 받는 것과 응답을 가능하게 합니다. 이 클래스들은 아주 적은
코드만으로도 플랫폼 플러그인을 개발할 수 있게 해줍니다.

*Note*: 원한다면 메소드 호출은 반대의 방향으로도 보내질 수 있습니다.
(Dart로 구현된 메소드의 클라이언트 역할을 하는 플랫폼일때) 예시는 이 플러그인([`quick_actions`](https://pub.dartlang.org/packages/quick_actions))을 봐주세요.

[MethodChannel]: https://docs.flutter.io/flutter/services/MethodChannel-class.html
[MethodChannelAndroid]: https://docs.flutter.io/javadoc/io/flutter/plugin/common/MethodChannel.html
[MethodChanneliOS]: https://docs.flutter.io/objcdoc/Classes/FlutterMethodChannel.html

### 플랫폼 채널 지원 데이터형과 코덱 {#codec}

표준 플랫폼 채널은 간단한 json 형태의 효율적인 바이너리 직렬화를 지원하는 boolean, numbers, Strings,
byte butters, List, Map등의 표준 메시지 코덱을 사용합니다 (참고: [`StandardMessageCodec`](https://docs.flutter.io/flutter/services/StandardMessageCodec-class.html)).
이 값들에 대한 메시지 직렬화와 역직렬화는 당신이 값을 보내고 받을 때 자동으로 이루어집니다.

아래 표는 dart의 자료형이 플랫폼에서 어떻게 받고 보내지는지 보여줍니다.

| Dart        | Android             | iOS
|-------------|---------------------|----
| null        | null                | nil (NSNull when nested)
| bool        | java.lang.Boolean   | NSNumber numberWithBool:
| int         | java.lang.Integer   | NSNumber numberWithInt:
| int, if 32 bits not enough | java.lang.Long | NSNumber numberWithLong:
| double      | java.lang.Double    | NSNumber numberWithDouble:
| String      | java.lang.String    | NSString
| Uint8List   | byte[]   | FlutterStandardTypedData typedDataWithBytes:
| Int32List   | int[]    | FlutterStandardTypedData typedDataWithInt32:
| Int64List   | long[]   | FlutterStandardTypedData typedDataWithInt64:
| Float64List | double[] | FlutterStandardTypedData typedDataWithFloat64:
| List        | java.util.ArrayList | NSArray
| Map         | java.util.HashMap   | NSDictionary

<br>
## 예시: 플랫폼 채널을 이용해서 iOS와 안드로이드 코드 호출하기 {#example}

아래에선 현재 배터리 레벨를 표시하기 위해 플랫폼 특화 API를 사용하는 법을 설명합니다. 안드로이드의 `BatteryManager`,
iOS의 `device.batteryLevel` API 를 `getBatteryLevel` 이라는 단일 플랫폼 메시지로 사용합니다.

해당 예시는 메인 앱 자체에 플랫폼 별 코드를 추가합니다. 만약 당신이 다양한 앱에서 플랫폼 특화 코드를 재사용하고
싶다면, 프로젝트 시작 방법이 약간 다릅니다([developing
packages](/docs/development/packages-and-plugins/developing-packages#plugin) 참고).
하지만 플랫폼 채널 코드는 여전히 같은 방법으로 작성됩니다.

*Note*: 이 예시의 실행가능한 전체 코드는 여기서 확인할 수 있습니다. [`/examples/platform_channel/`](https://github.com/flutter/flutter/tree/master/examples/platform_channel)

안드로이드는 Java로 되어 있고 iOS 는 Objective-C 로 되어있습니다. Swift 와 iOS에 대한 예제는 다음을
참고하세요.[`/examples/platform_channel_swift/`](https://github.com/flutter/flutter/tree/master/examples/platform_channel_swift)

### Step 1: 새로운 앱 프로젝트 만들기 {#example-project}

새로운 앱을 만들어서 시작하세요.

* 터미널에서 실행: `flutter create batterylevel`

기본적으로 안드로이드는 Java, iOS는 Objective-C를 사용해서 작성하는 템플릿으로 지원됩니다. Kotlin 이나 Swift를
사용하려면 `-i` 혹은 `-a` 플래그를 사용하세요.

* 터미널에서 실행: `flutter create -i swift -a kotlin batterylevel`

### Step 2: Flutter 플랫폼 클라이언트 생성 {#example-client}

앱의 `State` 클래스가 현재 앱 상태를 저장합니다. 현재 배터리 상태를 저장하려면 이를 상속해야 합니다.

먼저, 채널을 작성합니다. 우린 배터리 레벨을 반환하는 한개의 플랫폼 메소드를 가진 `MethodChannel`을 사용할
것입니다.


채널의 클라이언트와 호스트는 채널 생성자를 통해 전달된 채널 이름으로 연결됩니다. 하나의 앱에서 사용하는 모든
채널 이름은 유일해야 하기 때문에, 유일한 도메인 prefix를 사용해서 채널 이름 앞에 추가할 것을 권장합니다. 
예) `samples.flutter.io/battery`

<!-- skip -->
```dart
import 'dart:async';

import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
...
class _MyHomePageState extends State<MyHomePage> {
  static const platform = const MethodChannel('samples.flutter.io/battery');

  // 배터리 레벨을 가져옵니다.
}
```

다음으로, `getBatteryLevel` 구분자를 통해 특정 메소드를 지정해서 메소드 채널에서 메소드를 실행합니다.
호출은 실패할 수도 있습니다. 예를 들어 특정 플랫폼이 해당 API를 지원하지 않는 경우(시뮬레이터에서의 실행 등)가 있습니다.
그러므로 `invokeMethod` 호출을 try-catch 문으로 감싸주세요.


반환된 결과를 `setState` 함수 안에서 사용해서 `_batteryLevel` 변수가 들고있는 UI 상태를 업데이트 하세요.

<!-- skip -->
```dart
  // 배터리 레벨을 가져옵니다.
  String _batteryLevel = 'Unknown battery level.';

  Future<void> _getBatteryLevel() async {
    String batteryLevel;
    try {
      final int result = await platform.invokeMethod('getBatteryLevel');
      batteryLevel = 'Battery level at $result % .';
    } on PlatformException catch (e) {
      batteryLevel = "Failed to get battery level: '${e.message}'.";
    }

    setState(() {
      _batteryLevel = batteryLevel;
    });
  }
```


마지막으로, 기존 템플릿의 `build` 메소드를  작은 UI (문자열로 배터리 레벨 표시를 하기 위함)와 값을 새로고침하기 위한 버튼을 생성하는 코드로 바꾸세요.

<!-- skip -->
```dart
@override
Widget build(BuildContext context) {
  return Material(
    child: Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        children: [
          RaisedButton(
            child: Text('Get Battery Level'),
            onPressed: _getBatteryLevel,
          ),
          Text(_batteryLevel),
        ],
      ),
    ),
  );
}
```


### Step 3a : Java를 이용한 안드로이드 플랫폼 구현 {#example-java}

*Note*: 해당 단계는 Java를 사용합니다. 코틀린을 선호한다면, Step 3b 로 넘어가세요.


안드로이드 스튜디오에서 안드로이드 부분을 열어서 시작하세요:

1. 안드로이드 스튜디오 실행

1. 메뉴에서 'File > Open...' 선택

1. Flutter 앱이 있는 디렉토리로 이동해서, 그 안의 `android` 폴더를 선택합니다. OK를 클릭합니다.

1. 프로젝트 뷰에서 `java` 폴더 안에 있는 `MainActivity.java` 를 엽니다.


다음으로, `MethodChannel` 을 만들고 `onCreate` 메소드 안에서 `MethodCallHandler` 를 설정합니다.
Flutter 클라이언트 측과 같은 채널 이름이 사용되었는지 확인해주세요.

```java
import io.flutter.app.FlutterActivity;
import io.flutter.plugin.common.MethodCall;
import io.flutter.plugin.common.MethodChannel;
import io.flutter.plugin.common.MethodChannel.MethodCallHandler;
import io.flutter.plugin.common.MethodChannel.Result;

public class MainActivity extends FlutterActivity {
    private static final String CHANNEL = "samples.flutter.io/battery";

    @Override
    public void onCreate(Bundle savedInstanceState) {

        super.onCreate(savedInstanceState);
        GeneratedPluginRegistrant.registerWith(this);

        new MethodChannel(getFlutterView(), CHANNEL).setMethodCallHandler(
                new MethodCallHandler() {
                    @Override
                    public void onMethodCall(MethodCall call, Result result) {
                        // TODO
                    }
                });
    }
}
```


다음으로, 배터리 레벨을 가져오기 위해 안드로이드 배터리 API를 사용하는 실제  안드로이드 Java 코드를 추가합니다. 해당 코드는
네이티브 안드로이드 앱에서 사용하던 것과 완전히 같습니다.

첫번째로, 필요한 import 들을 파일 상단에 추가해주세요.

```
import android.content.ContextWrapper;
import android.content.Intent;
import android.content.IntentFilter;
import android.os.BatteryManager;
import android.os.Build.VERSION;
import android.os.Build.VERSION_CODES;
import android.os.Bundle;
```


그리고 아래 코드를 액티비티 클래스의 `onCreate` 메소드 아래에 새로운 메소드로 작성해주세요.

```java
private int getBatteryLevel() {
  int batteryLevel = -1;
  if (VERSION.SDK_INT >= VERSION_CODES.LOLLIPOP) {
    BatteryManager batteryManager = (BatteryManager) getSystemService(BATTERY_SERVICE);
    batteryLevel = batteryManager.getIntProperty(BatteryManager.BATTERY_PROPERTY_CAPACITY);
  } else {
    Intent intent = new ContextWrapper(getApplicationContext()).
        registerReceiver(null, new IntentFilter(Intent.ACTION_BATTERY_CHANGED));
    batteryLevel = (intent.getIntExtra(BatteryManager.EXTRA_LEVEL, -1) * 100) /
        intent.getIntExtra(BatteryManager.EXTRA_SCALE, -1);
  }

  return batteryLevel;
}
```


마지막으로, 먼저 추가한 `onMethodCall` 메소드를 완성합니다. 사용할 플랫폼 메소드는 `getBatteryLevel` 이며, 이는 `call` 매개변수 안에서 가져와 테스트 해볼 수 있습니다.  이 플랫폼 메소드는 단순히 이전 단계에서 작성한 안드로이드 코드를 호출합니다.
그리고 `response` 매개변수를 통해 성공과 에러를 응답으로 돌려줍니다. 만약 알수 없는 메소드가 호출된다면, 기록 해놓으세요.

아래 코드를:

```java
public void onMethodCall(MethodCall call, Result result) {
    // TODO
}
```

이렇게 변경하세요:

```java
@Override
public void onMethodCall(MethodCall call, Result result) {
    if (call.method.equals("getBatteryLevel")) {
        int batteryLevel = getBatteryLevel();

        if (batteryLevel != -1) {
            result.success(batteryLevel);
        } else {
            result.error("UNAVAILABLE", "Battery level not available.", null);
        }
    } else {
        result.notImplemented();
    }
}
```


이제 안드로이드에서 앱을 실행하는것이 가능합니다. 만약 안드로이드 에뮬레이터를 사용하고 있다면 , 툴바의 `...`
버튼에서 제어판을 열어서 배터리 레벨을 설정할 수 있습니다.


### Step 3b : Kotlin을 사용해서 안드로이드 플랫폼 구현 추가 {#example-kotlin}


*Note*: 이 단계는 Java 말고 Kotlin을 사용하는것 외엔 3a와 비슷합니다.


이 과정은 [step 1.](#example-project) 으로 프로젝트를 생성할 때 `-a kotlin` 옵션을 사용했다고
가정합니다.


안드로이드 스튜디오에서 Flutter 앱의 안드로이드 부분을 열어서 시작하세요:

1. 안드로이드 스튜디오 실행

1. 메뉴에서 'File > Open...' 선택

1. Flutter 앱이 있는 디렉토리로 이동해서, 그 안의 `android` 폴더를 선택합니다. OK를 클릭합니다.

1. 프로젝트 뷰에서 `kotlin` 폴더 안에 있는 `MainActivity.kt` 파일을 열어주세요. (참고: 만약 안드로이드 스튜디오
  2.3 버전을 사용한다면, 'kotlin' 폴더가 'java' 폴더로 보이는 것에 유의하세요.)


다음으로, `onCreate` 메소드 안에서 `MethodChannel`을 만들고 `setMethodCallHandler` 를 호출하세요.
Flutter 클라이언트 쪽에서 사용한 것과 같은 채널 이름을 사용했는지 확인해주세요.

```kotlin
import android.os.Bundle
import io.flutter.app.FlutterActivity
import io.flutter.plugin.common.MethodChannel

class MainActivity() : FlutterActivity() {
  private val CHANNEL = "samples.flutter.io/battery"

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    GeneratedPluginRegistrant.registerWith(this)
    MethodChannel(flutterView, CHANNEL).setMethodCallHandler { call, result ->
      // TODO
    }
  }
}
```


다음으로, 배터리 레벨을 가져오기 위해 안드로이드 배터리 API를 사용하는 실제  안드로이드 Kotlin 코드를 추가합니다. 해당 코드는
네이티브 안드로이드 앱에서 사용하던 것과 완전히 같습니다.

첫번째로, 필요한 import 들을 파일 상단에 추가해주세요.

```
import android.content.Context
import android.content.ContextWrapper
import android.content.Intent
import android.content.IntentFilter
import android.os.BatteryManager
import android.os.Build.VERSION
import android.os.Build.VERSION_CODES
```

그리고 아래 코드를 액티비티 클래스의 `onCreate` 메소드 아래에 새로운 메소드로 작성해주세요.


```kotlin
  private fun getBatteryLevel(): Int {
    val batteryLevel: Int
    if (VERSION.SDK_INT >= VERSION_CODES.LOLLIPOP) {
      val batteryManager = getSystemService(Context.BATTERY_SERVICE) as BatteryManager
      batteryLevel = batteryManager.getIntProperty(BatteryManager.BATTERY_PROPERTY_CAPACITY)
    } else {
      val intent = ContextWrapper(applicationContext).registerReceiver(null, IntentFilter(Intent.ACTION_BATTERY_CHANGED))
      batteryLevel = intent!!.getIntExtra(BatteryManager.EXTRA_LEVEL, -1) * 100 / intent.getIntExtra(BatteryManager.EXTRA_SCALE, -1)
    }

    return batteryLevel
  }
```


마지막으로, 먼저 추가한 `onMethodCall` 메소드를 완성합니다. 사용할 플랫폼 메소드는 `getBatteryLevel` 이며, 이는 `call` 매개변수 안에서 가져와 테스트 해볼 수 있습니다.  이 플랫폼 메소드는 단순히 이전 단계에서 작성한 안드로이드 코드를 호출합니다.
그리고 `response` 매개변수를 통해 성공과 에러를 응답으로 돌려줍니다. 만약 알수 없는 메소드가 호출된다면, 기록 해놓으세요.

아래 코드를:


```kotlin
    MethodChannel(flutterView, CHANNEL).setMethodCallHandler { call, result ->
      // TODO
    }
```

이렇게 변경하세요:

```kotlin
    MethodChannel(flutterView, CHANNEL).setMethodCallHandler { call, result ->
      if (call.method == "getBatteryLevel") {
        val batteryLevel = getBatteryLevel()

        if (batteryLevel != -1) {
          result.success(batteryLevel)
        } else {
          result.error("UNAVAILABLE", "Battery level not available.", null)
        }
      } else {
        result.notImplemented()
      }
    }
```

이제 안드로이드에서 앱을 실행하는것이 가능합니다. 만약 안드로이드 에뮬레이터를 사용하고 있다면 , 툴바의 `...`
버튼에서 제어판을 열어서 배터리 레벨을 설정할 수 있습니다.


### Step 4a : Objective-C를 이용해서 iOS 플랫폼 특화 구현 추가 {#example-objc}


*Note*: 해당 단계는 Objective-C 를 사용합니다. Swift를 선호한다면, step 4b로 건너뛰세요.

Xcode에서 Flutter 앱의 iOS 호스트 부분을 열어서 시작하세요:

1. Xcode 실행

1. 메뉴에서 'File > Open...' 선택

1. Flutter 앱이 위치한 디렉토리고 이동하고, 그 안의 `ios` 폴더를 선택합니다. OK 를 클릭합니다.

1. Xcode 프로젝트가 에러 없이 빌드되는지 확인하세요.

1. 프로젝트 네비게이터에서 Runner > Runner 에 있는 `AppDelegate.m` 파일을 엽니다.


다음으로, `FlutterMethodChannel`을 만들고 `applicationDidFinishLaunchingWithOptions:` 메소드 안에 핸들러를 추가해주세요. Flutter 클라이언트 측과 같은 채널 이름이 사용되었는지 확인해주세요.


```objectivec
#import <Flutter/Flutter.h>
#import "GeneratedPluginRegistrant.h"

@implementation AppDelegate
- (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions {
  FlutterViewController* controller = (FlutterViewController*)self.window.rootViewController;

  FlutterMethodChannel* batteryChannel = [FlutterMethodChannel
                                          methodChannelWithName:@"samples.flutter.io/battery"
                                          binaryMessenger:controller];

  [batteryChannel setMethodCallHandler:^(FlutterMethodCall* call, FlutterResult result) {
    // TODO
  }];

  [GeneratedPluginRegistrant registerWithRegistry:self];
  return [super application:application didFinishLaunchingWithOptions:launchOptions];
}
```

다음으로, 배터리 레벨을 가져오기 위해 iOS 배터리 API를 사용하는 실제  iOS ObjectiveC 코드를 추가합니다. 해당 코드는
네이티브 iOS 앱에서 사용하던 것과 완전히 같습니다.

`AppDelegate` 클래스에서 `@end` 바로 전에 아래 코드를 새로운 메소드로 추가해주세요.

```objectivec
- (int)getBatteryLevel {
  UIDevice* device = UIDevice.currentDevice;
  device.batteryMonitoringEnabled = YES;
  if (device.batteryState == UIDeviceBatteryStateUnknown) {
    return -1;
  } else {
    return (int)(device.batteryLevel * 100);
  }
}
```

마지막으로, 먼저 추가한 `setMethodCallHandler` 메소드를 완성합니다. 사용할 플랫폼 메소드는 `getBatteryLevel` 이며, 이는 `call` 매개변수 안에서 가져와 테스트 해볼 수 있습니다.  이 플랫폼 메소드는 단순히 이전 단계에서 작성한 iOS 코드를 호출합니다.
그리고 `response` 매개변수를 통해 성공과 에러를 응답으로 돌려줍니다. 만약 알수 없는 메소드가 호출된다면, 기록 해놓으세요.

```objectivec
__weak typeof(self) weakSelf = self
[batteryChannel setMethodCallHandler:^(FlutterMethodCall* call, FlutterResult result) {
  if ([@"getBatteryLevel" isEqualToString:call.method]) {
    int batteryLevel = [weakSelf getBatteryLevel];

    if (batteryLevel == -1) {
      result([FlutterError errorWithCode:@"UNAVAILABLE"
                                 message:@"Battery info unavailable"
                                 details:nil]);
    } else {
      result(@(batteryLevel));
    }
  } else {
    result(FlutterMethodNotImplemented);
  }
}];
```

이제 iOS 에서 앱을 실행하는것이 가능합니다.  만약 iOS 시뮬레이터를 사용하고 있다면, 배터리 API 가 지원되지 않음을 알아두세요. (앱에서는 'battery info unavailable' 이라는 메시지를 보여줄 것입니다.)

### Step 4b: Add an iOS platform-specific implementation using Swift {#example-swift}

*Note*: 아래 단계는 Objective-C 말고 Swift를 사용한다는 외에는 step 4a 와 비슷합니다. 

이 과정은 [step 1.](#example-project) 으로 프로젝트를 생성할 때 `-i swift` 옵션을 사용했다고
가정합니다.

Xcode에서 Flutter 앱의 iOS 호스트 부분을 열어서 시작하세요:

1. Xcode 실행

1. 메뉴에서 'File > Open...' 선택

1. Flutter 앱이 위치한 디렉토리고 이동하고, 그 안의 `ios` 폴더를 선택합니다. OK 를 클릭합니다.

다음으로, 기본 템플릿에 Objective-C를 사용하는 Swift 에 대한 지원을 추가하세요.

1. Expand Runner > Runner in the Project navigator.

1. 프로젝트 네비게이터에서 Runner > Runner을 여세요.

1. 프로젝트 네비게이터 안 Runner > Runner 에 위치한 `AppDelegate.swift` 파일을 여세요

다음으로,  `application:didFinishLaunchingWithOptions:` 함수를 오버라이드 하고  `samples.flutter.io/battery` 를 채널 이름으로 사용하는 `FlutterMethodChannel` 를 생성하세요. 


```swift
@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
  override func application(
    _ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {

    let controller : FlutterViewController = window?.rootViewController as! FlutterViewController
    let batteryChannel = FlutterMethodChannel(name: "samples.flutter.io/battery",
                                              binaryMessenger: controller)
    batteryChannel.setMethodCallHandler({
      (call: FlutterMethodCall, result: FlutterResult) -> Void in
      // 배터리 메시지를 처리
    })

    GeneratedPluginRegistrant.register(with: self)
    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
  }
}
```

다음으로, 배터리 레벨을 가져오기 위해 iOS 배터리 API를 사용하는 실제  iOS의 Swift 코드를 추가합니다. 해당 코드는
네이티브 iOS 앱에서 사용하던 것과 완전히 같습니다.

아래 코드를 `AppDelegate.swift`  맨 밑에 새로운 메소드로 추가하세요.

```swift
private func receiveBatteryLevel(result: FlutterResult) {
  let device = UIDevice.current
  device.isBatteryMonitoringEnabled = true
  if device.batteryState == UIDeviceBatteryState.unknown {
    result(FlutterError(code: "UNAVAILABLE",
                        message: "Battery info unavailable",
                        details: nil))
  } else {
    result(Int(device.batteryLevel * 100))
  }
}
```


마지막으로, 먼저 추가한 `setMethodCallHandler` 메소드를 완성합니다. 사용할 플랫폼 메소드는 `getBatteryLevel` 이며, 이는 `call` 매개변수 안에서 가져와 테스트 해볼 수 있습니다.  이 플랫폼 메소드는 단순히 이전 단계에서 작성한 iOS 코드를 호출합니다.
그리고 `response` 매개변수를 통해 성공과 에러를 응답으로 돌려줍니다. 만약 알수 없는 메소드가 호출된다면, 기록 해놓으세요.

```swift
batteryChannel.setMethodCallHandler({
  [weak self] (call: FlutterMethodCall, result: FlutterResult) -> Void in
  guard call.method == "getBatteryLevel" else {
    result(FlutterMethodNotImplemented)
    return
  }
  self.receiveBatteryLevel(result: result)
})
```

이제 iOS 에서 앱을 실행하는것이 가능합니다.  만약 iOS 시뮬레이터를 사용하고 있다면, 배터리 API 가 지원되지 않음을 알아두세요. (앱에서는 'battery info unavailable' 이라는 메시지를 보여줄 것입니다.)

## 플랫폼 별 코드를 UI 코드에서 분리하기 {#separate}

만약 당신의 플랫폼 별 코드를 다양한 Flutter 앱에서 사용할 것 같다면, 메인 어플리케이션 바깥 디렉토리에 플랫폼 플러그인으로 분리하는것이 유용합니다. 자세한 내용은 [developing
packages](/docs/development/packages-and-plugins/developing-packages)를 참고하세요


## 플랫폼 특화 코드를 패키지로 배포하기 {#publish}


만약 당신의 플랫폼 특화 코드를 Flutter 생태계의 다른 개발자들과 공유하고 싶다면, [publishing
packages](/docs/development/packages-and-plugins/developing-packages#publish) 를 참고해주세요.


## 커스텀 채널과 코덱

위에 언급한 `MethodChannel` 외에도, 더 기본적이며 비동기적인 커스텀 메시지 코덱을 사용해서 메시지를 전달하는
[`BasicMessageChannel`][BasicMessageChannel] 를 사용할 수도 있습니다. 또한 특화된 [`BinaryCodec`][BinaryCodec], [`StringCodec`][StringCodec],
[`JSONMessageCodec`][JSONMessageCodec] 클래스들 을 사용하거나, 직접 코덱을 만드세요.

[BasicMessageChannel]: https://docs.flutter.io/flutter/services/BasicMessageChannel-class.html
[BinaryCodec]: https://docs.flutter.io/flutter/services/BinaryCodec-class.html
[StringCodec]: https://docs.flutter.io/flutter/services/StringCodec-class.html
[JSONMessageCodec]: https://docs.flutter.io/flutter/services/JSONMessageCodec-class.html
