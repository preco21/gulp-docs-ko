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
타입: `String` or `Array`

읽어들일 glob 또는 glob 배열입니다.

#### options
타입: `Object`

[glob-stream]을 통해 [node-glob]에 전달할 옵션입니다.

gulp는 [node-glob][node-glob documentation]와 [glob-stream]에서 지원하는 옵션 외에도 추가 옵션을 제공합니다:

#### options.buffer
타입: `Boolean`
기본값: `true`

Setting this to `false` will return `file.contents` as a stream and not buffer files. This is useful when working with large files. **Note:** Plugins might not implement support for streams.

#### options.read
타입: `Boolean`
기본값: `true`

Setting this to `false` will return `file.contents` as null and not read the file at all.

#### options.base
타입: `String`
기본값: everything before a glob starts (see [glob2base])

E.g., consider `somefile.js` in `client/js/somedir`:

```js
gulp.src('client/js/**/*.js') // Matches 'client/js/somedir/somefile.js' and resolves `base` to `client/js/`
  .pipe(minify())
  .pipe(gulp.dest('build'));  // Writes 'build/somedir/somefile.js'

gulp.src('client/js/**/*.js', { base: 'client' })
  .pipe(minify())
  .pipe(gulp.dest('build'));  // Writes 'build/js/somedir/somefile.js'
```

### gulp.dest(path[, options])

Can be piped to and it will write files. Re-emits all data passed to it so you can pipe to multiple folders.  Folders that don't exist will be created.

```javascript
gulp.src('./client/templates/*.jade')
  .pipe(jade())
  .pipe(gulp.dest('./build/templates'))
  .pipe(minify())
  .pipe(gulp.dest('./build/minified_templates'));
```

The write path is calculated by appending the file relative path to the given
destination directory. In turn, relative paths are calculated against the file base. 
See `gulp.src` above for more info.

#### path
타입: `String` or `Function`

The path (output folder) to write files to. Or a function that returns it, the function will be provided a [vinyl File instance](https://github.com/wearefractal/vinyl).

#### options
타입: `Object`

#### options.cwd
타입: `String`
기본값: `process.cwd()`

`cwd` for the output folder, only has an effect if provided output folder is relative.

#### options.mode
타입: `String`
기본값: `0777`

Octal permission string specifying mode for any folders that need to be created for output folder.

### gulp.task(name[, deps], fn)

Define a task using [Orchestrator].

```js
gulp.task('somename', function() {
  // Do stuff
});
```

#### name

The name of the task. Tasks that you want to run from the command line should not have spaces in them.

#### deps
타입: `Array`

An array of tasks to be executed and completed before your task will run.

```js
gulp.task('mytask', ['array', 'of', 'task', 'names'], function() {
  // Do stuff
});
```

**Note:** Are your tasks running before the dependencies are complete?  Make sure your dependency tasks are correctly using the async run hints: take in a callback or return a promise or event stream.

#### fn

The function that performs the task's operations. Generally this takes the form of `gulp.src().pipe(someplugin())`.

#### Async task support

Tasks can be made asynchronous if its `fn` does one of the following:

##### Accept a callback

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

##### Return a stream

```js
gulp.task('somename', function() {
  var stream = gulp.src('client/**/*.js')
    .pipe(minify())
    .pipe(gulp.dest('build'));
  return stream;
});
```

##### Return a promise

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

**Note:** By default, tasks run with maximum concurrency -- e.g. it launches all the tasks at once and waits for nothing. If you want to create a series where tasks run in a particular order, you need to do two things:

- give it a hint to tell it when the task is done,
- and give it a hint that a task depends on completion of another.

For these examples, let's presume you have two tasks, "one" and "two" that you specifically want to run in this order:

1. In task "one" you add a hint to tell it when the task is done.  Either take in a callback and call it when you're
done or return a promise or stream that the engine should wait to resolve or end respectively.

2. In task "two" you add a hint telling the engine that it depends on completion of the first task.

So this example would look like this:

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


### gulp.watch(glob [, opts], tasks) or gulp.watch(glob [, opts, cb])

Watch files and do something when a file changes. This always returns an EventEmitter that emits `change` events.

### gulp.watch(glob[, opts], tasks)

#### glob
타입: `String` or `Array`

A single glob or array of globs that indicate which files to watch for changes.

#### opts
타입: `Object`

Options, that are passed to [`gaze`](https://github.com/shama/gaze).

#### tasks
타입: `Array`

Names of task(s) to run when a file changes, added with `gulp.task()`

```js
var watcher = gulp.watch('js/**/*.js', ['uglify','reload']);
watcher.on('change', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```

### gulp.watch(glob[, opts, cb])

#### glob
타입: `String` or `Array`

A single glob or array of globs that indicate which files to watch for changes.

#### opts
타입: `Object`

Options, that are passed to [`gaze`](https://github.com/shama/gaze).

#### cb(event)
타입: `Function`

Callback to be called on each change.

```js
gulp.watch('js/**/*.js', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```

The callback will be passed an object, `event`, that describes the change:

##### event.type
타입: `String`

The type of change that occurred, either `added`, `changed` or `deleted`.

##### event.path
타입: `String`

The path to the file that triggered the event.


[node-glob documentation]: https://github.com/isaacs/node-glob#options
[node-glob]: https://github.com/isaacs/node-glob
[glob-stream]: https://github.com/wearefractal/glob-stream
[gulp-if]: https://github.com/robrich/gulp-if
[Orchestrator]: https://github.com/robrich/orchestrator
[glob2base]: https://github.com/wearefractal/glob2base
