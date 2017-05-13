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

