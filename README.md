# Steps

## Initialize Git

```bash
git init
```

`.gitignore`

```bash
node_modules/
coverage/
src/*.js
test/*.js
test/report.html
```

## Initialize NPM package

```bash
npm init
```

```txt
package name: (demo-ts-package)
version: (1.0.0) 0.0.1
description: Demo TS package.
entry point: ./dist/index.js
test command:
git repository: https://github.com/another-guy/demo-ts-package
keywords:
author: Igor Soloydenko
license: (ISC) MIT
```

## Create directories

```bash
mkdir src test .config
touch src/.gitkeep test/.gitkeep .config/.gitkeep
```

## Initialize TypeScript

```bash
tsc --init
```

Override the following values:

```js
"target": "es2015",    /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'. */
"module": "commonjs",  /* Specify module code generation: 'none', commonjs', 'amd', 'system', 'umd', 'es2015', or 'ESNext'. */
"lib": ["dom", "es2015"],
"declaration": true,   /* Generates corresponding '.d.ts' file. */
"sourceMap": true,     /* Generates corresponding '.map' file. */
"outDir": "./dist",    /* Redirect output structure to the directory. */
"strict": true         /* Enable all strict type-checking options. */
```

## Add dependencies

```bash
npm i --SD typescript tslint karma-typescript karma-chrome-launcher puppeteer karma-htmlfile-reporter karma @types/mocha mocha karma-mocha @types/chai chai karma-chai
```

## Initialize Karma

Important!

```bash
"C:\Program Files\Git\bin\bash.exe" --login -i
./node_modules/karma/bin/karma init ./.config/karma.conf.js

mocha
no Require.js
ChromeHeadless
src/**/*.ts
test/**/*.ts
watch, not single run
```

**karma.conf.js** looks as following. (See http://karma-runner.github.io/1.0/config/configuration-file.html for more details).

```js
// Karma configuration

process.env.CHROME_BIN = require('puppeteer').executablePath();

module.exports = function(config) {
  config.set({
    // base path that will be used to resolve all patterns (eg. files, exclude)
    basePath: '..',

    plugins: [
      'karma-mocha',
      'karma-chrome-launcher',
      'karma-chai',
      'karma-typescript',
      'karma-htmlfile-reporter',
    ],

    // frameworks to use. available frameworks: https://npmjs.org/browse/keyword/karma-adapter
    frameworks: [
      'karma-typescript',
      'mocha'
    ],

    // list of files / patterns to load in the browser
    files: [
      'src/**/*.ts',
      'test/**/*.ts'
    ],

    // list of files to exclude
    exclude: [
    ],

    // preprocess matching files before serving them to the browser. available preprocessors: https://npmjs.org/browse/keyword/karma-preprocessor
    preprocessors: {
      '**/*.ts': [
        'karma-typescript',
      ],
    },

    karmaTypescriptConfig: {
      tsconfig: "./tsconfig.json",
    },

    // test results reporter to use. possible values: 'dots', 'progress'. available reporters: https://npmjs.org/browse/keyword/karma-reporter
    reporters: [
      'progress',
      'karma-typescript',
    ],

    // web server port
    port: 9876,

    // enable / disable colors in the output (reporters and logs)
    colors: true,

    // level of logging. possible values: config.LOG_DISABLE || config.LOG_ERROR || config.LOG_WARN || config.LOG_INFO || config.LOG_DEBUG
    logLevel: config.LOG_INFO,

    // start these browsers. available browser launchers: https://npmjs.org/browse/keyword/karma-launcher
    browsers: [
      'ChromeHeadless',
    ],

    // enable / disable watching file and executing tests whenever any file changes
    autoWatch: true,

    // Continuous Integration mode. if true, Karma captures browsers, runs the tests and exits
    singleRun: false,

    // Concurrency level. how many browser should be started simultaneous
    concurrency: Infinity,

    htmlReporter: {
      outputFile: 'test/report.html',
    },
  });
};
```

## Initialize `tslint.json` under `.config` directory

```js
{
  "defaultSeverity": "error",
  "extends": [
    "tslint:recommended"
  ],
  "jsRules": {},
  "rules": {
    "arrow-parens": [true, "ban-single-arg-parens"],
    "member-access": [true, "no-public"],
    "quotemark": [true, "single"],
    "array-type": [true, "array"],
    "curly": [true, "ignore-same-line"],
    "object-literal-sort-keys": [false],
    "interface-name": [true, "never-prefix"],
    "no-empty-interface": [false]
  },
  "rulesDirectory": []
}
```

## Custom scripts in `package.json`

```js
"test": "karma start",
"test:single-run": "karma start --single-run",
"tslint": "tslint -c ./.config/tslint.json 'src/**/*.ts' -t stylish",
"double-check": "npm run test:single-run && npm run tslint"
```

## Add `settings.json` under `.vscode` directory

```js
{
  "tslint.configFile": ".config/tslint.json",
  "tslint.autoFixOnSave": true,
  "tslint.enable": true,
  "editor.tabSize": 2
}
```

## Sources

https://medium.com/powtoon-engineering/a-complete-guide-to-testing-javascript-in-2017-a217b4cd5a2a
