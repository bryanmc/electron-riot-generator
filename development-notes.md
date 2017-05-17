# Development Notes

---

## Open Source Electron Apps

Some apps from which we may or may not draw inspiration:

* **BoostNote.io** - [Website ](https://boostnote.io)\| [Github](https://github.com/BoostIO/Boostnote/)
  * A full featured app, polished for all platforms
* **PlayCode.io** - [Website ](http://playcode.io)\| Github
  * Simple, minimilistic setup and structure

---

## Bash Script

The **gitbook-app-generator **package is responsible for parsing an building the app out and thus will need to be installed by anyone who uses a Gitbook App Generator in order to build the app from the Gitbook source.  Part of that may be a shell/bash script which handles the initial generator:

[http://stackoverflow.com/questions/19538669/run-bash-script-with-sh](/h ttp://stackoverflow.com/questions/19538669/run-bash-script-with-sh)

---

## Passing Config Values from Package.json to NPM Scripts

The CLI tool will need to be able to read some config values from package.json in order to do it's thing.  Here's how you pass config values from the package.json object into the javascript files:

[http://stackoverflow.com/questions/11580961/sending-command-line-arguments-to-npm-script](http://stackoverflow.com/questions/11580961/sending-command-line-arguments-to-npm-script)

---

## Running Shell Commands in Javascript

There is still only one tool for this job, nothing else is required: [ShellJS](https://github.com/shelljs/shelljs)

---

## Converting Markdown to JSON

The CLI tool inspects every relevant file of the Gitbook's Github repository and converts the markdown into a more workable form, JSON.  There are several packages that claim to do the job, such as this which was not fully explored:

* [https://www.npmjs.com/package/markdown-to-json](https://www.npmjs.com/package/markdown-to-json)

Ultimately, the best candidate for the job was an extremely well developed and documented wrapper called **Markdown-It** combined with **Gulp.**

* [https://markdown-it.github.io/markdown-it/\#MarkdownIt.parse](https://markdown-it.github.io/markdown-it/#MarkdownIt.parse)
* [https://www.npmjs.com/package/gulp](https://www.npmjs.com/package/gulp)
* [http://stackoverflow.com/questions/40442058/convert-markdown-to-json-object](http://stackoverflow.com/questions/40442058/convert-markdown-to-json-object)
* [http://stackoverflow.com/questions/22835609/gulp-ondata-how-to-pass-data-to-next-pipe](http://stackoverflow.com/questions/22835609/gulp-ondata-how-to-pass-data-to-next-pipe)

### Example Gulpfile.js

```js
var gulp = require('gulp');
var MarkdownIt = require('markdown-it');
var md = new MarkdownIt();

gulp.task('md2json-test', function() {
  return gulp.src('./test.md')
  .pipe(parseMD())
  .pipe(gulp.dest('./test-parsed'));
});

function parseMD() {
  // you're going to receive Vinyl files as chunks
  function transform(file, cb) {
    // read and modify file contents
    // file.contents = new Buffer(String(file.contents) + ' some modified content');
    var jsonString = JSON.stringify(md.parse(String(file.contents)));
    console.log(`\nGenerating Sample output:\n\n{${jsonString.substr(0, 100)}}\n\n`);
    var jsonBuffer = new Buffer(jsonString);
    file.contents = jsonBuffer;
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

### Renaming files when using \`gulp.src\`:

The the `gulp-rename` package is required and then simply add it as a step in the method chain:

```js
//....
.pipe(rename('filename.json')) //whatever you want
// ...
```

[http://stackoverflow.com/questions/28593660/gulp-copy-and-rename-a-file](http://stackoverflow.com/questions/28593660/gulp-copy-and-rename-a-file)

---

## Generating Pretty JSON Output

For those times when you need to read it both in files and in the terminal, this package works:

* [https://www.npmjs.com/package/prettyjson](https://www.npmjs.com/package/prettyjson)

---

## Updating JSON files such as package.json

To handle in-place update of JSON files, for example replacing the values of our package details in the project's package.json file, we can use the **json package.**

* [http://stackoverflow.com/questions/25329241/edit-package-json-from-commandline](http://stackoverflow.com/questions/25329241/edit-package-json-from-commandline)

* [http://trentm.com/json/\#FEATURE-In-place-editing](http://trentm.com/json/#FEATURE-In-place-editing)

I originally found this "promzard" utility for drop-in enhancements of npm package init, but decided ultimately to handle it via file content updates.

* [https://github.com/npm/promzard](https://github.com/npm/promzard)

The following package also looks like a possibility:

* [https://github.com/npm/init-package-json](https://github.com/npm/init-package-json)

And finally, NPM's documentation for **package.json** handling could be of some use as well:

* [https://docs.npmjs.com/files/package.json](https://docs.npmjs.com/files/package.json)

---

## Reading JSON files from path

In order to get values from **package.json** for example, or any JSON file for which we require parsing out values, we can quite simply require the **package.json **file as a module and then access it's values in dot notation form:

```js
var pjson = require('./package.json');
console.log(pjson.version);
```

* [http://stackoverflow.com/questions/9153571/is-there-a-way-to-get-version-from-package-json-in-nodejs-code](http://stackoverflow.com/questions/9153571/is-there-a-way-to-get-version-from-package-json-in-nodejs-code)

---

## Managing Terminal Output / Formatting

**Dedent **is useful for messages that are too long allowing you to format them with line-breaks but still properly display in the terminal output:

* [https://www.npmjs.com/package/dedent](https://www.npmjs.com/package/dedent)

---

## Basics of a Creating a CLI

The **Gitbook App Generator **requires a CLI to help manage the creation and syncronisation of the Gitbook app.  The following covers the basics of creating an NPM package for our CLI.

First we create a new project directory and run `npm init`** **to generate a **package.json**.  We install required dependencies via `npm install` and then create the following directory structure:

```bash
//Package Root

├── script
│   └── lib
│       └── some-action-script.js
    ├── build.js
    └── cli.js
├── index.js
└── package.json
```

The **some-action-script.js** is an arbitrary name which refers to a script which is created to handle one very specific task, such as cloning a repo, or creating a project directory, etc.

The following describes the others:

* index.js - the main entry point of our package defined as our **start **script in **package.json** script block.
* lib - a directory which contains our targeted action scripts
* build.js - a script which imports scripts from **lib** and runs a sequence of actions to complete the initial build
* cli.js - the main CLI entry point which we will tell NPM to add to our path, executable via a custom command which we define in **package.json**.

### Creating an Executable and Custom Command

We need to tell NPM to add **cli.js** to our PATH when the package is installed.   On install, npm will symlink that file into prefix/bin for global installs, or ./node\_modules/.bin/ for local installs.  We add this to **package.json:**

```json
{ "bin" : { "gap" : "./script/cli.js" } }
```

### Parsing/Validating Commands and Options

Now in **cli.js** we use the **yargs** package to parse the command line, manage options and generate help on the fly:

* [**This package**](https://www.npmjs.com/package/yargs) is a God-send and major props to [**the contributors**](https://www.npmjs.com/package/yargs/access) who have most assuredly saved countless hours and heads of hair.

```js
#!/usr/bin/env node

var pkgjson = require('../package.json');
var dedent = require('dedent')
const yargs = require('yargs')
const path = require('path')
const fs = require('fs-plus')
var githubRepo = process.env.npm_package_config_githubRepo || "this is something invoked directly"

console.log(`Running Gitbook App Generator CLI v${pkgjson.version}...`);

//Run the CLI once invoked via terminal
parseCLI();

/**
 * Parses the arguments provided with the CLI to verify usage.
 * @return {boolean} returns true if there are no usage errors
 */
function parseCLI() {
  verifyCLICommand(process.argv);
}

/**
 * Verifies that the CLI tool receives proper arguments and handles usage issues
 * and basic commands such as help and version.
 *
 * @param  {object} parsedArgs An array of args passed to the CLI omitting the
 *                             first two items which are the NPM path and the
 *                             CLI script path.
 * @return {object}            Returns the options object from the yargs package
 */
function verifyCLICommand(parsedArgs) {
  // const options = yargs(parsedArgs).wrap(yargs.terminalWidth())
  const options = yargs
  const version = pkgjson.version;

  //Configure yargs to handle commands and their options
  options.usage(
    dedent`Gitbook App Generator v${version}

    Usage: gap <command> [options]

    CLI tool for building and updating applications generated with the Gitbook
    App Generator tool.`
  )
  .command(
    'create',
    'Create a new project from a Gitbook app generator hosted on Github',
    function (yargs) {
      return yargs.option('repo', {
        alias: 'r',
        describe: dedent `The HTTPS web URL of the Github repo that is synced with
                          the Gitbook App Generator for this project.  To load an
                          example, fork the following repo and import it into your
                          Gitbook account:
                          -----------------
                          https://github.com/bryanmc/electron-riot-generator.git

                          For more information visit: http://gitbookapp.io/docs/cli/getting-started`,
        demandOption: true,
        nArgs: 1,
        requireArgs: true
      })
    },
    function (argv) {
      console.log(argv.repo)
    }
  )
  .command(
    'configure',
    'Set global config values that persists throughout execution of commands.',
    function (yargs) {
      return yargs.option('options', {
        alias: 'o',
        describe: dedent  `An object literal containing configuration options to be
                          set globally for this project.  These values persist even
                          after the execution of a command. This option is unused at
                          the moment and has no effect.`,
        demandOption: (yargs) => (true),
        nArgs: 1,
        requireArgs: true
      })
    },
    function (argv) {
      console.log(argv.opts)
    }
  )
  .demandCommand(
      1,
     dedent`You must provide a valid command.   Please review the usage above.`
   )
  .help('h')
  .alias('h', 'help')
  .version('v', 'The currrent version of the CLI', version)
  .alias('v', 'version')

  const args = options.argv

  if (args.help) {
    process.stdout.write(options.help())
    process.exit(0)
  }

  if (args.version) {
    process.stdout.write(
      `Gitbook App Generator    : ${version}\n` +
      `Node    : ${process.versions.node}\n`
    )
    process.exit(0)
  }

  // If somehow we arrive here, exit gracefully...
  setTimeout(function(){
    console.log('Process will exit without execution') 
    process.exit(0)
  });
}
```

### Executing Commands

Each command block can have a parameter which is a function that executes upon the commands and their options/arguments being validated:

```js
//...
.command(
    'create',
    'Create a new project from a Gitbook app generator hosted on Github',
    function (yargs) {
      return yargs.option('repo', {
        alias: 'r',
        describe: dedent `The HTTPS web URL of the Github repo that is synced with
                          the Gitbook App Generator for this project.  To load an
                          example, fork the following repo and import it into your
                          Gitbook account:
                          -----------------
                          https://github.com/bryanmc/electron-riot-generator.git

                          For more information visit: http://gitbookapp.io/docs/cli/getting-started`,
        demandOption: true,
        nArgs: 1,
        requireArgs: true
      })
    },
    //Function is invoked after CLI is parsed and validated
    //Here we are simply logging out the value of the required option
    function (argv) {
      console.log(argv.repo)
    }
  )
//...
```

### Test Installing Globally

Seeing as our package is not published, we cannot install the package as we normally would via NPM's package manager, by invoking the **npm install &lt;package&gt;** command.  Luckily the CLI provides a test install method that can be run directly from the package's root directory.

```bash
npm install-test --global
```

This simulates the package being installed globally and as we have added the **bin block** in **package.json** it add our custom CLI command to our PATH, in our case `gap`command is now available to use.  We could also do the same thing by installing it locally.  For more information:

* [https://docs.npmjs.com/files/package.json\#bin](https://docs.npmjs.com/files/package.json#bin)
* [https://docs.npmjs.com/cli/install-test](https://docs.npmjs.com/cli/install-test)

The following is more basic recap with a slightly different approach

* [https://www.hacksparrow.com/commandline-node-js-scripts-utilities-modules.html](https://www.hacksparrow.com/commandline-node-js-scripts-utilities-modules.html)

## Live Reloading a Global NPM Package

When we make changes during development, the package needs to be reinstalled via the **install-test** command so that the latest CLI changes take effect.  To do this we can use the **nodemon **package** **and add a **watch** command to the the **scripts** **block** in **package.json**.

```json
"scripts": {
    //...
    "watch": "nodemon --watch script --watch index.js"
}
```

Additionally, we can add a config value to the **config block  **of the **package.json** file where we define a setting '**devmode**' - this is an arbitrary name, with it's value set to **true**:

```json
//...
"config": {
    "devmode": true
}
//...
```

Then in the entry point **index.js **we add a block of code that accesses this value and if set to true, does the stuff required to reload the package:

```js
// index.js

if ( process.env.npm_package_config_devmode ) {
  logger({
    notifier: {
      title:`Gitbook App Generator:\nDev Mode Active`,
      message: `Starting gitbook-app-generator package!`,
      status: `positive`
    },
    logs: [
      {m: `Starting gitbook-app-generator package`, c: 'cyan'},
      {m: `Dev Reload mode is activated.`}
    ]
  });

  sh.exec(`npm install-test --global`)
  sh.exec(`gap dev-reload`)
  sh.exec(`gap help`)
}
```

This calls a **logger **function that handles console logs and notifications.  Finally it executes 3 shell commands with the help of the **shelljs package** invoking **install-test** to reload the package globally and then two commands from the CLI itself: **dev-reload** which doesn't do much, but is there just in case in the future we need to do "stuff" when the package is reloaded, and the **help** command which logs the latest **usage **text of the CLI to terminal, which is auto-generated with the **yargs package.** This is a way to confirm that the latest changes to the CLI are in effect, including all **commands and options**.

> #### Example Usage Output:

```bash
Usage: gap <command> [options]

CLI tool for building and updating applications generated with the Gitbook
App Generator tool.

Commands:
  dev-reload  Verify the package has been reloaded and installed globally when
              files change.
  create      Create a new project from a Gitbook app generator hosted on Github
  configure   Set global config values that persists throughout execution of
              commands.

Options:
  -h, --help     Show help                                             [boolean]
  -v, --version  The currrent version of the CLI                       [boolean]
```

Whenever we then make changes to the files being watched, we get some terminal output as such:

> #### Example Restart Output

```console
[nodemon] starting `node index.js`
npm WARN deprecated minimatch@2.0.10: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue
npm WARN deprecated minimatch@0.2.14: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue
npm WARN deprecated graceful-fs@1.2.3: graceful-fs v3.0.0 and before will fail on node releases >= v7.0. Please update to graceful-fs@^4.0.0 as soon as possible. Use 'npm ls graceful-fs' to find it in the tree.
c:\Users\bryan\AppData\Roaming\npm\gap -> c:\Users\bryan\AppData\Roaming\npm\node_modules\gitbook-app-generator\script\cli.js
c:\Users\bryan\AppData\Roaming\npm
`-- gitbook-app-generator@1.0.1

npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@^1.0.0 (node_modules\gitbook-app-generator\node_modules\chokidar\node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.1.1: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})

> gitbook-app-generator@1.0.1 test C:\Users\Bryan\OneDrive\PC\Development\Desktop\gitbook-app-generator
> echo 'No Tests'

'No Tests'
Building project...
Running Gitbook App Generator CLI v1.0.1...
Executed [dev-reload] at: Mon May 15 2017 19:52:35 GMT+0000 (Coordinated Universal Time)Building project...
Running Gitbook App Generator CLI v1.0.1...
Gitbook App Generator v1.0.1

Usage: gap <command> [options]

CLI tool for building and updating applications generated with the Gitbook
App Generator tool.

Commands:
  dev-reload  Verify the package has been reloaded and installed globally when
              files change.
  create      Create a new project from a Gitbook app generator hosted on Github
  configure   Set global config values that persists throughout execution of
              commands.

Options:
  -h, --help     Show help                                             [boolean]
  -v, --version  The currrent version of the CLI                       [boolean]

[nodemon] clean exit - waiting for changes before restart
```

---

## Fuschia OS README

An abstract overview of Fuschia's UI structure and base of components which could potentially be used as inspiration for the Generator UI

* [https://github.com/fuchsia-mirror/sysui](https://github.com/fuchsia-mirror/sysui)

# ...



