---
title: Writing custom platform-specific code
short-title: Platform-specific code
---

This guide describes how to write custom platform-specific code. Some
platform-specific functionality is available through existing packages;
please see [using packages](/docs/development/packages-and-plugins/using-packages).

이 가이드는 플랫폼 특화된 코드를 어떻게 작성하는지 설명합니다. 어떤 플랫폼 특화 기능은
이미 있는 패키지를 통해 사용이 가능합니다.
[using packages](/docs/development/packages-and-plugins/using-packages) 를 봐주세요.

Flutter uses a flexible system that allows you to call platform-specific APIs
whether available in Java or Kotlin code on Android, or in ObjectiveC or Swift
code on iOS.

플러터는 안드로이드에서의 자바와 코틀린, iOS에서의 ObjectiveC 와 Swift 에서 가능한 플랫폼 특화 api를
사용하게 해주는 유연한 시스템을 사용합니다.

Flutter's platform-specific API support does not rely on code generation, but
rather on a flexible message passing style:

플러터의 플랫폼 특화 API는 코드 생성에 의존하고 있지 않고, 유연한 메시지 전달 스타일을 사용합니다.

* The Flutter portion of your app sends messages to its *host*, the iOS or
 Android portion of your app, over a platform channel.

 * 당신의 앱의 플러터 부분?은 플랫폼 채널을 사용해서 iOS 나 Android 가 될 수 있는 *호스트* 에게 메시지를 보냅니다.

* The *host* listens on the platform channel, and receives the message. It then
 calls into any number of platform-specific APIs&mdash;using the native
 programming language&mdash;and sends back a response to the *client*, the Flutter
 portion of your app.

* *호스트* 는 플랫폼 채널에서 메시지를 받아 처리합니다. 그리고 어떤 플랫폼 네이티브 언어를 사용해서
특화 APIs 를 호출하고, 플러터 부분인 *클라이언트* 에게 응답을 보냅니다.

## Architectural overview: platform channels {#architecture}
## 아키텍쳐: 플랫폼 채널 {#architecture}

Messages are passed between the client (UI) and host (platform) using platform
channels as illustrated in this diagram:

클라이언트(UI)와 호스트(플랫폼)이 플랫폼 채널에서 사용하는 메시지는 아래 다이어그램에 그려져있습니다.:

![Platform channels architecture](/images/PlatformChannels.png)

Messages and responses are passed asynchronously, to ensure the user interface
remains responsive.

메시지와 응답은 반응성 좋은 사용자 인터페이스를 비동기적으로 전달됩니다.

On the client side, `MethodChannel` ([API][MethodChannel]) enables sending
messages that correspond to method calls. On the platform side, `MethodChannel`
on Android ([API][MethodChannelAndroid]) and `FlutterMethodChannel` on iOS
([API][MethodChanneliOS]) enable receiving method calls and sending back a
result. These classes allow you to develop a platform plugin with very little
'boilerplate' code.

클라이언트 측면에서는, `MethodChannel` ([API][MethodChannel])이 메시지를 그에 상응하는
메소드로 보낼 수 있도록 가능하게 합니다. 플랫폼 측면에서, 안드로이드에서는 `MethodChannel`([API][MethodChannelAndroid]), iOS에서는 `FlutterMethodChannel`
([API][MethodChanneliOS])들이 메시지를 받고 응답을 전달 가능하게 해줍니다. 이 클래스들은 아주 적은
코드만으로도 플랫폼 플러그인을 개발할 수 있게 해줍니다.

