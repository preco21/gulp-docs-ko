# 시작하기

#### 1. npm을 통해 gulp CLI를 전역에 설치합니다:

```sh
$ npm install --global gulp
```

#### 2. gulp를 프로젝트 devDependencies로 설치합니다:

```sh
$ npm install --save-dev gulp
```

#### 3. 프로젝트의 루트 디렉터리에 `gulpfile.js`를 만듭니다:

```js
var gulp = require('gulp');

gulp.task('default', function() {
  // 기본 작업 코드를 이곳에 작성하세요
});
```

#### 4. gulp를 실행합니다:

```sh
$ gulp
```

위 코드의 기본 작업은 실행되고 아무 일도 하지 않을 것입니다.

각각의 작업을 따로 실행하려면 `gulp <작업> <또 다른 작업>`을 실행하세요.

## 이제 무엇을 해야 하나요?

이렇게 해서 빈 gulpfile을 만들었고 필요한 것은 모두 설치했습니다. 이제 좀 더 자세한 활용법은 [조합법](recipes)과 [관련 기사들](README.md#관련-기사들)을 참고하세요!

## .src, .watch, .dest, CLI args - 이것들은 어떻게 사용하나요?

gulp API에 대한 내용은 모두 [API 참조 문서](API.md)에서 찾을 수 있습니다.

## 사용할 수 있는 플러그인들

gulp 커뮤니티는 계속 자라나고 있고 매일 새로운 플러그인이 만들어지고 있습니다. [gulp 플러그인 목록](http://gulpjs.com/plugins/)을 통해 필요한 플러그인을 탐색하세요.
