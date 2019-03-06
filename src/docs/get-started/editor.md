---
title: 에디터 설정
prev:
  title: 설치
  path: /docs/get-started/install
next:
  title: 시운전
  path: /docs/get-started/test-drive
toc: false
---

모든 종류의 텍스트 에디터와 커멘트라인 도구를 활용하여 Flutter 앱을 만들 수 있습니다.
하지만, 더 나은 환경을 위해 우리의 에디터 플러그인을 사용하기를 권합니다.
플러그인은 코드 자동완성, 문법 강조 표시, 위젯 편집 지원, 실행 및 디버그 지원 등을 제공합니다.

아래 단계를 따라 안드로이드 스튜디오나 IntelliJ 혹은 VS Code에 플러그인을 추가하세요.
다른 에디터를 사용하고 싶다면, 생략하고 [다음 단계: 시운전](/docs/get-started/test-drive)로 넘어가도 좋습니다.

{% comment %} Nav tabs {% endcomment -%}
<ul class="nav nav-tabs" id="editor-setup" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="androidstudio-tab" href="#androidstudio" role="tab" aria-controls="androidstudio" aria-selected="true">안드로이드 스튜디오 / IntelliJ</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="vscode-tab" href="#vscode" role="tab" aria-controls="vscode" aria-selected="false">비주얼 스튜디오 코드</a>
  </li>
</ul>

{% comment %} Tab panes {% endcomment -%}
<div class="tab-content">

<div class="tab-pane active" id="androidstudio" role="tabpanel" aria-labelledby="androidstudio-tab" markdown="1">

## 안드로이드 스튜디오 설치

안드로이드 스튜디오는 Flutter에 맞는 완벽한 통합 IDE 경험을 제공합니다.

* [안드로이드 스튜디오]({{site.android-dev}}/studio), 3.0 이상 버전

또는, IntelliJ를 사용할 수도 있습니다:

* [IntelliJ IDEA Community](https://www.jetbrains.com/idea/download/), 2017.1 이상 버전
* [IntelliJ IDEA Ultimate](https://www.jetbrains.com/idea/download/), 2017.1 이상 버전

## Flutter와 다트 플러그인 설치

설치를 위해서:

 1. 안드로이드 스튜디오를 시작하세요.
 1. 플러그인 preferences를 여세요 
    (맥OS에서는 **Preferences > Plugins**,
    윈도우와 리눅스에서는 **File > Settings > Plugins**).
 1. **Browse repositories**를 선택하고, Flutter 플러그인을 선택하고 **설치**를 클릭하세요.
 1. 다트 플러그인을 설치하라는 메시지가 나타나면 **Yes**를 클릭하세요.
 1. 메시지가 나타나면 **Restart**를 클릭하세요.

</div>
<div class="tab-pane" id="vscode" role="tabpanel" aria-labelledby="vscode-tab" markdown="1">

## VS Code 설치

VS Code는 Flutter 앱 실행 및 디버그를 지원하는 가벼운 에디터입니다.

* [VS Code](https://code.visualstudio.com/), 최신 안정 버전

## Flutter와 다트 플러그인 설치

 1. VS Code를 시작하세요.
 1. **View > Command Palette...**를 호출하세요.
 1. "install"를 입력하고, **Extensions: Install Extensions**를 선택하세요.
 1. 확장 검색 필드에서 "flutter"를 입력하고, 리스트에서 **Flutter**를 선택한 뒤, **Install**을 클릭하세요.
    이렇게하면 필수 다트 플러그인도 함께 설치됩니다.
 1. VS Code를 다시 로드하기 위해 **Reload to Activate**를 클릭하세요.
 1. **Reload to Activate**을 눌러 VS Code를 다시 시작하세요.

## Flutter Doctor로 설정 확인

 1. **View > Command Palette...**를 호출하세요.
 1. "doctor"를 입력하고, **Flutter: Run Flutter Doctor**를 선택하세요.
 1. **OUTPUT** 창의 출력에 아무 문제가 없는지 확인하세요.

</div>

</div>{% comment %} End: Tab panes. {% endcomment -%}

## 다음 단계

시운전을 위해 Flutter를 사용해봅시다:
첫번째 프로젝트를 만들고, 실행한 뒤 "hot reload"를 경험해보세요 
