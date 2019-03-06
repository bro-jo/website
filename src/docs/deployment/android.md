---
title: 안드로이드 앱 출시 준비하기
short-title: Android
---

Flutter 앱을 개발하는 동안, 명령줄에서의 `flutter run` 과 IntelliJ에서의 툴바 **Run** 과 **Debug** 버튼을 통해 앱을 테스트할 수 있습니다. Flutter는 기본적으로 앱의 *debug* 버전을 빌드합니다.

[Google Play 출시][play]하기 위해 안드로이드의 *release* 버전을 준비할 준비가 되었다면, 다음 단계를 따르세요.

## 앱 매니페스트 검토하기

올바른 앱 설정을 위하여 `<app dir>/android/app/src/main` 에 있는 기본 [앱 매니페스트][manifest] 파일인 `AndroidManifest.xml`을 검토하고 올바른 값들을 포함하는지 확인하세요.
특히:

* `application`: 앱의 이름을 반영하기 위해 [`application`][applicationtag]에 있는 `android:label`을 수정하세요.

* `uses-permission`: 앱 내에서 인터넷 접근이 필요하지 않다면 `android.permission.INTERNET` [permission][permissiontag]을 제거하세요. 기본 템플릿에서는 Flutter 도구와 실행 중인 앱의 커뮤니케이션을 위해 해당 권한을 포함합니다.
  

## 빌드 구성 검토하기

올바른 빌드 구성을 위하여 `<app dir>/android/app` 에 있는 기본 [Gradle build file][gradlebuild]  파일인 `build.gradle` 을 검토하고 올바른 값들을 포함하는지 확인하세요.

* `defaultConfig`:

  * `applicationId`: 고유한 최종 (Application Id)[appid]를 지정하세요.

  * `versionCode` & `versionName`: 내부 앱 버전 번호를 지정하고, 문자열 형태로 명시하세요. pubspec.yaml 파일에 `version` 속성을 설정함으로써 내부 앱 버전 번호를 문자열 형태로 지정할 수 있습니다. 버전 정보 지침에 대해서는 [버전 문서][versions]를 참조하세요.

  * `minSdkVersion` & `targetSdkVersion`: 최소 API 레벨과 개발 대상 버전으로 지정한 지정 API 레벨을 명시하세요. 자세한 내용은 [버전 문서][versions]의 API 레벨 영역을 참조하세요.

## 런처 아이콘 추가하기

새로 생성된 Flutter 앱의 아이콘은 기본 런처 아이콘으로 설정되어있습니다. 런처 아이콘을 원하는 아이콘으로 변경하고 싶다면, [Flutter Launcher Icons]({{site.pub}}/packages/flutter_launcher_icons) package 를 확인해보세요.

package를 사용하지 않고, 런처 아이콘을 바꾸려면 다음 지시사항을 따르세요.

1. 아이콘 디자인을 위해 [Android Launcher Icons][launchericons] 가이드라인을 확인합니다.

1. `<app dir>/android/app/src/main/res/` 에서 아이콘 파일을 [configuration qualifiers][]에 맞는 이름의 폴더에 넣습니다.

1. `AndroidManifest.xml` 에서 [`application`][applicationtag] 태그의 `android:icon` 항목을 업데이트하여 이전 단계에서 넣은 아이콘을 가져옵니다. (예를 들어, `<application android:icon="@mipmap/ic_launcher> ...`)

1. 아이콘이 변경된 것을 확인하기 위해 `flutter run` 를 통해 앱을 실행시키고 런처에서 앱 아이콘을 합니다.

## 앱 서명하기

Google Play 스토어에 출시하기 위해서는 반드시 앱에 디지털 서명을 해야 합니다.
앱에 디지털 서명을 하기 위해 다음 지시사항을 따르세요.

### keystore 만들기
이미 keystore를 가지고 있다면 다음 단계로 넘어가세요. 그렇지 않다면, 다음 명령줄 코드를 통해 keystore를 생성하세요:
`keytool -genkey -v -keystore ~/key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key`

*참고:* 이 파일은 항상 개인적으로 보관하세요; 절대 공개된 저장소에 업로드 하지 마세요.

*참고:* `keytool` 은 프로젝트 경로에 존재하지 않을 수 있습니다. 해당 파일은 안드로이드 스튜디오와 함께 설치되는 Java JDK에 포함되는 파일입니다. 해당 파일에 대한 구체적인 경로는 명령줄에 `flutter doctor -v` 을 통하여 표시되는 'Java binary at:' 다음에 나타나는 경로에서 `java`를 포함하고 있는 디렉토리의 `keytool` 파일을 통해 확인할 수 있습니다.

### 앱으로부터 keystore 참조하기

keystore 참조에 관한 구성을 담을  `<app dir>/android/key.properties` 파일을 생성합니다:

```
storePassword=<password from previous step>
keyPassword=<password from previous step>
keyAlias=key
storeFile=<key store 파일 위치, 예) /Users/<user name>/key.jks>
```

*참고:* 이 파일은 항상 개인적으로 보관하세요; 절대 공개된 저장소에 업로드 하지 마세요.

### gradle에서 서명 구성하기

