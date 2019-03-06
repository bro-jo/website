<div class="tab-pane" id="terminal" role="tabpanel" aria-labelledby="terminal-tab" markdown="1">

## 앱 생성

새로운 프로젝트를 생성하기 위해 `flutter create` 명령을 실행하세요:

```terminal
$ flutter create myapp
$ cd myapp
```

위 명령은 [머터리얼 컴포넌트]({{site.material}}/guidelines/)를 사용한 간단한 예시 앱이 들어있는 Flutter 프로젝트 디렉토리 `myapp`을 생성합니다.

{% include_relative _main-code-note.md  %}

## 앱 실행

 1. 안드로이드 기기가 실행중인지 확인하세요.
    아무것도 나타나지 않는다면,
    [설치][] 페이지에서 OS를 선택하고 해당 페이지의 기기별 지침을 따르세요.

    ```terminal
    $ flutter devices
    ```

 2. 아래 명령으로 앱을 실행하세요:

    ```terminal
    $ flutter run
    ```

{% capture save_changes -%}
.
1. 터미널 창에 <kbd>r</kbd>을 입력하세요.
{% endcapture %}

{% include_relative _try-hot-reload.md save_changes=save_changes %}

[설치]: /docs/get-started/install
</div>
