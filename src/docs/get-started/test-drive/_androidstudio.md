<div class="tab-pane active" id="androidstudio" role="tabpanel" aria-labelledby="androidstudio-tab" markdown="1">

## 앱 생성 {#create-app}

 1. **File > New Flutter Project**를 선택하세요.
 1. 프로젝트 유형으로 **Flutter application**을 선택한 후, **Next**을 누르세요.
 1. **Flutter SDK Path** 텍스트 창의 내용이 SDK 위치가 맞는지 확인하세요.
    아직 SDK를 설치하지 않았다면 SDK를 설치하세요.
 1. 프로젝트 이름을 입력하고 **Next**를 누르세요. (예, `myapp`) 
 1. **Finish**를 클릭하세요.
 1. 안드로이드 스튜디오가 SDK를 설치하고 프로젝트를 생성할 때까지 기다리세요.

위 명령은 [머터리얼 컴포넌트][]를 사용한 간단한 예시 앱이 들어있는 Flutter 프로젝트 디렉토리 `myapp`을 생성합니다.

{% include_relative _main-code-note.md  %}

## 앱 실행

 1. 안드로이드 스튜디오 기본 툴바를 찾으세요: <br>
    ![Main IntelliJ toolbar][]{:.mw-100}
 1. **target selector**에서, 앱을 실행할 안드로이드 기기를 선택하세요.
    리스트에 사용 가능한 기기가 없다면, 
    **Tools> Android > AVD Manager**를 선택하고 새 기기를 생성하세요.
    자세한 사항은, [AVD 관리][]를 참조하세요.
 1. 툴바에서 실행 아이콘을 선택하세요, 
    또는 메뉴 항목에서 **Run > Run**를 실행하세요.

{% capture save_changes -%}
  : invoke **Save All**, or click **Hot Reload**
  <i class="material-icons align-bottom">offline_bolt</i>.
  {% comment %} Or, as an alternative:
    {% asset 'get-started/hot-reload-button.png' alt='looks like a lightning bolt' %}.
  {% endcomment -%}
{% endcapture %}

{% include_relative _try-hot-reload.md save_changes=save_changes %}

[Main IntelliJ toolbar]: {% asset tools/android-studio/main-toolbar.png @path %}
[AVD 관리]: {{site.android-dev}}/studio/run/managing-avds
[머터리얼 컴포넌트]: {{site.material}}/guidelines
</div>