*Note*: If desired, method calls can also be sent in the reverse direction, with
the platform acting as client to methods implemented in Dart. A concrete example
of this is the [`quick_actions`](https://pub.dartlang.org/packages/quick_actions) plugin.

*Note*: 원한다면 메소드 호출은 반대의 방향으로도 보내질 수 있습니다.
(플랫폼이 다트로 구현된 클라이언트로서 동작한다면?) 예시는 이 플러그인을 봐주세요.

[MethodChannel]: https://docs.flutter.io/flutter/services/MethodChannel-class.html
[MethodChannelAndroid]: https://docs.flutter.io/javadoc/io/flutter/plugin/common/MethodChannel.html
[MethodChanneliOS]: https://docs.flutter.io/objcdoc/Classes/FlutterMethodChannel.html

### Platform channel data types support and codecs {#codec}
### 플랫폼 채널 지원 데이터형과 코덱 {#codec}

The standard platform channels use a standard message codec that supports
efficient binary serialization of simple JSON-like values, such as booleans,
numbers, Strings, byte buffers, and List and Maps of these (see
[`StandardMessageCodec`](https://docs.flutter.io/flutter/services/StandardMessageCodec-class.html))
for details). The serialization and deserialization of these values to and from
messages happens automatically when you send and receive values.

표준 플랫폼 채널은 간단한 json 형태의 효율적인 바이너리 직렬화를 지원하는 boolean, numbers, Strings,
byte butters, List, Map등ㅢ 표준 메시지 코덱을 사용합니다. (참고 [`StandardMessageCodec`](https://docs.flutter.io/flutter/services/StandardMessageCodec-class.html))
이 값들에 대한 메시지 직렬화와 역직렬화는 당신이 값을 보내고 받을 때 자동으로 이루어집니다.

The following table shows how Dart values are received on the platform side and vice versa:
아래 표는 dart의 자료형이 플랫폼에서 어떻게 받아지는지(처리되는지) 보여줍니다.

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
## Example: Calling platform-specific iOS and Android code using platform channels {#example}
## 예시: 플랫폼 채널을 이용해서 iOS와 안드로이드 코드 호출하기 {#example}

The following demonstrates how to call a platform-specific API to retrieve and
display the current battery level. It uses the Android `BatteryManager` API, and
the iOS `device.batteryLevel` API, via a single platform message,
`getBatteryLevel`.

아래 예시는 현재 배터리 단계를 표시하기 위해 플랫폼 특화 API를 사용하는 법을 보여줍니다. 안드로이드의 `BatteryManager`,
iOS의 `device.batteryLevel` API 를 `getBatteryLevel` 이라는 단일 플랫폼 메시지로 사용합니다.

The example adds the platform-specific code inside the main app itself. If you
want to reuse the platform-specific code for multiple apps, the project creation
step is slightly different (see [developing
packages](/docs/development/packages-and-plugins/developing-packages#plugin)),
but the platform channel code is still written in the same way.

해당 예시는 메인 앱 자체에 플랫폼 특화 코드를 추가합니다. 만약 당신이 다양한 앱에서 플랫폼 특화 코드를 재사용하고
싶다면, 프로젝트 시작 방법이 약간 다릅니다. ([developing
packages](/docs/development/packages-and-plugins/developing-packages#plugin) 참고)
하지만 플랫폼 채널 코드는 여전히 같은 방법으로 작성됩니다.

*Note*: The full, runnable source-code for this example is available in
[`/examples/platform_channel/`](https://github.com/flutter/flutter/tree/master/examples/platform_channel)

*Note*: 이 예시의 실행가능한 전체 코드는 여기서 확인할 수 있습니다. [`/examples/platform_channel/`](https://github.com/flutter/flutter/tree/master/examples/platform_channel)

for Android with Java and iOS with Objective-C. For iOS with Swift, see
[`/examples/platform_channel_swift/`](https://github.com/flutter/flutter/tree/master/examples/platform_channel_swift).

안드로이드는 Java로 되어 있고 iOS 는 Objective-C 로 되어있습니다. Swift 와 iOS에 대한 예제는 다음을
참고하세요.[`/examples/platform_channel_swift/`](https://github.com/flutter/flutter/tree/master/examples/platform_channel_swift)

### Step 1: Create a new app project {#example-project}
### 1단계: 새로운 앱 프로젝트 만들기 {#example-project}

Start by creating a new app:

새로운 앱을 만들기:

* In a terminal run: `flutter create batterylevel`

* 터미널에서 실행: `flutter create batterylevel`

By default our template supports writing Android code using Java, or iOS code
using Objective-C. To use Kotlin or Swift, use the `-i` and/or `-a` flags:

안드로이드는 Java, iOS는 Objective-C를 사용해서 작성하는 템플릿으로  기본 지원됩니다. kotlin 이나 Swift를
사용하려면 `-i` 혹은 `-a` 플래그를 사용하세요.

* In a terminal run: `flutter create -i swift -a kotlin batterylevel`

* 테미널에서 실행: `flutter create -i swift -a kotlin batterylevel`

### Step 2: Create the Flutter platform client {#example-client}

### 2단계: 플러터 플랫폼 클라이언트 생성 {#example-client}

The app's `State` class holds the current app state. We need to extend that to
hold the current battery state.

앱의 `State` 클래스가 현재 앱의 상태를 저장합니다. 현재 배터리 상태를 저장하려면 이를 확장해야 합니다.

First, we construct the channel. We use a `MethodChannel` with a single
platform method that returns the battery level.

먼저, 채널을 작성합니다. 우린 배터리 레벨을 반환하는 한개의 플랫폼 메소드를 가진 `MethodChannel`을 사용할 
것입니다.

The client and host sides of a channel are connected through a channel name
passed in the channel constructor. All channel names used in a single app must
be unique; we recommend prefixing the channel name with a unique 'domain
prefix', e.g. `samples.flutter.io/battery`.

<!-- skip -->
```dart
import 'dart:async';

import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
...
class _MyHomePageState extends State<MyHomePage> {
  static const platform = const MethodChannel('samples.flutter.io/battery');

  // Get battery level.
}
```

Next, invoke a method on the method channel, specifying the concrete method
to call via the String identifier `getBatteryLevel`. The call may fail&mdash;for
example if the platform does not support the platform API (such as when running
in a simulator), so wrap the `invokeMethod` call in a try-catch statement.

Use the returned result to update the user interface state in `_batteryLevel`
inside `setState`.

<!-- skip -->
```dart
  // Get battery level.
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

Finally, replace the `build` method from the template to contain a small user
interface that displays the battery state in a string, and a button for
refreshing the value.

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


### Step 3a: Add an Android platform-specific implementation using Java {#example-java}

*Note*: The following steps use Java. If you prefer Kotlin, skip to step
3b.

Start by opening the Android host portion of your Flutter app in Android Studio:

1. Start Android Studio

1. Select the menu item 'File > Open...'

1. Navigate to the directory holding your Flutter app, and select the `android`
folder inside it. Click OK.

1. Open the file `MainActivity.java` located in the `java` folder in the Project
view.

Next, create a `MethodChannel` and set a `MethodCallHandler` inside the
`onCreate` method. Make sure to use the same channel name as was used on the
Flutter client side.

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

Next, we add the actual Android Java code that uses the Android battery APIs to
retrieve the battery level. This code is exactly the same as you would have
written in a native Android app.

First, add the needed imports at the top of the file:

```
import android.content.ContextWrapper;
import android.content.Intent;
import android.content.IntentFilter;
import android.os.BatteryManager;
import android.os.Build.VERSION;
import android.os.Build.VERSION_CODES;
import android.os.Bundle;
```

Then add the following as a new method in the activity class, below the `onCreate`
method:

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

Finally, complete the `onMethodCall` method added earlier. You need to
handle a single platform method, `getBatteryLevel`, so test for that in the
`call` argument. The implementation of this platform method simply calls the
Android code written in the previous step, and passes back a response for both
the success and error cases using the `response` argument. If an unknown method
is called, report that instead. Replace:

```java
public void onMethodCall(MethodCall call, Result result) {
    // TODO
}
```

with:

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

You should now be able to run the app on Android. If you are using the Android
Emulator, you can set the battery level in the Extended Controls panel
accessible from the `...` button in the toolbar.

### Step 3b: Add an Android platform-specific implementation using Kotlin {#example-kotlin}

*Note*: The following steps are similar to step 3a, only using Kotlin rather than
Java.

This step assumes that you created your project in [step 1.](#example-project)
using the `-a kotlin` option.

Start by opening the Android host portion of your Flutter app in Android Studio:

1. Start Android Studio

1. Select the menu item 'File > Open...'

1. Navigate to the directory holding your Flutter app, and select the `android`
   folder inside it. Click OK.

1. Open the file `MainActivity.kt` located in the `kotlin` folder in the Project
   view. (참고: If you are editing using Android Studio 2.3, note that the
   'kotlin' folder is shown as-if named 'java'.)

Next, inside the `onCreate` method, create a `MethodChannel` and call
`setMethodCallHandler`. Make sure to use the same channel name as was used on
the Flutter client side.

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

Next, add the actual Android Kotlin code that uses the Android battery APIs to
retrieve the battery level. This code is exactly the same as you would have
written in a native Android app.

First, add the needed imports at the top of the file:

```
import android.content.Context
import android.content.ContextWrapper
import android.content.Intent
import android.content.IntentFilter
import android.os.BatteryManager
import android.os.Build.VERSION
import android.os.Build.VERSION_CODES
```

Next, add the following as a new method in the `MainActivity` class, below the `onCreate`
method:

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

Finally, complete the `onMethodCall` method added earlier. You need to
handle a single platform method, `getBatteryLevel`, so test for that in the
`call` argument. The implementation of this platform method simply calls the
Android code written in the previous step, and passes back a response for both
the success and error cases using the `response` argument. If an unknown method
is called, report that instead. Replace:

```kotlin
    MethodChannel(flutterView, CHANNEL).setMethodCallHandler { call, result ->
      // TODO
    }
```

with:

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

You should now be able to run the app on Android. If you are using the Android
Emulator, you can set the battery level in the Extended Controls panel
accessible from the `...` button in the toolbar.

### Step 4a: Add an iOS platform-specific implementation using Objective-C {#example-objc}

*Note*: The following steps use Objective-C. If you prefer Swift, skip to step
4b.

Start by opening the iOS host portion of your Flutter app in Xcode:

1. Start Xcode

1. Select the menu item 'File > Open...'

1. Navigate to the directory holding your Flutter app, and select the `ios`
folder inside it. Click OK.

1. Make sure the Xcode projects builds without errors.

1. Open the file `AppDelegate.m` located under Runner > Runner in the Project
navigator.

Next, create a `FlutterMethodChannel` and add a handler inside the `application
didFinishLaunchingWithOptions:` method. Make sure to use the same channel name
as was used on the Flutter client side.

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

Next, add the actual iOS ObjectiveC code that uses the iOS battery APIs to
retrieve the battery level. This code is exactly the same as you would have
written in a native iOS app.

Add the following as a new method in the `AppDelegate` class, just before `@end`:

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

Finally, complete the `setMethodCallHandler` method added earlier. You need
to handle a single platform method, `getBatteryLevel`, so test for that in
the `call` argument. The implementation of this platform method simply calls the
iOS code written in the previous step, and passes back a response for both
the success and error cases using the `result` argument. If an unknown method
is called, report that instead.

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

You should now be able to run the app on iOS. If you are using the iOS
Simulator, note that it does not support battery APIs, and the app will thus
display 'battery info unavailable'.

### Step 4b: Add an iOS platform-specific implementation using Swift {#example-swift}

*Note*: The following steps are similar to step 4a, only using Swift rather than
Objective-C.

This step assumes that you created your project in [step 1.](#example-project)
using the `-i swift` option.

Start by opening the iOS host portion of your Flutter app in Xcode:

1. Start Xcode

1. Select the menu item 'File > Open...'

1. Navigate to the directory holding your Flutter app, and select the `ios`
folder inside it. Click OK.

Next, add support for Swift in the standard template setup that uses
Objective-C:

1. Expand Runner > Runner in the Project navigator.

1. Open the file `AppDelegate.swift` located under Runner > Runner in the Project
navigator.

Next, override the `application:didFinishLaunchingWithOptions:` function and create
a `FlutterMethodChannel` tied to the channel name `samples.flutter.io/battery`:

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
      // Handle battery messages.
    })

    GeneratedPluginRegistrant.register(with: self)
    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
  }
}
```

Next, add the actual iOS Swift code that uses the iOS battery APIs to retrieve
the battery level. This code is exactly the same as you would have written in a
native iOS app.

Add the following as a new method at the bottom of `AppDelegate.swift`:

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

Finally, complete the `setMethodCallHandler` method added earlier. You need
to handle a single platform method, `getBatteryLevel`, so test for that in
the `call` argument. The implementation of this platform method simply calls the
iOS code written in the previous step. If an unknown method
is called, report that instead.

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

You should now be able to run the app on iOS. If you are using the iOS
Simulator, note that it does not support battery APIs, and the app
displays 'Battery info unavailable.'.

## Separate platform-specific code from UI code {#separate}

If you expect to use your platform-specific code in multiple Flutter apps, it
can be useful to separate the code into a platform plugin located in a directory
outside your main application. See [developing
packages](/docs/development/packages-and-plugins/developing-packages) for details.

## Publish platform-specific code as a package {#publish}

If you wish to share your platform-specific with other developers in the Flutter
ecosystem, please see [publishing
packages](/docs/development/packages-and-plugins/developing-packages#publish) for details.

## Custom channels and codecs

Besides the above mentioned `MethodChannel`, you can also use the more plain
[`BasicMessageChannel`][BasicMessageChannel], which supports basic, asynchronous
message passing using a custom message codec. Further, you can use the
specialized [`BinaryCodec`][BinaryCodec], [`StringCodec`][StringCodec], and
[`JSONMessageCodec`][JSONMessageCodec] classes, or create your own codec.

[BasicMessageChannel]: https://docs.flutter.io/flutter/services/BasicMessageChannel-class.html
[BinaryCodec]: https://docs.flutter.io/flutter/services/BinaryCodec-class.html
[StringCodec]: https://docs.flutter.io/flutter/services/StringCodec-class.html
[JSONMessageCodec]: https://docs.flutter.io/flutter/services/JSONMessageCodec-class.html
