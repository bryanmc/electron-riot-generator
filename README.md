# Electron + Riot JS App Generator Book {#electron--riot-js-app-generator-book}

This is an "app generator book" which looks to explore the execution of an idea - and that is to more develop small - medium applications in a more semantic and readable manner, as well speeding up the development process by actually parsing the contents of the book into a fully functioning application.

This book generates an a desktop app with Electron and Riot JS. To use this book for yourself, first fork it and then create an application using the Gitbook App Generator CLI tool, which if not yet available \(is that linked?\) will be available soon. Please be patient.

If you have insights, comments, ideas or wish to contribute to this project, please contact me at bryan \[at\] futuristikweb {dot} com. Remember, this is an experiment. If you don't like the idea, think it's dumb, or a waste of time, you are welcome to move right along while keeping your super-ego developer hot takes to yourself.

# Initial Setup Steps

---

## Prepare Electron App Analytics Integration

Create an account at [NeutrinoMetrics](http://neutrinometrics.net "A new Electron app analytics platform in beta stage, where you can get a free beta account.") and follow the integration steps just below:

1. install **electron-analytic**s via npm
2. Add the following lines to your main process file:

```js
const EA = require("electron-analytics");
EA.init("HkxXaheVx-");
```

More info here: [electron-analytics](https://github.com/NeutrinoMetrics/electron-analytics)

# Awesome Existing and Trending Developer Resources

---

## Trending JS Repos on Github this Month

[https://github.com/trending?l=javascript&since=monthly](https://github.com/trending?l=javascript&since=monthly)

---

## Take Care of Yourself

This is a little dashboard that tries to take care of you when you're using your terminal. It tells you cute, self care things, and tries not to stress you out. It shows:

* the last tweets from [@tinycarebot](https://twitter.com/tinycarebot), [@selfcare\_bot](https://twitter.com/selfcare_bot) and [@magicrealismbot](https://twitter.com/magicrealismbot). The first two tend to tweet reminders about taking breaks, drinking water and looking outside, and the latter tells you strange, whimsical stories. If you don't like these bots, they're configurable!
* your `git`commits from today and the last 7 days. When I get stressed out because I think I haven't done anything, it turns out that I only think about big and serious commits, and forget about all the tiny amounts of work I've actually done throughout. Hopefully this will help you too.
* the weather, because you might get rained on.

[Github](https://github.com/notwaldorf/tiny-care-terminal)

---

## Turn Node JS Apps into Cross Platform Executable

This command line interface enables you to package your Node.js project into an executable that can be run even on devices without Node.js installed.  
  Creates executables for Windows, Mac, Linux and Freebsd.

[Github ](https://github.com/zeit/pkg)\| [NPM](https://www.npmjs.com/package/pkg)

