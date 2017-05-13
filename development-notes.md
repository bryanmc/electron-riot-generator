# Development Notes

## Bash Script

The **gitbook-app-generator **package is responsible for parsing an building the app out and thus will need to be installed by anyone who uses a Gitbook App Generator in order to build the app from the Gitbook source.  Part of that may be a shell/bash script which handles the initial generator:

[http://stackoverflow.com/questions/19538669/run-bash-script-with-sh](/h ttp://stackoverflow.com/questions/19538669/run-bash-script-with-sh)

---

## Passing Config Values from Package.json to NPM Scripts

The CLI tool will need to be able to read some config values from package.json in order to do it's thing.  Here's how you pass config values from the package.json object into the javascript files:

[http://stackoverflow.com/questions/11580961/sending-command-line-arguments-to-npm-script](http://stackoverflow.com/questions/11580961/sending-command-line-arguments-to-npm-script)

---

# Running Shell Commands in Javascript

There is still only one tool for this job, nothing else is required: [ShellJS](https://github.com/shelljs/shelljs)

---

# Converting Markdown to JSON

The CLI tool inspects every relevant file of the Gitbook's Github repository and converts the markdown into a more workable form, JSON.  There are several packages that claim to do the job, such as this which was not fully explored:

* [https://www.npmjs.com/package/markdown-to-json](https://www.npmjs.com/package/markdown-to-json)

Ultimately, the best candidate for the job was an extremely well developed and documented wrapper called **Markdown-It** combined with **Gulp.**

* [https://markdown-it.github.io/markdown-it/\#MarkdownIt.parse](https://markdown-it.github.io/markdown-it/#MarkdownIt.parse)
* [https://www.npmjs.com/package/gulp](https://www.npmjs.com/package/gulp)
* [http://stackoverflow.com/questions/40442058/convert-markdown-to-json-object](http://stackoverflow.com/questions/40442058/convert-markdown-to-json-object)
* [http://stackoverflow.com/questions/22835609/gulp-ondata-how-to-pass-data-to-next-pipe](http://stackoverflow.com/questions/22835609/gulp-ondata-how-to-pass-data-to-next-pipe)

Example Gulpfile.js

```js
var gulp = require('gulp');
// var md2json = require('md-to-json');
var MarkdownIt = require('markdown-it');
var md = new MarkdownIt();

gulp.task('md2json_task', function() {
  return gulp.src('./test.md')
  .pipe(parseMD())
  .pipe(gulp.dest('./test-parsed'));
});

function parseMD() {
  // you're going to receive Vinyl files as chunks
  function transform(file, cb) {
    // read and convert MD file contents to JSON string using the Markdown-It library
    file.contents = new Buffer(JSON.stringify(md.parse(String(file.contents))));
    // if there was some error, just pass as the first parameter here
    cb(null, file);
  }

  // returning the map will cause your transform function to be called
  // for each one of the chunks (files) you receive. And when this stream
  // receives a 'end' signal, it will end as well.
  //
  // Additionally, you want to require the `event-stream` somewhere else.
  return require('event-stream').map(transform);
}

```

---