앱의 서명을 구성하기 위하여 `<app dir>/android/app/build.gradle` 파일을 수정합니다.

1. 다음 코드를:
```
   android {
```
   속성 파일로부터 keystore 정보를 가져올 수 있도록 변경하세요.
```
   def keystoreProperties = new Properties()
   def keystorePropertiesFile = rootProject.file('key.properties')
   if (keystorePropertiesFile.exists()) {
       keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
   }

   android {
```

1. 다음 코드를:
```
   buildTypes {
       release {
           // TODO: release 빌드 버전을 위한 서명 구성을 추가하세요.
           // 현재는 `flutter run --release`가 디버그용 키로 서명되어 동작합니다.
           signingConfig signingConfigs.debug
       }
   }
```
   다음과 같이 변경하세요:
```
   signingConfigs {
       release {
           keyAlias keystoreProperties['keyAlias']
           keyPassword keystoreProperties['keyPassword']
           storeFile file(keystoreProperties['storeFile'])
           storePassword keystoreProperties['storePassword']
       }
   }
   buildTypes {
       release {
           signingConfig signingConfigs.release
       }
   }
```

이제 앱의 release 빌드에서는 자동으로 서명될 것 입니다.


## Proguard 사용

기본적으로, Flutter는 안드로이드 관련 코드를 난독화하거나 축소화하지 않습니다.
서드파티 자바 라이브러리나 안드로이드 라이브러리를 사용한다면 Proguard 적용을 통해 APK의 크기를 줄이고 리버스 엔지니어링으로부터 코드를 보호할 수 있습니다.

다트 코드 난독화에 대한 정보를 얻기 위해서는 [Flutter wiki]({{site.github}}/flutter/flutter/wiki)의 [Obfuscating Dart
Code]({{site.github}}/flutter/flutter/wiki/Obfuscating-Dart-Code)를 참조하세요.

### Step 1 - Proguard 구성하기

 Proguard Rule을 구성하기 위하여 `/android/app/proguard-rules.pro` 파일을 생성하고 다음과 같은 규칙을 추가합니다.

```
#Flutter Wrapper
-keep class io.flutter.app.** { *; }
-keep class io.flutter.plugin.**  { *; }
-keep class io.flutter.util.**  { *; }
-keep class io.flutter.view.**  { *; }
-keep class io.flutter.**  { *; }
-keep class io.flutter.plugins.**  { *; }
```

이 설정은 Flutter 엔진 라이브러리를 보호합니다. </br>
다른 라이브러리(예. Firebase)를 보호하기 위해서는 추가적으로 규칙을 작성해야합니다.

### Step 2 - 난독화와 축소 사용하기

`/android/app/build.gradle` 파일을 열고 `buildTypes`가 정의된 부분을 찾으세요.</br>
`release` 내부에는 `minifiyEnabled`와 `useProguard` 플래그가 true 로 지정되어 있으며, 여기에 step 1 에서 만든 Proguard 규칙 파일을 추가해야 합니다.

```
android {

    ...

    buildTypes {

        release {

            signingConfig signingConfigs.release

            minifyEnabled true
            useProguard true

            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'

        }
    }
}
```

참고: 난독화와 축소 과정으로 인해 안드로이드 앱의 컴파일 시간이 크게 늘어날 수 있습니다.

## Release APK 빌드하기

여기서는 Release APK를 빌드하는 방법을 소개합니다. 만약 이전 서명 구성 단계를 수행했다면, Release APK는 이미 서명되었을 것입니다.

명령줄을 사용하세요:

1. `cd <app dir>` (`<app dir>` 를 앱의 디렉토리로 변경하세요).
1. Run `flutter build apk` (`flutter build` 의 기본값 옵션은 `--release` 입니다).

이제 앱의 release APK가 `<app dir>/build/app/outputs/apk/release/app-release.apk` 에 생성되었습니다.

## 기기에 release APK 설치하기

다음 지시사항에 따라 이전 단계에서 생성한 APK를 연결된 안드로이드 기기에 설치하세요.

명령줄을 사용하세요:

1. 안드로이드 기기를 컴퓨터에 연결합니다.
1. `cd <app dir>` (설치하려는 앱의 디렉토리입니다).
1. `flutter install` 을 통해 설치합니다.

## Google Play 스토어에 APK 출시하기

Google Play 스토어 앱 출시에 대한 자세한 내용은 [Google Play 출시 문서][play]에서 확인하세요.

[manifest]: {{site.android-dev}}/guide/topics/manifest/manifest-intro
[manifesttag]: {{site.android-dev}}/guide/topics/manifest/manifest-element
[appid]: {{site.android-dev}}/studio/build/application-id
[permissiontag]: {{site.android-dev}}/guide/topics/manifest/uses-permission-element
[applicationtag]: {{site.android-dev}}/guide/topics/manifest/application-element
[gradlebuild]: {{site.android-dev}}/studio/build/#module-level
[versions]: {{site.android-dev}}/studio/publish/versioning
[launchericons]: {{site.android-dev}}/guide/practices/ui_guidelines/icon_design_launcher
[configuration qualifiers]: {{site.android-dev}}/guide/topics/resources/providing-resources#AlternativeResources
[play]: {{site.android-dev}}/distribute/googleplay/start
