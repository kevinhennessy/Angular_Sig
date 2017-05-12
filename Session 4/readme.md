# Session 4 - Unit and End-to-End Testing

Slides for the session can be found at [kevinhennessy.github.io/angular2/Session4](http://kevinhennessy.github.io/angular2/Session4)

The repository for the book chapter is located at [github.com/chandermani/angular2byexample/tree/checkpoint7.1](https://github.com/chandermani/angular2byexample/tree/checkpoint7.1)
## Testing in Angular
### Types of Testing
* Unit Testing
* End-to-end testing
### The Angular testing ecosystem
![alt text](TestingEcosystem.png "Testing Ecosystem")
## Getting started with unit testing
### Setting up Karma for unit testing
* Start by installing Karma from the command line. Navigate to the root of your application
code base (in the trainer folder) and install Karma using this command:
    ```javascript
    npm install karma --save-dev
    ```
* To use Karma from the command line, we need to install its command-line interface:
    ```javascript
    npm install -g karma-cli
    ```
* Next we need to configure Karma
    * Create a tests folder in the root (in the
trainer folder), navigate to it, and start the Karma configuration setup with the following
command:
        ```javascript
        karma init
        ```
    * This starts a command-line wizard that guides us through the options available, including
the test framework, folders to watch, and other such settings. 
    * Once the wizard is complete, it generates a karma.conf.js file. 
    * Instead of using the generated configuration file, copy
the **karma.conf.js** file from the companion code base at checkpoint7.1/tests to your
local tests folder.
        > The karma init wizard installs some packages based on our selection. If
        we skip the wizard, the karma-chromelauncher and karma-jasmine
        packages need to be installed manually for unit testing.
* Finally, we install Jasmine:
    ```javascript
    npm install jasmine-core --save-dev --save-exact
    ```
#### The Karma configuration file
```javascript
module.exports = function(config) {
  config.set({

    // base path that will be used to resolve all patterns (eg. files, exclude)
    basePath: '../',

    // frameworks to use
    // available frameworks: https://npmjs.org/browse/keyword/karma-adapter
    frameworks: ['jasmine'],

   // list of files / patterns to load in the browser
   // files will be loaded in <script> tag
    files: [
      // Polyfills.
      'node_modules/core-js/client/shim.js',
      'node_modules/reflect-metadata/Reflect.js',

      // System.js for module loading
      'node_modules/systemjs/dist/system-polyfills.js',
      'node_modules/systemjs/dist/system.src.js',

      // Zone.js dependencies
      'node_modules/zone.js/dist/zone.js',
      'node_modules/zone.js/dist/proxy.js',
      'node_modules/zone.js/dist/sync-test.js',
      'node_modules/zone.js/dist/jasmine-patch.js',
      'node_modules/zone.js/dist/async-test.js',
      'node_modules/zone.js/dist/fake-async-test.js',

      // The included property determines whether Karma will initially load
      // the file. If set to true, Karma will load these files in a <script> tag. If not, 
      // Karma will not load the file, so some other mechanism must be found to do that. The watched
      // property indicates whether a file will be watched for changes.
      // RxJs.
      { pattern: 'node_modules/rxjs/**/*.js', included: false, watched: false },
      { pattern: 'node_modules/rxjs/**/*.js.map', included: false, watched: false },

      // karma - this is the only file where we have set the included property to true
      {pattern: 'tests/karma-test-shim.js', included: true, watched: true},
      //{pattern: 'src/test/matchers.js', included: true, watched: true},

      // paths loaded via module imports
      // Angular itself
      {pattern: 'node_modules/@angular/**/*.js', included: false, watched: true},
      {pattern: 'node_modules/@angular/**/*.js.map', included: false, watched: true},

      // Angular 2 modal
      {pattern: 'node_modules/angular2-modal/**/*.js', included: false, watched: true},

       // Our built application code
      {pattern: 'dist/**/*.js', included: false, watched: true},

      // paths loaded via Angular's component compiler
      // (these paths need to be rewritten, see proxies section)
      {pattern: 'src/**/*.html', included: false, watched: true},
      {pattern: 'static/**/*.css', included: false, watched: true},

      // paths to support debugging with source maps in dev tools
      {pattern: 'src/**/*.ts', included: false, watched: false},
      {pattern: 'node_modules/**/*.js.map', included: false, watched: false}
    ],

    ...

    // proxied base paths
    proxies: {
      // required for component assests fetched by Angular's compiler
      "/src/": "/base/src/"
    },

    ...

    // enable / disable watching file and executing tests whenever any file changes
    autoWatch: true,

    ...
  })
}

```
#### The Karma test shim file
This file uses SystemJs to transpile and load our code and related modules for testing. We'll not cover that here. You can read a description of that file in the book.

### Organization and naming of our test files
We are using the following conventions in setting up and naming our test files:
* Create one test file for each Typescript file that we plan to test.
* Name each of the test files with the name of the file under test plus **.spec**.
* Place each test file in the directory where the file under test is located.
### Unit testing Angular applications
#### Unit testing pipes
#### Running our test files 
### Unit testing components
#### Angular testing utilities
#### Managing Dependencies in our tests
### Unit testing WorkoutRunner Component
### Start unit testing
### Debugging unit tests in Karma
### Unit testing WorkoutRunner Component continued
### Unit Testing Services
### Unit Testing Directives
## Getting started with E2E testing