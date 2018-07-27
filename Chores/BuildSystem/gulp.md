# Installation

The goal of `gulp-cli` is to let you use gulp like a global program, but without installing `gulp` globally.
For example if you installed gulp 3.9.1 globally and your project testGulp4 has gulp 4.0 installed locally, 
what would happen if you run gulp -v into testGulp4?

### Without gulp-cli globally installed:
```bash
CLI version 3.9.1
```
In this case the version information above is that of globally installed gulp. And we cann't get the
version for locally installed gulp.

### With gulp-cli globally installed:
``` bash
CLI version 1.2.1
Local version 4.0.0-alpha.2
```
And in this case the version information above are the version of gulp-cli and that of locally installed gulp respectively. 
The global gulp with version 3.9.1 is totally ignored.

### Conclusion :
`gulp-cli` is preferred because it allows you to use different versions of gulp(by running ```npm install --global gulp-cli```). And we also 
need a local version of `gulp` installed (by running ```npm install --save-dev gulp@version```).

# The gulpfile

## Structure of a typical gulpfile

### Required modules
- Declare required dependencies
```javascript
var gulp = require('gulp');
var uglify require('gulp-uglify');
```

### Named tasks
- compressing static images
- preprocessing css, javascript files
- depolyment build
```jascript
gulp.task('scripts', function(){
    // code here
})
```
> To run a specific gulp task, type the command `gulp task-name` in the terminal. If no task specified, the
`default` task will be fired

### Watch task
- the purpose of the watch task is to watch certain files and folders for changes and then those changes occur run
a named task

```javascript
// watch folder 'js' for all files ending in `.js`, then on change run 'scripts' task
gulp.task('watch', function(){
   gulp.watch('app/js/**/*.js', ['scripts']);
})
```

### Default task
- default task is the task that kicks off all the other tasks

```javascript
// run both 'scripts' and 'watch' tasks and asynchronously runs all tasks at the same time
gulp.task('default', ['scripts', 'watch'])
```
> Like the bulid tool ***make***, in ***makefile*** we can have more than one build objectives, and then designate which to  
build in the command line.
