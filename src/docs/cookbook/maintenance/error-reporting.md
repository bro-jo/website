---
title: 서비스에 에러 보고하기
prev:
  title: 큰 사이즈의 리스트 다루기
  path: /docs/cookbook/lists/long-lists
next:
  title: 스크린간 위젯 애니메이션
  path: /docs/cookbook/navigation/hero-animations
---

버그로부터 자유로운 앱을 만들기 위해 항상 노력하지만, 완전히 자유로울 수 없습니다.
버그가 많은 앱은 사용자 불만을 야기하기 때문에, 앱의 사용자들이 버그를 얼마나 자주, 
주로 어디서 발생하는지 이해하는 것은 매우 중요한 일입니다. 그렇게 해야,
 버그의 우선 순위를 정하고 그것들을 수정해나갈 수 있기 때문입니다.

사용자가 얼마나 자주 버그를 경험하는지 어떻게 알 수 있을 까요? 에러가 발생하면 
에러와 stacktrace로 구성된 보고서를 만들어 Sentry, Fabric 혹은 Rollbar와 같은 
에러 추적 서비스에 보내세요.

에러 추적 서비스는 사용자가 경험한 모든 에러들을 모아 그룹화합니다. 
이를 통해 얼마나 자주 에러가 발생하고 어디서 사용자가 어려움을 겪는지 알 수 있게 됩니다.

이 페이지에서는 에러 리포팅 서비스인 [Sentry](https://sentry.io/welcome/)에 에러를 리포팅하는 방법에 대해 다룹니다.

## Directions

  1. DSN from Sentry로부터 DSN 얻기
  2. Sentry 패키지 import
  3. `SentryClient` 생성
  4. 에러 리포팅을 위한 함수 생성
  5. 다트 에러 리포팅하기
  6. Flutter 에러 리포팅하기

## 1. Sentry로부터 DSN 얻기

Before reporting errors to Sentry, you'll need a "DSN" to uniquely identify
your app with the Sentry.io service.

To get a DSN, use the following steps:

  1. [Create an account with Sentry](https://sentry.io/signup/)
  2. Log in to the account
  3. Create a new app
  4. Copy the DSN

## 2. Import the Sentry package

Import the
[`sentry`]({{site.pub-pkg}}/sentry) package into the app. The
sentry package makes it easier to send error reports to the Sentry
error tracking service.

```yaml
dependencies:
  sentry: <latest_version>
```

## 3. Create a `SentryClient`

Create a `SentryClient`. You'll use the `SentryClient` to send
error reports to the sentry service.

<!-- skip -->
```dart
final SentryClient _sentry = SentryClient(dsn: "App DSN goes Here");
```

## 4. Create a function to report errors

With Sentry set up, you can begin to report errors. Since you don't want to
report errors to Sentry during development, first create a function that
let's you know whether you're in debug or production mode.

<!-- skip -->
```dart
bool get isInDebugMode {
  // Assume you're in production mode
  bool inDebugMode = false;

  // Assert expressions are only evaluated during development. They are ignored
  // in production. Therefore, this code only sets `inDebugMode` to true
  // in a development environment.
  assert(inDebugMode = true);

  return inDebugMode;
}
```

Next, use this function in combination with the `SentryClient` to report
errors when the app is in production mode.

<!-- skip -->
```dart
Future<void> _reportError(dynamic error, dynamic stackTrace) async {
  // Print the exception to the console
  print('Caught error: $error');
  if (isInDebugMode) {
    // Print the full stacktrace in debug mode
    print(stackTrace);
    return;
  } else {
    // Send the Exception and Stacktrace to Sentry in Production mode
    _sentry.captureException(
      exception: error,
      stackTrace: stackTrace,
    );
  }
}
```

## 5. Catch and report Dart errors

Now that you have a function to report errors depending on the environment,
you need a way to capture Dart errors.

For this task, run your app inside a custom
[`Zone`]({{site.api}}/flutter/dart-async/Zone-class.html). Zones
establish an execution context for the code. This provides a convenient way to
capture all errors that occur within that context by providing an `onError`
function.

In this case, you'll run the app in a new `Zone` and capture all errors by
providing an `onError` callback.

<!-- skip -->
```dart
runZoned<Future<void>>(() async {
  runApp(CrashyApp());
}, onError: (error, stackTrace) {
  // Whenever an error occurs, call the `_reportError` function. This sends
  // Dart errors to the dev console or Sentry depending on the environment.
  _reportError(error, stackTrace);
});
```

## 6. Catch and report Flutter errors

In addition to Dart errors, Flutter can throw additional errors, such as
platform exceptions that occur when calling native code. You need to be sure to
capture and report these types of errors as well.

To capture Flutter errors, override the
[`FlutterError.onError`]({{site.api}}/flutter/foundation/FlutterError/onError.html)
property. If you're in debug mode, use a convenience function
from Flutter to properly format the error. If you're in production mode, 
send the error to the `onError` callback defined in the previous step.

<!-- skip -->
```dart
// This captures errors reported by the Flutter framework.
FlutterError.onError = (FlutterErrorDetails details) {
  if (isInDebugMode) {
    // In development mode, simply print to console.
    FlutterError.dumpErrorToConsole(details);
  } else {
    // In production mode, report to the application zone to report to
    // Sentry.
    Zone.current.handleUncaughtError(details.exception, details.stack);
  }
};
```

## Complete example

To view a working example, see the
[Crashy]({{site.github}}/flutter/crashy) example app.
