# Session 1 - TypeScript Intro, Getting Started

Slides for the session can be found at [kevinhennessy.github.io/angular2/Session1](http://kevinhennessy.github.io/angular2/Session1)

The repository for the book chapter is located at [github.com/chandermani/angular2byexample/tree/checkpoint1.1](https://github.com/chandermani/angular2byexample/tree/checkpoint1.1)

## Pre-reqs
This session assumes you have read chapter 1 of the book and have some familiarity with JavaScript and web development.
## TypeScript Intro
## Getting Started
### Developer Tools
We recommend using one of the following code editors:
* [Visual Studio Code](https://code.visualstudio.com/)
* [WebStorm](https://www.jetbrains.com/webstorm/)
* [Sublime](https://www.sublimetext.com/)

We will not be working with Visual Studio in these sessions.

We'll use the command line for executing commands including running the application.
### Setting up a Development Server
* Download and Install [Node and NPM](https://nodejs.org)
* Install the live server module
```
npm install -g live-server
```
## Guess the Number
### The Host File
* Add a directory on your local drive.
* Name it guessthenumber or whatever you like.
* Create a file named index.html and add it to the directory with the following markup:
```javascript
<!DOCTYPE html>
<html>
  <head>
    <title>Guess the Number!</title>
    <link href="http://netdna.bootstrapcdn.com/bootstrap/3.1.1/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://unpkg.com/core-js/client/shim.min.js"></script>
    <script src="https://unpkg.com/reflect-metadata@0.1.3"></script>
    <script src="https://unpkg.com/zone.js@0.6.23?main=browser"></script>
    <script src="https://unpkg.com/typescript@2.0/lib/typescript.js"></script>
    <script src="https://unpkg.com/systemjs@0.19.27/dist/system.js"></script>
    <script src="systemjs.config.js"></script>
    <script>
      System.import('app').catch(function(err){ console.error(err); });
    </script>
  </head>
  <body>
    <my-app>Loading...</my-app>
  </body>
</html>
```
### The Config File
* Add a file called systemjs.config.js to the same directory with the following script:
```javascript
System.config({
    map : {
        'app': 'app',
        'rxjs': 'https://unpkg.com/rxjs@5.0.0-beta.12',
        '@angular/common': 'https://unpkg.com/@angular/common@2.0.0',
        '@angular/compiler': 'https://unpkg.com/@angular/compiler@2.0.0',
        '@angular/core': 'https://unpkg.com/@angular/core@2.0.0',
        '@angular/platform-browser': 'https://unpkg.com/@angular/platform-browser@2.0.0',
        '@angular/platform-browser-dynamic': 'https://unpkg.com/@angular/platform-browser-dynamic@2.0.0'
    },
    packages:{
        'app':  { main: 'main.ts',  defaultExtension: 'ts' },
        '@angular/common': { main: 'bundles/common.umd.js', defaultExtension: 'js' },
        '@angular/compiler': { main: 'bundles/compiler.umd.js', defaultExtension: 'js' },
        '@angular/core': { main: 'bundles/core.umd.js', defaultExtension: 'js' },
        '@angular/platform-browser': { main: 'bundles/platform-browser.umd.js', defaultExtension: 'js' },
        '@angular/platform-browser-dynamic': { main: 'bundles/platform-browser-dynamic.umd.js', defaultExtension: 'js' },
    },
    // DEMO ONLY! REAL CODE SHOULD NOT TRANSPILE IN THE BROWSER
    transpiler: 'typescript',
    typescriptOptions: {
        emitDecoratorMetadata: true
    }
});
```
### The Component File
* Add a subdirectory named app.
* Create a file with the name guess-the-number.component.ts
* Add the following at the top of the page:
```javascript
import { Component }from '@angular/core';
```
#### Decorator
Next add the following to the component file:
```javascript
@Component({
  selector: 'my-app',
  template: `
    <div class="container">
      <h2>Guess the Number !</h2>
      <p class="well lead">Guess the computer generated random number between 1 and 1000.</p>
      <label>Your Guess: </label>
      <input type="number" [value]="guess" (input)="guess = $event.target.value" />
      <button (click)="verifyGuess()" class="btn btn-primary btn-sm">Verify</button>
      <button (click)="initializeGame()" class="btn btn-warning btn-sm">Restart</button>
      <div>
        <p *ngIf="deviation<0" class="alert alert-warning">Your guess is higher.</p>
        <p *ngIf="deviation>0" class="alert alert-warning">Your guess is lower.</p>
        <p *ngIf="deviation===0" class="alert alert-success">Yes! That's it.</p>
      </div>
      <p class="text-info">No of guesses :
        <span class="badge">{{noOfTries}}</span>
      </p>
    </div>
    `
})
```
#### Defining the Class
Finally add the following below the decorator:
```javascript
export class GuessTheNumberComponent {
  deviation: number;
  noOfTries: number;
  original: number;
  guess: number;

  constructor() {
    this.initializeGame();
  }
  initializeGame() {
    this.noOfTries = 0;
    this.original = Math.floor((Math.random() * 1000) + 1);
    this.guess = null;
    this.deviation = null;
  }
  verifyGuess() {
    this.deviation = this.original - this.guess;
    this.noOfTries = this.noOfTries + 1;
  }
}
```
### The Module File
* Create a file named app.module.ts. in the app directory
* Add the following code:
```javascript
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { GuessTheNumberComponent }   from './guess-the-number.component';

@NgModule({
imports:      [ BrowserModule ],
declarations: [ GuessTheNumberComponent ],
bootstrap:    [ GuessTheNumberComponent ]
})

export class AppModule { }
```
### Boostrapping
* Create a file named main.ts in the app directory
* Add the following code:
```javascript
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

import { AppModule } from './app.module';

const platform = platformBrowserDynamic();
platform.bootstrapModule(AppModule)
```
### Start the App
Run the application (from its root directory)
```javascript
live-server
```
## Extra Credit
Get the app setup and working using the [Angular CLI](https://cli.angular.io/)
### Installation
```
npm install -g @angular/cli
```
### Create and serve the project
```
ng new GUESS_THE_NUMBER
cd GUESS_THE_NUMBER
ng serve
```
* Replace the code in the app.component.ts with the code in the guess-the-number.component.ts
* Fix the naming of the component in app.module.ts. 
* Optionally add the link to bootstrap to the index.html. 
* Fix the test files or delete them.
* Navigate to http://localhost:4200/
## Homework
* Read Chapter 4
* Grab the [repo for Checkpoint 4.1](https://github.com/chandermani/angular2byexample/tree/checkpoint3.1/trainer)
* Get Personal Trainer set up and running -- see the [readme](https://github.com/chandermani/angular2byexample/tree/checkpoint3.1/trainer)  in the trainer directory
