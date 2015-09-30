## API 참조 문서

바로 가기:
  [gulp.src](#gulpsrcglobs-options) |
  [gulp.dest](#gulpdestpath-options) |
  [gulp.task](#gulptaskname-deps-fn) |
  [gulp.watch](#gulpwatchglob--opts-tasks-or-gulpwatchglob--opts-cb)

### gulp.src(globs[, options])

지정한 glob 또는 glob 배열을 통해 파일을 표현합니다.
[Vinyl 파일](https://github.com/wearefractal/vinyl-fs)의 [stream](http://nodejs.org/api/stream.html)을 반환하며
[파이프](http://nodejs.org/api/stream.html#stream_readable_pipe_destination_options)를 통해 플러그인을 사용할 수 있습니다.

```javascript
gulp.src('client/templates/*.jade')
  .pipe(jade())
  .pipe(minify())
  .pipe(gulp.dest('build/minified_templates'));
```

`glob`는 [node-glob 문법](https://github.com/isaacs/node-glob)을 참조하거나 파일 경로를 직접 사용할 수 있습니다.

#### globs
타입: `String` 또는 `Array`

읽어들일 glob 또는 glob 배열입니다.

#### options
타입: `Object`

[glob-stream]을 통해 [node-glob]에 전달할 옵션입니다.

gulp는 [node-glob][node-glob documentation]와 [glob-stream]에서 지원하는 옵션 외에도 추가 옵션을 제공합니다:

#### options.buffer
타입: `Boolean`
기본값: `true`

`false`로 지정하면 `file.contents`를 버퍼 파일 대신 스트림 형식으로 반환합니다.
아주 큰 파일을 다룰 때 유용하게 사용할 수 있습니다.

**참고:** 플러그인은 스트림에 대한 구현을 지원하지 않을 수 있습니다.

#### options.read
타입: `Boolean`
기본값: `true`

`false`로 지정하면 `file.contents`가 null이 되며 모든 파일을 읽지 않습니다.

#### options.base
타입: `String`
기본값: 모든 glob가 시작되는 위치 ([glob2base]를 참고하세요)

예제: `somefile.js`가 `client/js/somedir`안에 있을 때:

```js
gulp.src('client/js/**/*.js') // Matches 'client/js/somedir/somefile.js' and resolves `base` to `client/js/`
  .pipe(minify())
  .pipe(gulp.dest('build'));  // Writes 'build/somedir/somefile.js'

gulp.src('client/js/**/*.js', { base: 'client' })
  .pipe(minify())
  .pipe(gulp.dest('build'));  // Writes 'build/js/somedir/somefile.js'
```

### gulp.dest(path[, options])

파이프된 스트림을 파일로 변환합니다.
이 작업이 수행된 이후에도 계속해서 스트림을 반환하므로 여러 폴더에 대해 작업을 할 수도 있습니다.
지정한 폴더가 없으면 새로 생성합니다.

```javascript
gulp.src('./client/templates/*.jade')
  .pipe(jade())
  .pipe(gulp.dest('./build/templates'))
  .pipe(minify())
  .pipe(gulp.dest('./build/minified_templates'));
```

파일의 저장 경로는 지정한 목적지 경로에 파일 상대 경로를 추가하여 연산됩니다.
차례로 파일의 상대 경로는 파일 base를 기반으로 연산됩니다.
자세한 내용은 위의 `gulp.src`를 참고하세요.

#### path
타입: `String` 또는 `Function`

`path`(출력 폴더)는 파일을 쓸 경로를 지정합니다. 또는 함수를 지정하면 함수에선 [vinyl File instance](https://github.com/wearefractal/vinyl)를 제공합니다.

#### options
타입: `Object`

#### options.cwd
타입: `String`
기본값: `process.cwd()`

결과물을 출력할 `cwd` 폴더를 지정합니다. 출력 폴더가 상대 경로일 경우에만 작동합니다.

#### options.mode
타입: `String`
기본값: `0777`

출력 폴더를 생성하기 위해 필요한 폴더의 모드를 8진수 권한 문자열로 설정합니다.

### gulp.task(name[, deps], fn)

[Orchestrator]를 사용하여 작업을 정의합니다.

```js
gulp.task('somename', function() {
  // Do stuff
});
```

#### name

작업의 이름입니다. 여기서 지정한 이름으로 CLI 터미널에서 작업을 실행할 수 있으며 공백이 들어가선 안됩니다.

#### deps
타입: `Array`

현재 작업이 실행되기 이전에 먼저 실행할 종속성 작업의 배열입니다.

```js
gulp.task('mytask', ['array', 'of', 'task', 'names'], function() {
  // Do stuff
});
```

**참고:** 혹시 본 작업이 종속성 작업이 끝나기도 전에 실행됩니까? 아래의 비동기 작업 지원을 확인하고 종속성 작업이 올바르게 작동하는지 확인하세요.

#### fn

작업에 사용될 함수입니다. 일반적으로 함수는 `gulp.src().pipe(someplugin())` 같은 구조를 가집니다.

#### 비동기 작업 지원

`fn`의 작업은 비동기로 만들 수 있습니다. 다음 몇 가지 방법이 있습니다:

##### 콜백 사용

```javascript
// run a command in a shell
var exec = require('child_process').exec;
gulp.task('jekyll', function(cb) {
  // build Jekyll
  exec('jekyll build', function(err) {
    if (err) return cb(err); // return error
    cb(); // finished task
  });
});
```

##### Stream 반환

```js
gulp.task('somename', function() {
  var stream = gulp.src('client/**/*.js')
    .pipe(minify())
    .pipe(gulp.dest('build'));
  return stream;
});
```

##### Promise 반환

```javascript
var Q = require('q');

gulp.task('somename', function() {
  var deferred = Q.defer();

  // do async stuff
  setTimeout(function() {
    deferred.resolve();
  }, 1);

  return deferred.promise;
});
```

기본적으로 작업들은 동시성을 최대로 실행됩니다. 쉽게 말해 모든 작업이 한 번에 실행되면 작업들은 대기 없이 모조리 한 번에 실행됩니다. 만약 특정한 순서에 따라 작업이 실행되도록 하려면 다음 두 가지를 만족해야 합니다:

- 작업이 언제 완료되는지 표시하고
- 다른 작업에서 해당 작업의 완료에 대해 표시해야 합니다.

예제로 설명하자면:

먼저 "one"과 "two"라는 작업이 있고, 다음과 같은 순서에 맞춰 진행 합니다:

1. Stream, 콜백 또는 `Promise`을 사용하여 작업 "one"이 정상적으로 완료되는 부분을 표시합니다.

2. 작업 "two"에선 실행 이전에 필요한 "one"이라는 작업을 종속성 작업으로 지정합니다.

예시 코드: 

```js
var gulp = require('gulp');

// takes in a callback so the engine knows when it'll be done
gulp.task('one', function(cb) {
    // do stuff -- async or otherwise
    cb(err); // if err is not null and not undefined, the run will stop, and note that it failed
});

// identifies a dependent task must be complete before this one begins
gulp.task('two', ['one'], function() {
    // task 'one' is done now
});

gulp.task('default', ['one', 'two']);
```


### gulp.watch(glob [, opts], tasks) 또는 gulp.watch(glob [, opts, cb])

파일의 변경을 감시합니다. 언제나 `change` 이벤트를 발생시키는 EventEmitter를 반환합니다.

### gulp.watch(glob[, opts], tasks)

#### glob
타입: `String` 또는 `Array`

변경을 감시할 타겟 파일입니다. 단일 glob 또는 배열을 지정할 수 있습니다.

#### opts
타입: `Object`

[`gaze`](https://github.com/shama/gaze)로 넘겨지는 옵션입니다.

#### tasks
타입: `Array`

파일의 변경 이벤트가 발생할 때마다 호출할 task입니다. 여러 개 지정할 수 있습니다.

```js
var watcher = gulp.watch('js/**/*.js', ['uglify','reload']);
watcher.on('change', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```

### gulp.watch(glob[, opts, cb])

#### glob
타입: `String` 또는 `Array`

변경을 감시할 타겟 파일입니다. 단일 glob 또는 배열을 지정할 수 있습니다.

#### opts
타입: `Object`

[`gaze`](https://github.com/shama/gaze)로 넘겨지는 옵션입니다.

#### cb(event)
타입: `Function`

콜백은 변경 이벤트마다 호출됩니다.

```js
gulp.watch('js/**/*.js', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```

콜백은 변경 내용을 포함하는 `event` 객체를 반환합니다:

##### event.type
타입: `String`

발생한 이벤트의 타입입니다. `added`, `changed`, `deleted` 중 한 가지가 지정됩니다.

##### event.path
타입: `String`

이벤트가 발생한 파일의 경로입니다.


[node-glob documentation]: https://github.com/isaacs/node-glob#options
[node-glob]: https://github.com/isaacs/node-glob
[glob-stream]: https://github.com/wearefractal/glob-stream
[gulp-if]: https://github.com/robrich/gulp-if
[Orchestrator]: https://github.com/robrich/orchestrator
[glob2base]: https://github.com/wearefractal/glob2base
