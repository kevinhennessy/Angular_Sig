## Getting started with E2E testing

### What is e2e testing?

*  Testing the application like a real user.

We will be looking at how to run e2e testing of the Personal Trainer app using angular-cli

## 1. Introducton to angular-cli  

* Command line interface for creating angular applications.
* Scafolding - uses best practices to create templates.
* Testing framework is configured by default - both unit testing and e2e testing.

```bash
npm install -g @angular/cli

ng new trainer-e2e

cd trinug

ng serve

```

>  runs the app in the default port 4200 

## cli scaffolds the following files and runs npm install.
![alt text](file-structure-1.jpg "File structure")

### List of Commands that cli currently supports.


Scaffold  | Usage
---       | ---
[Component](https://github.com/angular/angular-cli/wiki/generate-component) | `ng g component my-new-component`
[Directive](https://github.com/angular/angular-cli/wiki/generate-directive) | `ng g directive my-new-directive`
[Pipe](https://github.com/angular/angular-cli/wiki/generate-pipe)           | `ng g pipe my-new-pipe`
[Service](https://github.com/angular/angular-cli/wiki/generate-service)     | `ng g service my-new-service`
[Class](https://github.com/angular/angular-cli/wiki/generate-class)         | `ng g class my-new-class`
[Guard](https://github.com/angular/angular-cli/wiki/generate-guard)         | `ng g guard my-new-guard`
[Interface](https://github.com/angular/angular-cli/wiki/generate-interface) | `ng g interface my-new-interface`
[Enum](https://github.com/angular/angular-cli/wiki/generate-enum)           | `ng g enum my-new-enum`
[Module](https://github.com/angular/angular-cli/wiki/generate-module)       | `ng g module my-module`


You can use the `ng generate` (or just `ng g`) command to generate Angular components:

```bash
ng generate component my-new-component
ng g component my-new-component # using the alias
```

>  angular-cli will add reference to the new generated component in the app.module.ts

## 2. How to Run e2e Test

### Run `ng e2e` 

The test runner will look for files which ends with **\*.spec.ts** in **e2e** folder 



## Frameworks used for e2e testing

1. Protractor - uses Webdriver to control browsers and simulate user actions.
2. Jasmine 


### 


## What happens when you run `ng e2e`
![alt text](angular-cli-json.jpg "angular-cli.json")

## Protractor config file - protractor.conf.js
![alt text](protractor-config.jpg "protractor config file")

## Two files for each page.

	1. File which have the  actual tests -  ends with spec.ts
	2. File which encapsulates the test cases - ends with .po.ts - called as page objects.
		* getTitle() - Gets the title of the page.
		* getExcerciseCount() - counts the number of exercises.
		
The spec file will use the page objects and call the appropriate functions.
		

## Page objects  -  files ending with .po.ts

  *  Encapsulate the test cases for each page.

## Locators

### Locators is the main feature which tells protractor to
    * Find dom elements
	* Interact with elements like adding texts and clicking buttons.

```js
// Find an element using a css selector.
by.css('.myclass') 

// Find an element with the given id.
by.id('myid')

// Find an element with a certain ng-model.
// Note that at the moment, this is only supported for AngularJS apps.
by.model('name')

// Find an element bound to the given variable.
// Note that at the moment, this is only supported for AngularJS apps.
by.binding('bindingname')
```
To Learn more about locators, [checkout the githib link](https://github.com/angular/protractor/blob/master/docs/locators.md)

> http://www.protractortest.org/#/api