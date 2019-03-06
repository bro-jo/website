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
주로 어디서 발생하는지 이해하는 것은 매우 중요한 일입니다. 그렇게 해서, 
버그의 우선 순위를 정하고 그것들을 수정해나갈 수 있습니다.

사용자가 얼마나 자주 버그를 경험하는지 어떻게 알 수 있을까요? 에러가 발생하면 
에러와 stacktrace로 구성된 보고서를 만들어 Sentry, Fabric 혹은 Rollbar와 같은 
에러 추적 서비스에 보내세요.

에러 추적 서비스는 사용자가 경험한 모든 에러들을 모아 그룹화합니다. 
이를 통해 얼마나 자주 에러가 발생하고 어디서 사용자가 어려움을 겪는지 알 수 있게 됩니다.

이 페이지에서는 에러 리포팅 서비스인 [Sentry](https://sentry.io/welcome/)에 에러를 리포팅하는 방법에 대해 다룹니다.

## 진행 단계

  1. Sentry로부터 DSN 얻기
  2. Sentry 패키지 import
  3. `SentryClient` 생성
  4. 에러 리포팅을 위한 함수 생성
  5. 다트 에러 catch하고 리포팅하기
  6. Flutter 에러 catch하고 리포팅하기

## 1. Sentry로부터 DSN 얻기

Sentry에 에러 리포팅을 하기 전에 Sentry.io 서비스에서 여러분의 앱을 식별할 수 있는 "DSN"이 필요합니다.

DSN을 얻기 위해 다음 단계를 진행하세요:

  1. [Sentry 계정 생성](https://sentry.io/signup/)
  2. 생성한 계정으로 로그인
  3. 새로운 앱 생성
  4. DSN 복사

## 2. Sentry 패키지 Import

[`Sentry`]({{site.pub-pkg}}/sentry) 패키지를 앱에 import 하세요.
Sentry 패키지를 사용하면 에러 리포트를 더 쉽게 Sentry 에러 추적 서비스에 보낼 수 있습니다.

```yaml
dependencies:
  sentry: <latest_version>
```

## 3. `SentryClient` 생성

`SentryClient`를 생성하세요. `SentryClient`는 Sentry 서비스에 에러 리포트를 보내기 위해 사용됩니다.

<!-- skip -->
```dart
final SentryClient _sentry = SentryClient(dsn: "App DSN goes Here");
```

## 4. 에러 리포팅을 위한 함수 생성

Sentry 설정이 완료되면 이제 에러 리포팅을 할 수 있습니다. 
개발 단계에서는 에러 리포팅을 할 필요가 없기 때문에, 우선 디버그 모드인지 프로덕션 모드인지 구분할 수 있는 함수를 만드세요.

<!-- skip -->
```dart
bool get isInDebugMode {
  // 기본적으로 프로덕션 모드라고 가정합니다.
  bool inDebugMode = false;

  // Assert 표현식은 개발 단계에서만 사용되며, 프로덕션 모드에서는 무시됩니다.
  // 그러므로 이 코드는 개발 단계에서만 'inDebugMode'를 true로 설정합니다.
  assert(inDebugMode = true);

  return inDebugMode;
}
```

그런 다음 프로덕션 모드일 때는 에러를 리포팅할 수 있도록 `SentryClient`를 사용합니다.

<!-- skip -->
```dart
Future<void> _reportError(dynamic error, dynamic stackTrace) async {
  // exception을 콘솔에 출력합니다
  print('Caught error: $error');
  if (isInDebugMode) {
    // 디버그 모드에서는 전체 stacktrace를 출력합니다
    print(stackTrace);
    return;
  } else {
    // 프로덕션 모드에서는 exception과 stacktrace를 Sentry로 보냅니다.
    _sentry.captureException(
      exception: error,
      stackTrace: stackTrace,
    );
  }
}
```

## 5. 다트 에러 catch하고 리포팅하기

개발 환경에 따라 에러 리포팅하는 함수를 작성했으니 이제 다트 에러를 catch할 방법이 필요합니다.

이 작업을 진행하기 위해, 커스텀 [`Zone`]({{site.api}}/flutter/dart-async/Zone-class.html)에서 앱을 실행하세요.
Zone은 코드가 수행될 컨텍스트를 설정합니다. `onError` 함수를 제공하여 컨텍스트 내에서 발생하는 모든 에러를 편리하게 잡을 수 있게 해줍니다.

아래 코드에서는 앱을 새로운 `Zone`에서 실행하고 `onError` 콜백을 통해 모든 에러를 잡습니다.

<!-- skip -->
```dart
runZoned<Future<void>>(() async {
  runApp(CrashyApp());
}, onError: (error, stackTrace) {
  // 에러가 발생할 때 마다, `_reportError` 함수를 호출합니다.
  // 발생한 다트 에러는 개발 환경에 따라 콘솔에 출력되거나 Sentry로 보내집니다.
  _reportError(error, stackTrace);
});
```

## 6. Flutter 에러 catch하고 리포팅하기

다트 에러 뿐만 아니라, Flutter도 native 코드를 호출할 때 발생할 수 있는 플랫폼 예외와 같은 에러들을 던질 수 있는데,
이러한 유형의 에러들 역시 반드시 리포팅해야 합니다.

Flutter 에러를 처리하기 위해서는 [`FlutterError.onError`]({{site.api}}/flutter/foundation/FlutterError/onError.html) 프로퍼티를 오버라이드 하세요.
만약 디버그 모드라면, Flutter의 편리한 함수를 사용하여 적절한 에러 출력 형식을 지정하세요.
만약 프로덕션 모드라면, 이전 단계에서 정의한 `onError` 콜백에 에러를 보내세요.

<!-- skip -->
```dart
// Flutter 프레임워크에 의해 보고된 에러를 포착합니다.
FlutterError.onError = (FlutterErrorDetails details) {
  if (isInDebugMode) {
    // 개발 모드에서는 단순히 콘솔에 출력합니다.
    FlutterError.dumpErrorToConsole(details);
  } else {
    // 프로덕션 모드에서는 Sentry에 보고하기 위해 application zone에 보고 합니다.
    Zone.current.handleUncaughtError(details.exception, details.stack);
  }
};
```

## 전체 예제

동작하는 전체 예제 코드는 [Crashy]({{site.github}}/flutter/crashy)에서 확인할 수 있습니다.
