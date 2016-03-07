# Note: gulp
Gulp is a streaming build system of javascript. It does help on automating tasks in web development.<br>
There are some notes of gulp cli and api.

### Agenda
* [Intallation](#installation)
* [API](#api)
  * [gulp.src](#gulpsrc)
  * [gulp.dest](#gulpdest)
  * [gulp.task](#gulptask)
  * [gulp.watch](#gulpwatch)
* [CLI](#cli)
* [Writing a Plugin](https://github.com/gulpjs/gulp/blob/master/docs/writing-a-plugin/README.md)

### Installation
- Install gulp-cli: 
```
$ npm install --global gulp-cli
```
- Install gulp in project
```
$ npm install --save-dev gulp
```

- Configure gulpfile.js in root of project
```
var gulp = require('gulp');
gulp.task('default', function() {

});
```

### API
#### gulp.src
Matching related files.
```
gulp.src(app/app.js); //matching app/app.js
gulp.src(app/*.js); //matching all .js file in app directory
gulp.src(app/**/*.js); //recursively matching all .js file in app folder
gulp.src(!app/app.js); //mathing all the files excluding app/app.js

gulp.src(['client/*.js', '!client/b*.js', 'client/bad.js']);
```

#### gulp.dest
Write in directed folder.
```
gulp.dest('./build/minified_templates'); // write results to ./build/minified_templates
```

#### gulp.task
Execute task in defined order.
```
// execute task 'one' first
gulp.task('one', function(cb) {
});

// task 'two' should be executed only when 'one' is finished
gulp.task('two', ['one'], function() {
});

// task 'default' should be executed only when 'one' and 'two' is finished
gulp.task('default', ['one', 'two']);
```

#### gulp.watch
Watch files and do something when a file changes.
```
var watcher = gulp.watch('js/**/*.js', ['uglify','reload']);
watcher.on('change', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```

### CLI
//copy and paste
gulp has very few flags to know about. All other flags are for tasks to use if needed.

- `-v` or `--version` will display the global and local gulp versions
- `--require <module path>` will require a module before running the gulpfile. This is useful for transpilers but also has other applications. You can use multiple `--require` flags
- `--gulpfile <gulpfile path>` will manually set path of gulpfile. Useful if you have multiple gulpfiles. This will set the CWD to the gulpfile directory as well
- `--cwd <dir path>` will manually set the CWD. The search for the gulpfile, as well as the relativity of all requires will be from here
- `-T` or `--tasks` will display the task dependency tree for the loaded gulpfile
- `--tasks-simple` will display a plaintext list of tasks for the loaded gulpfile
- `--color` will force gulp and gulp plugins to display colors even when no color support is detected
- `--no-color` will force gulp and gulp plugins to not display colors even when color support is detected
- `--silent` will disable all gulp logging

The CLI adds process.env.INIT_CWD which is the original cwd it was launched from.
