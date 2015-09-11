## CLI 사용법

### 플래그

gulp는 아주 적은 플래그를 가지고 있습니다. 작업에 필요하다면 다음 플래그를 사용할 수 있습니다.

- `-v` or `--version` 전역, 지역 gulp의 버전을 표시합니다.
- `--require <module path>` gulpfile을 실행하기 전에 포함할 모듈을 지정합니다. 이 플래그는 빌드전에 transpile 언어를 컴파일 할 때 사용할 수 있습니다. 또한 다중으로 `--require` 플래그를 사용할 수 있습니다.
- `--gulpfile <gulpfile path>` 실행할 gulpfile을 직접 지정합니다. gulpfile이 여러 개일 때 유용한 플래그입니다. 그뿐만 아니라 gulpfile 디렉터리를 CWD로 설정합니다.
- `--cwd <dir path>` CWD를 직접 지정합니다. gulpfile과 포함된 모든 모듈들은 이 디렉터리에서 찾습니다.
- `-T` or `--tasks` 로드된 gulpfile의 작업 리스트를 의존성 트리 형태로 표시합니다.
- `--tasks-simple` 로드된 gulpfile의 작업 리스트를 간단한 텍스트로 표시합니다.
- `--color` gulp와 gulp 플러그인의 색상 로깅을 지원하지 않을 때도 활성화합니다.
- `--no-color` gulp와 gulp 플러그인의 색상 로깅을 모두 비활성화합니다.
- `--silent` 모든 gulp 로그를 비활성화합니다.

CLI는 자동으로 `process.env.INIT_CWD`에 실제 gulp가 실행된 cwd 위치를 지정합니다.

#### 작업 특정 플래그

작업에서 커스텀 플래그를 사용하려면 이 [StackOverflow 질문](http://stackoverflow.com/questions/23023650/is-it-possible-to-pass-a-flag-to-gulp-to-have-it-run-tasks-in-different-ways)을 참고하세요.

### 작업

작업은 `gulp <작업> <또 다른 작업>` 형식으로 실행할 수 있습니다. 그냥 `gulp`만 실행하면 등록해둔 `default` 작업이 실행됩니다.
만약 `default` 작업이 없으면 gulp 오류가 발생합니다.

### 컴파일러

[Interpret](https://github.com/tkellen/node-interpret#jsvariants)에서 지원하는 언어 목록을 찾을 수 있습니다.
새로운 언어를 추가하고 싶다면 PR을 넣거나 이슈를 생성하세요.
