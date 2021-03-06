# Session 2 - Routing, Forms
Slides for the session can be found at [kevinhennessy.github.io/angular2/Session2](http://kevinhennessy.github.io/angular2/Session2)

In this session we will be working with a series of checkpoints for Chapter 4 of the book.The starting repository for the book chapter Checkpoint 4.1 which is located at [github.com/chandermani/angular2byexample/tree/checkpoint4.1](https://github.com/chandermani/angular2byexample/tree/checkpoint4.1). 

## Pre-reqs
This session assumes you have read chapter 4 of the book.

## Getting set up with Personal Trainer

### Project Setup
Make sure you have node and git installed.  In your favorite local folder: clone the source repository, install live-server, install gulp, install packages, checkout the starting branch, and run the app:

> Why do C# and TypeScript developers keep breaking their keyboards? Because they use a strongly typed language.
```
$ git clone https://github.com/chandermani/angular2byexample.git

$ npm install -g gulp

$ cd angular2byexample/trainer

$ npm install

$ git checkout checkpoint4.1

$ gulp play
```

Now open the application folder in your favorite javascript editor (VS Code, Sublime, Atom, WebStorm, etc).

> **Tip: It is strongly encouraged for you to use git to pull the application code to your local workstation.**  
This will make it much easier to track your changes, move between checkpoints, and restart if necessary.  However, if you don't have git installed you can pull down the code base at any checkpoint as a zip file: [https://github.com/chandermani/angular2byexample/archive/checkpoint4.1.zip](https://github.com/chandermani/angular2byexample/archive/checkpoint4.1.zip) &leftarrow; **Substitute any checkpoint number**

### Personal Trainer
7 Minute Workout is an exercise/workout plan that requires us to perform a set of twelve exercises in quick succession within the seven minute time span. 7 Minute Workout has become quite popular due to its benefits and the short duration of the workout. We cannot confirm or refute the claims but doing any form of strenuous physical activity is better than doing nothing at all. If you are interested to know more about the workout, then check out [http://well.blogs.nytimes.com/2013/05/09/the-scientific-7-minute-workout/](http://well.blogs.nytimes.com/2013/05/09/the-scientific-7-minute-workout/).

Chapter four adds even more functionality to the 7 Minute Workout application by turning it into a Personal Trainer.   The Personal Trainer application not only allows us to run the 7 minute workout, but also to add our own exercises and workouts.  To do this we are going to add a Workout Builder.

### Using git Branches for Checkpoints
The code in the source repository progresses through the exercises outlined in the book using git branches.  As we proceed through these exercises below we will use these branches walk through different steps in the process.  If you get behind or can't get one section working, you can always catch up by moving to the next branch.  As shown above you can move to a new branch using the git checkout command. 
```
$ git checkout checkpoint4.1
```

During this presentation we will be working with checkpoints between 4.1 and 4.6.

### Use of Gulp for builds
Gulp is used in the book and this presentation as a rudimentary build system for our application.  It provides a build pipeline which coordinates the typescript transpilation, startup of the web server, and live reloading.  This is a very simple build pipeline and is used only to facilitate the learning of Angular concepts.  A detailed discussion of the build pipeline is outside the scope of this presentation.  There are a myriad of choices for setting up Angular 2 build pipelines and you would likely want to use something different in your production application.  Here is more information about available tools and recommendataions from the Angular team:

[https://angular.io/docs/ts/latest/guide/deployment.html](https://angular.io/docs/ts/latest/guide/deployment.html)

### Project Organization
The application is organized based on the [Angular style guidelines](https://angular.io/styleguide):  


- trainer
    - src 
        - components 
            - features				
        - services
            - shared services
        - static
            - static files (styles, images, audio)



### 
## Routing
### Essentials
### Why does a client side framework have routing?
We're all used to the way that server side frameworks route incoming http requests to the appropriate logic to handle the request.  This happens with almost every HTTP request on the web whether you working with an API web service or a traditional web application.  In ASP.NET MVC this is handled by establishing a route collection which matches the incoming request and dispatches it to the appropriate controller method.  This is generally based on convention in ASP.NET.

A Single Page Application is designed to allow the client to interact with the application and move between pages without the need for server side page refreshes.  To the user it still appears that they are changing pages because the URL changes and their browser history is updated.  However, the framework is dynamically switching the pages completely on the client.  How is this magic accomplished?

This is not trivial, but Angular is doing all of the heavy lifting.  It has a built-in framework for handling this routing for you. The framework allows you to define rules for switching pages based on the URL.

### HTML 5 History API vs. # URLs
localhost/start vs. localhost/index.html#!/start

In the past SPA frameworks used URLs with a hash in them to identify the client side route to load.  In the HTML spec these indicated bookmarks in a page and do not cause the browser to refresh.  In this way the framework could move between pages without having the browser refresh the page from the server.  

Angular 2 takes advantage of the new HTML 5 history API.  This API allows the routing framework to programmatically update the browsers' history as you navigate.  This allows a much more natural URL scheme and makes the site easier for search engines to navigate.

### Routing in Angular 2
Angular supports SPA development using its routing infrastructure. This infrastructure tracks browser URLs, enables hyperlink generation, exposes routing events, and provides a set of directives/components for view.

There are four major framework pieces that work together to support the Angular routing infrastructure:

- **The Router (Router)**: The primary infrastructure piece that actually provides component navigation.
- **Routing configuration (Route)**: The component router is dependent upon the routing configuration for setting up routes.
- **RouterOutlet component**: The RouterOutlet component is the placeholder container (host) where route-specific views are loaded.
- **RouterLink directive**: This generates hyperlinks that can be embedded in the anchor tags for navigation

We setup routing in our application by making the following changes:

- package.json
    - @angular/router is added to provide routing capabilites.
    ```javascript
    "dependencies": {
        ...
        "@angular/router": "3.0.0",
        ...
    }
    ```
- systemjs.config.js 
    - Update SystemJS to load the router module.
    ```javascript
    var ngPackageNames = [ 
    ... 
        'router', 
    ...];
    ```
- index.html 
    - Add base html tag so ng routing will know the root of the site.
    ```html
    <base href="/">
    ```
- app.routes.ts 
    - Route configuration for the app.
    - A path with ** denotes a catch-all path or the wildcard route for the app.
    ```javascript
    export const routes: Routes = [
    { path: 'start', component: StartComponent },
    { path: 'workout', component: WorkoutContainerCompnent },
    { path: 'finish', component: FinishComponent },
    { path: 'history', component: WorkoutHistoryComponent },
    { path: 'builder', component: WorkoutBuilderComponent},
    { path: '**', redirectTo: '/start' }
    ];

    export const routing: ModuleWithProviders = RouterModule.forRoot(routes);
    ```
- app.component.ts
    - Template needs to contain \<router-outlet>\</router-outlet>
    - RouterOutlet is an Angular component directive that acts as a placeholder for a child component to load on route change. It integrates with the router to load the appropriate component based on the current browser URL and the route definition.
    ```html
    <router-outlet></router-outlet>
    ```
- start.html 
    - Add a link to the workout route.  Notice the routeLink property binding.  This is used throughout the application.  The start template is just one example.
    ```html
    <a [routerLink]="['/builder']">
    ```
- workout-runner.component.ts 
    - Navigates programatically using the router servicw
    ```javascript
    this.router.navigate( ['/finish'] );
    ```
- gulpfile.js
    - Add fallback to gulp-webserver 
    ```javascript
      connect.server({
        root: __dirname,
        port: port,
        livereload: true,
        fallback: 'index.html'
      });
    ```
  
### Child Routing and Lazy Loading
> **Start at [Checkpoint 4.1](https://github.com/chandermani/angular2byexample/tree/checkpoint4.1). Zip file is [here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.1.zip).**

Note the following changes from the previous checkpoint:

* if you look at the route configuration in **app.routes.ts** in the src/components/app folder, you will find one new route definition – **builder**.
```javascript
export const routes: Routes = [ 
  ... 
{ path: 'builder', component: WorkoutBuilderComponent }, 
  ... 
];
``` 
* Behind the scenes, we have added another router link for this link into **start.html**:
```javascript 
<a [routerLink]="['/builder']"> 
  <span>Create a Workout</span> 
  <span class="glyphicon glyphicon-plus"></span> 
</a>
``` 
* Again behind the scenes, we have added a **WorkoutBuilderComponent** in the trainer/src/components/workout-builder folder with an inline template:
```javascript 
import { Component } from '@angular/core';

@Component({
    template: `<div class="row"> 
                    <div class="col-sm-3"></div> 
                    <div class="col-sm-6"> 
                        <h1 class="text-center">Workout Builder</h1> 
                    </div> 
                    <div class="col-sm-3"></div> 
                </div> `
    })
export class WorkoutBuilderComponent{
}
``` 
* And this component's view is displayed on the screen under the header using the router outlet in our **app.component.ts** view template:
```javascript 
<div class="container body-content app-container"> 
    <router-outlet></router-outlet> 
</div>
``` 
* Then we have wrapped this component in a new module named  **workout-builder.module.ts**:

```javascript 
import { NgModule }      from '@angular/core'; 
import { CommonModule } from '@angular/common'; 
 
import { WorkoutBuilderComponent } from "./workout-builder.component"; 
 
@NgModule({ 
    imports: [CommonModule], 
    declarations: [ 
        WorkoutBuilderComponent, 
    ], 
    exports: [WorkoutBuilderComponent], 
}) 
export class WorkoutBuilderModule { }
``` 
The only thing that might look different here from the other modules that we have created is that we are importing **CommonModule** instead of **BrowserModule**. This avoids our importing all of BrowserModule a second time, which would generate an error when we get to implementing lazy loading for this module. 

* Finally, we have added an import for this module to

```javascript
app.module.ts: 
  ... 
@NgModule({ 
  imports: [ 
  ... 
    WorkoutBuilderModule], 
  ... 
  ```
### Proposed Workout Builder Routes

| Route        | Description    |
| ------------- |-------------|
| /builder      | This just redirects to builder/workouts.  |
| /builder/workouts     | This lists all the available workouts. This is the landing page for Workout Builder.  |
| /builder/workout/new  | This creates a new workout. |
| /builder/workout/:id   | This edits an existing workout with the specific ID.|
| /builder/exercises   | This lists all the available exercises. |
| /builder/exercises/new  | This creates a new exercise. |
| /builder/exercises/:id  | This edits an existing exercise with the specific ID.      
### Child Routing
> **Start at [Checkpoint 4.1](https://github.com/chandermani/angular2byexample/tree/checkpoint4.1). Zip file is [here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.1.zip).**

> **Finished code at at [Checkpoint 4.2](https://github.com/chandermani/angular2byexample/tree/checkpoint4.2). Zip file is [here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.2.zip).**

In this section we'll be adding the code for child routes for the workout builder:
![alt text](ChildRoutes.png "Child Routes")

#### Adding the child routing component
* In the workout-builder directory, add a new TypeScript file named **workout-builder.routes.ts**.
* Then add the with the following imports to that file: 
```javascript
import { ModuleWithProviders } from '@angular/core'; 
import { Routes, RouterModule } from '@angular/router';

import { WorkoutBuilderComponent}  from "./workout-builder.component"; 
import { ExerciseComponent} from './exercise/exercise.component'; 
import { ExercisesComponent} from './exercises/exercises.component'; 
import { WorkoutComponent} from './workout/workout.component'; 
import { WorkoutsComponent} from './workouts/workouts.component';
``` 
* Next add the following route configuration below the imports:
```javascript
export const workoutBuilderRoutes: Routes = [
    {
        path: 'builder',
        component: WorkoutBuilderComponent,
        children: [
             {path:'', pathMatch: 'full', redirectTo: 'workouts'},
             {path:'workouts', component: WorkoutsComponent },
             {path:'workout/new',  component: WorkoutComponent },
             {path:'workout/:id', component: WorkoutComponent },
             {path:'exercises', component: ExercisesComponent},
             {path:'exercise/new', component: ExerciseComponent },
             {path:'exercise/:id', component: ExerciseComponent }
        ]
    }
];
```
* Finally, add the following export statement:
```javascript
export const workoutBuilderRouting: ModuleWithProviders = RouterModule.forChild(workoutBuilderRoutes);
```
#### Updating the WorkoutBuilder Component to support child routes
We next need to update the WorkoutBuilder component to support our new child routes. To do so change the @Component decorator for Workout Builder to: 
* remove the selector,  
* replace the reference to a templateUrl with a template reference, 
* add a <sub-nav> custom element to the template, and  
* add a <router-outlet> tag to the template.

The decorator should now look like the following: 
```javascript
@Component({ 
    template: `<div class="navbar navbar-default navbar-fixed-top second-top-nav"> 
                  <sub-nav></sub-nav> 
               </div> 
               <div class="container body-content app-container"> 
                  <router-outlet></router-outlet> 
               </div>` 
})
```  
#### Updating the WorkoutBuilder module to make it a feature module
* Update the imports in workout-builder.module.ts to the following:
```javascript
import { NgModule }      from '@angular/core';
import { CommonModule } from '@angular/common';

import { ExerciseComponent } from "./exercise/exercise.component";
import { ExercisesComponent } from "./exercises/exercises.component";
import { LeftNavExercisesComponent } from "./navigation/left-nav-exercises.component";
import { LeftNavMainComponent } from "./navigation/left-nav-main.component";
import { SubNavComponent} from './navigation/sub-nav.component';
import { WorkoutBuilderComponent } from "./workout-builder.component";
import { WorkoutComponent } from "./workout/workout.component";
import { WorkoutsComponent } from "./workouts/workouts.component";

import { workoutBuilderRouting } from './workout-builder.routes';
```
* Next update the @NgModule decorator to the following:
```javascript
@NgModule({
    imports: [
        CommonModule,
        workoutBuilderRouting
    ],
    declarations: [
        WorkoutBuilderComponent,
        WorkoutComponent,
        WorkoutsComponent,
        ExerciseComponent,
        ExercisesComponent,
        SubNavComponent,
        LeftNavExercisesComponent,
        LeftNavMainComponent
    ],
    exports: [WorkoutBuilderComponent],
})
```

#### Updating app.routes to remove the workout builder component and its route from that file.
Finally, go back to app.routes.ts and remove the import of the WorkoutBuilderComponent and its route.

**Run the app**
### Lazy Loading of Routes
> **Start at [Checkpoint 4.2](https://github.com/chandermani/angular2byexample/tree/checkpoint4.2). Zip file is [here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.21.zip).**

> **Finished code at at [Checkpoint 4.3](https://github.com/chandermani/angular2byexample/tree/checkpoint4.3). Zip file is [here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.3.zip).**

In this section we'll be adding the code for lazy loading. 
* First, modify **app.routes.ts** to add the following separate route configuration under the existing imports:
```javascript
const workoutBuilderRoutes: Routes = [
  {
    path: 'builder',
    loadChildren: 'dist/components/workout-builder/workout-builder.module#WorkoutBuilderModule'
  }
];
```
* Next update the Routes configuration to the following:
```javascript
export const routes: Routes = [
  { path: 'start', component: StartComponent },
  { path: 'workout', component: WorkoutContainerCompnent },
  { path: 'finish', component: FinishComponent },
  { path: 'history', component: WorkoutHistoryComponent },
    ...workoutBuilderRoutes,
  { path: '**', redirectTo: '/start' }
];
```
* Then go back to workout-builder.routes.ts and change the path property to an empty string:
```javascript
export const workoutBuilderRoutes: Routes = [ 
    { 
        path: '', 
. . . 
    } 
];
```
* Finally go back to the **app-module.ts** and remove the WorkoutBuilderModule import in the @NgModule configuration in that file.
* **Run the app**

### Integrating Sub and Side Level Navigation
> **Continue in [Checkpoint 4.2](https://github.com/chandermani/angular2byexample/tree/checkpoint4.2). Zip file is [here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.21.zip).**

> **Finished code at at [Checkpoint 4.3](https://github.com/chandermani/angular2byexample/tree/checkpoint4.3). Zip file is [here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.3.zip).**

#### Sub-level navigation
* Open the **sub-nav.component.html** file and change the HTML in it to the following:
```javascript 
<div> 
    <a [routerLink]="['/builder/workouts']" class="btn btn-primary"> 
        <span class="glyphicon glyphicon-home"></span> Home 
    </a> 
    <a [routerLink]="['/builder/workout/new']" class="btn btn-primary"> 
        <span class="glyphicon glyphicon-plus"></span> New Workout 
    </a> 
    <a [routerLink]="['/builder/exercise/new']" class="btn btn-primary"> 
        <span class="glyphicon glyphicon-plus"></span> New Exercise 
    </a>
</div>
``` 
#### Side Navigation
The component based nature of Angular gives us an easy way to implement these context-sensitive menus.  We can define new components for each of the menus and then import them into the components that need them.  In this case, we have three components that will need side menus – Workouts, Exercises, and Workout.

We already have files for the two menu components including template files and have imported them into the WorkoutBuilderModule.  We will now integrate these into the components that need them.   
* First, modify the template – **workouts.component.html** – to add the selector for the menu 
```javascript
<div class="container-fluid"> 
    <div id="content-container" class="row"> 
        <left-nav-main></left-nav-main> 
        <h1 class="text-center">Workouts</h1> 
    </div> 
</div>
``` 
* Then replace the placeholder text in the **left-nav-main.component.html** with the navigation links to the WorkoutsComponent and ExercisesComponent:
```javascript 
<div class="col-sm-2 left-nav-bar"> 
    <div class="list-group"> 
        <a [routerLink]="['/builder/workouts']" class="list-group-item list-group-item-info">Workouts</a> 
        <a [routerLink]="['/builder/exercises']" class="list-group-item list-group-item-info">Exercises</a> 
    </div> 
</div> 
```
* Follow the exact same steps to complete the side menu for the Exercises component:
```javascript
<div class="container-fluid"> 
    <div id="content-container" class="row"> 
        <left-nav-main></left-nav-main> 
        <h1 class="text-center">Exercises</h1> 
    </div> 
</div>
```
* For the menu for the Workout screen, the steps are the same except that you should change the left-nav-exercises.component.html to the following: 
```javascript
<div class="col-sm-2 left-nav-bar"> 
    <h3>Exercises</h3> 
</div>
``` 
We will use this template as the starting point for building out a list of exercises that will appear on the left-hand side of the screen and can be selected for inclusion in a workout. 

### Implementing workout and exercise lists
> **Start at [Checkpoint 4.3](https://github.com/chandermani/angular2byexample/tree/checkpoint4.3). Zip file is [here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.3.zip).**

> **Finished code is at [Checkpoint 4.4](https://github.com/chandermani/angular2byexample/tree/checkpoint4.4). Zip file is [here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.4.zip).**
 
 >If you are coding along be sure and update app.css that is found in the **Checkpoint 4.4** at this location [app.css](https://github.com/chandermani/angular2byexample/tree/checkpoint4.4/trainer/static/css).
#### Workout Service
* Add [workout-service.ts](https://github.com/chandermani/angular2byexample/blob/checkpoint4.4/trainer/src/services/workout-service.ts) in the trainer/src/services folder in **Checkpoint 4.4**. The code in that file should look like the following except for the implementation of the two methods setupInitialExercises and setupInitialWorkouts, which we have left out because of their length.
```javascript
import {Injectable} from '@angular/core'; 
import {ExercisePlan} from './model'; 
import {WorkoutPlan} from './model'; 
import {Exercise} from "./model"; 
 
@Injectable() 
export class WorkoutService { 
    workouts: Array<WorkoutPlan> = []; 
    exercises: Array<Exercise> = []; 
 
    constructor() { 
        this.setupInitialExercises(); 
        this.setupInitialWorkouts(); 
    } 
 
    getExercises(){ 
        return this.exercises; 
    } 
 
    getWorkouts(){ 
        return this.workouts; 
    } 
    setupInitialExercises(){ 
 // implementation of in-memory store. 
    } 
 
    setupInitialWorkouts(){ 
 // implementation of in-memory store. 
    } 
}} 
```
Open services.module.ts in the same folder and then import the workout-service and add it as a provider:
```javascript
---- other imports ---- 
import { WorkoutService } from "./workout-service"; 
 
@NgModule({ 
    imports: [], 
    declarations: [], 
    providers: [ 
        LocalStorage, 
        WorkoutHistoryTracker, 
        WorkoutService], 
})
``` 
This registers the WorkoutService as a provider with Angular’s Dependency Injection framework. 

#### Workout and exercise list components
First, open the workouts.component.ts file in the trainer/src/components/workout-builder/workouts folder and update the imports as follows: 
```javascript
import { Component, OnInit} from '@angular/core'; 
import { Router } from '@angular/router'; 
 
import { WorkoutPlan } from "../../../services/model"; 
import { WorkoutService } from "../../../services/workout-service";
```
This new code imports OnInit from Angular core as well as the Router, WorkoutService and the WorkoutPlan type . 
Next replace the class definition with the following code:
```javascript  
export class WorkoutsComponent implements OnInit { 
    workoutList:Array<WorkoutPlan> = []; 
 
    constructor( 
        public router:Router, 
        public workoutService:WorkoutService) {} 
 
    ngOnInit() { 
        this.workoutList = this.workoutService.getWorkouts(); 
    } 
 
    onSelect(workout: WorkoutPlan) { 
        this.router.navigate( ['./builder/workout', workout.name] ); 
    } 
}
``` 
#### Workout and exercist list views
##### Workout list view
To get the view working, open workouts.component.html and add the following markup:
```javascript 
<div class="container-fluid"> 
    <div id="content-container" class="row"> 
        <left-nav-main></left-nav-main> 
        <h1 class="text-center">Workouts</h1> 
        <div class="workouts-container"> 
            <div *ngFor="let workout of workoutList|orderBy:'title'" class="workout tile" (click)="onSelect(workout)"> 
                <div class="title">{{workout.title}}</div> 
                <div class="stats"> 
                    <span class="duration" title="Duration"><span class="glyphicon glyphicon-time"></span> - {{workout.totalWorkoutDuration()|secondsToTime}}</span> 
                    <span class="length pull-right" title="Exercise Count"><span class="glyphicon glyphicon-th-list"></span> - {{workout.exercises.length}}</span> 
                </div> 
            </div> 
        </div> 
    </div> 
</div>
```
You will also need to move the **secondsToTime pipe** from the workout-runner folder in the trainer/src/components directory into the shared folder in the same directory and include it in the SharedModule. Then add the SharedModule to the WorkoutBuilderModule as an additional import.
* Finally, we bind the click event to the following onSelect method that we add to our component:
```javascript 
 onSelect(workout: WorkoutPlan) { 
     this.router.navigate( ['./builder/workout', workout.name] ); 
 }
 ``` 
##### Exercises list view 
* For the Exercises component, we will follow an approach that is almost identical to what we did to display the list of workouts in the Workouts component. So we won’t show that code here.  Just add the files for **exercise.component.ts** and **exercise.component.html** from [Checkpoint 4.4](https://github.com/chandermani/angular2byexample/tree/checkpoint4.4). Zip file is [here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.4.zip). 
* When you are done copying the files, click on the Exercises link on the left navigation to load the 12 exercises that we have already configured in WorkoutService.
* In the final list view, we will add is a list of exercises that will display in the left context menu for the Workout Builder screen. This view is loaded in the left navigation when we create or edit a workout. Using Angular’s component based approach, we will update the leftNavExercisesComponent and its related view to provide this functionality. Again we won’t show that code here.  Just add the files for **left-nav-exercises.component.ts** and **left-nav-exercises.component.html** from the trainer/src/components/navigation folder in [Checkpoint 4.4](https://github.com/chandermani/angular2byexample/tree/checkpoint4.4). Zip file is [here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.4.zip).   
### Building a Workout
> **Start at [Checkpoint 4.4](https://github.com/chandermani/angular2byexample/tree/checkpoint4.4). Zip file is [here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.4.zip).**

> **Finished code is at [Checkpoint 4.5](https://github.com/chandermani/angular2byexample/tree/checkpoint4.5). Zip file is [here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.5.zip).** 
#### Finishing left nav
At the end of the previous section, we updated the left navigation view for the Workout component to show a list of exercises.  Our intention was to let the user click on an arrow next to an exercise to add it to the workout.  At the time, we deferred implementing the addExercise method in the LeftNavExercisesComponent that was bound to that click event.  Now we will go ahead and do that. 
#### Adding Workout Builder Service
The WorkoutBuilderService tracks the state of the workout being built. It: 
* Tracks the current workout   
* Creates a new workout 
* Loads the existing workout 
* Saves the workout

Follow these steps to implement the Workout Builder Service:
* Copy **workout-builder-service.ts** from the workout-builder/builder-services folder under trainer/src/components in **[Checkpoint 4.5](https://github.com/chandermani/angular2byexample/tree/checkpoint4.5)**.

* While we normally make services available application wide, the WorkoutBuilderService will only be used in the Workout Builder feature. Therefore, instead of registering it with the providers in AppModule, we will register it in the provider array of the WorkoutBuilderModule as follows (after adding it as an import at the top of the file):
```javascript
providers: [ 
    WorkoutBuilderService, 
    . . . 
] 
```
Let’s look at some of the relevant parts of the service. 
WorkoutBuilderService needs the type definitions for WorkoutPlan, Exercise and the WorkoutService, so we import these into the component:
```javascript 
import { WorkoutPlan, Exercise } from '../../../services/model'; 
import { WorkoutService } from "../../../services/workout-service";
``` 
WorkoutBuilderService has a dependency on WorkoutService to provide persistence and querying capabilities.  We resolve this dependency by injecting the WorkoutService into the constructor for the WorkoutBuilderService:
```javascript 
constructor(public workoutService:WorkoutService){}
``` 
WorkoutBuilderService also needs to track the workout being built. We use the buildingWorkout property for this. The tracking starts when we call the startBuilding method on the service:
```javascript 
startBuilding(name: string){ 
    if(name){ 
        this.buildingWorkout = this.workoutService.getWorkout(name) 
        this.newWorkout = false; 
    }else{ 
        this.buildingWorkout = new WorkoutPlan("", "", 30, []); 
        this.newWorkout = true; 
    } 
    return this.buildingWorkout; 
} 
```
#### Adding exercises using ExerciseNav
To add exercises to the workout we are building, we just need to import and inject the WorkoutBuilderService into the LeftNavExercisesComponent and call its addExercise method passing the selected exercise as a parameter:
```javascript 
constructor( 
    public workoutService:WorkoutService, 
    public workoutBuilderService:WorkoutBuilderService) {} 
. . . 
addExercise(exercise:Exercise) { 
    this.workoutBuilderService.addExercise(new ExercisePlan(exercise, 30)); 
}
```
#### Implement the Workout Component
The Workout component is associated with two routes/views namely /builder/ workout/new and /builder/workout/:id. These routes handle both creating and editing workout scenarios. The first job of the component is to load or create the workout that it needs to manipulate. 
#### Route parameters
The WorkoutComponent is associated with two routes namely **/builder/workout/new** and **/builder/workout/:id**. The difference in these two routes lies at what is at the end of these routes – in one case it is **/new** and in the other **/:id**. These are called Route Parameters.  The **:id** in the second route is a token for a route parameter. The router will convert the token to the id for the workout component.  

As we saw earlier this means that the URL that will be passed to the component in the case of the 7 Minute Workout will be **/builder/workout/7MinuteWorkout**.

How do we know that this is the workout name is the right parameter for the id?  As you recall, when we set up the event for handling a click on the Workout tiles on the Workouts screen that takes us to the Workout screen, we designated the workout name as the parameter for the id like so:
```javascript 
 onSelect(workout: WorkoutPlan) { 
     this.router.navigate( ['./builder/workout', workout.name] ); 
 }
 ```  
#### Route guards
Copy workout.guard.ts from the workout-builder/workout folder under trainer/src/components in **[Checkpoint 4.5](https://github.com/chandermani/angular2byexample/tree/checkpoint4.5).** Zip file is **[here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.5.zip)**. You will see the following code:
```javascript 
import { Injectable } from '@angular/core'; 
import { ActivatedRouteSnapshot, CanActivate, Router, RouterStateSnapshot } from '@angular/router'; 
 
import { WorkoutPlan } from "../../../services/model"; 
import { WorkoutService } from "../../../services/workout-service"; 
 
@Injectable() 
export class WorkoutGuard implements CanActivate { 
    publicworkout: WorkoutPlan; 
 
    constructor( 
        public workoutService: WorkoutService, 
        public router: Router) {} 
 
    canActivate( 
        route: ActivatedRouteSnapshot, 
        state: RouterStateSnapshot 
    ) { 
        this.workout = this.workoutService.getWorkout(route.params['id']); 
        if(this.workout){ return true; } 
        this.router.navigate(['/builder/workouts']); 
        return false; 
    } 
}
```  
The final step in implementing the WorkoutGuard is to add it to the route configuration for the WorkoutComponent. So update **workout-builder.routes.ts** as follows:
```javascript 
export const workoutBuilderRoutes: Routes = [ 
    { 
        path: '', 
        component: WorkoutBuilderComponent, 
        children: [ 
             {path:'', pathMatch: 'full', redirectTo: 'workouts'}, 
             {path:'workouts', component: WorkoutsComponent }, 
             {path:'workout/new',  component: WorkoutComponent }, 
             {path:'workout/:id', component: WorkoutComponent, canActivate: [WorkoutGuard] }, 
             {path:'exercises', component: ExercisesComponent}, 
             {path:'exercise/new', component: ExerciseComponent }, 
             {path:'exercise/:id', component: ExerciseComponent } 
        ] 
    } 
];
``` 
#### Implement the Workout Component (cont.)
Copy the **workout.component.ts** file from the workout-builder/workout folder under trainer/src/components in **[Checkpoint 4.5](https://github.com/chandermani/angular2byexample/tree/checkpoint4.5.Zip)** file is **[here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.5.zip)**. (Also copy **workout-builder.module.ts** from the workout-builder folder. We’ll discuss the changes in this file a little later when we get to Angular forms.)

Open workout.component.ts and you’ll see that we have added a constructor that injects the ActivatedRoute and the WorkoutBuilderService:
```javascript 
    constructor( 
    public route: ActivatedRoute, 
    public workoutBuilderService:WorkoutBuilderService){ }
``` 
In addition, we have added the following ngOnInit method:
```javascript  
ngOnInit() { 
    this.sub = this.route.params.subscribe(params => { 
        let workoutName = params['id']; 
        if (!workoutName) { 
            workoutName = ""; 
        } 
        this.workout = this.workoutBuilderService.startBuilding(workoutName); 
    }); 
} 
```
#### Implementing the Workout Template
Now copy the workout.component.html files from the workout-builder/workout folder under trainer/src/components in **[Checkpoint 4.5](https://github.com/chandermani/angular2byexample/tree/checkpoint4.5)**. Zip file is **[here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.5.zip)**. 

Run the app, navigate to /builder/workouts, and double-click on the 7 Minute Workout tile. This should load the 7 Minute Workout details.

## Forms


## Angular Forms

### Template Driven Forms
In this section, we will be starting with Checkpoint 4.5

which introduces the following:
* Steps to convert a normal html5 form to an angular form
* FormsModule
* NgForms Directive
* NgModel Directive
* Validations

### Template Driven Forms - Features
* Developing the Form in the html template using NgForms and NgModel directives.
* Validation logic is handled in the html form using some NgModel and angular css classes.
* Two way databinding is enabled so that the model(component.ts) and the html form elements are always in sync.
* Default form submission can be supressed and a special NgSubmit event handler is bound to the component function.

### Step1: Go to the checkpoint4.5 or you can download zip  file from [GitHub](https://github.com/chandermani/angular2byexample/archive/checkpoint4.5.zip) 

### Step2: Import angular forms module.
```javascript
//open the systemjs.config.js file in the trainer folder 
//and add forms module
var ngPackageNames = [ 
    'common', 
    'compiler', 
    'core', 
    'forms',   //forms module import.
    'http', 
    'platform-browser', 
    'platform-browser-dynamic', 
    'router', 
    'testing' 
  ]; 
```

### Step3: import the forms module in the your component module.

>In our lab today, open workout-builder.module.ts in trainer/src/components/workout-builder folder and refer the forms module.

```javascript
//
@NgModule({
    imports: [ 
        CommonModule, 
        FormsModule, //forms module reference for Template Driven forms.
        SharedModule, 
        workoutBuilderRouting 
    ]
});

// Once this is refereed, all components in this module can have access to forms directives.
``` 
> We will be using the NgForm and ngModel directives in the html templates.

### Using NgForm
``` html
<form #f="ngForm" (ngSubmit)="save(f.form)" novalidate>

</form>
```
> #f - template reference variable.
> 
> novalidate - disables the default form submit behavior
> 
>(ngSubmit) - form submit handler - calls the save method in the compoenent.

### ngModel
* Fundamental directive in forms module.
* Supports two data binding.
* Manages the state of the form like valid/invalid.

> Open workout-component.html and look for ngModel.
>
> It is applied only to input elements.

```html
<input name="workoutName" id="workout-name" [(ngModel)]="workout.name"> 
```
> look for **[()]** - Two way databinding 

### To check two way databinding,
#### 

Add the interpolation to next to the input tag.

```html
<input name="workoutName" id="workout-name" [(ngModel)]="workout.name"> 
{{workout.name}}
```

### Validation: ngModel is the main directive that is used to implement validation

#### Out of box validations supported by angular.
* required
* min
* max
* pattern

### To get started with validation, assign a local variable to ngModel and add required attribute
```html
<input #name="ngModel" name="workoutName" id="workout-name" 
[(ngModel)]="workout.name" required> 
{{workout.name}}
```
> Look for **#name** - local variable.
>
> Look for **required**

### How to check the state




[Kumanan]
### Model Driven Forms
> **Start at [Checkpoint 4.5](https://github.com/chandermani/angular2byexample/tree/checkpoint4.5). Zip file is [here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.5.zip).**

> **Finished code at at [Checkpoint 4.6](https://github.com/chandermani/angular2byexample/tree/checkpoint4.6). Zip file is [here](https://github.com/chandermani/angular2byexample/archive/checkpoint4.6.zip).**   
#### Getting Started
* To get started, open workout-builder.module.ts and add an import for the ReactiveFormsModule:
```javascript 
import { FormsModule, ReactiveFormsModule }   from '@angular/forms'; 
 ... 
@NgModule({ 
    imports: [ 
        CommonModule, 
        FormsModule, 
        ReactiveFormsModule, 
        SharedModule, 
        workoutBuilderRouting 
    ],
```
* Next copy exercise-builder-service.ts from the workout-builder/builder-services folder under trainer/src/components in [Checkpoint 4.6](https://github.com/chandermani/angular2byexample/tree/checkpoint4.6) and import it into workout-builder.module.ts:
```javascript 
import { ExerciseBuilderService } from "./builder-services/exercise-builder-service";
``` 
* Then add it as an additional provider to the providers array in that same file:
```javascript 
@NgModule({ 
 . . . 
    providers: [ 
        ExerciseBuilderService, 
        ExerciseGuard, 
        WorkoutBuilderService, 
        WorkoutGuard 
    ] 
})
```
You will notice here that we also have added ExerciseGuard as a provider. We won’t be covering that here, but you should copy it from the exercise folder as well and also copy the updated workout-builder.routes.ts that adds that as a route guard for the navigation to the ExerciseComponent
* Now open exercise.component.ts and add the following import statements at the top of the file:
```javascript 
import { Component, OnInit, OnDestroy } from '@angular/core';
import { ActivatedRoute, Router } from '@angular/router';
import { Validators, FormArray, FormGroup, FormControl, FormBuilder } from '@angular/forms';

import { ExerciseBuilderService } from "../builder-services/exercise-builder-service";
import { AlphaNumericValidator } from "../alphanumeric-validator";
import { Exercise} from "../../../services/model";
```
* Then inject the FormBuilder as well as the (as well as the Router, ActivatedRoute and the ExerciseBuilderService) into the constructor of our class:
```javascript 
constructor( 
    public route: ActivatedRoute, 
    public router: Router, 
    public exerciseBuilderService:ExerciseBuilderService, 
    public formBuilder: FormBuilder 
){}
```     
#### Using the FormBuilder API
* Go ahead and add the ngOnInit lifecycle hook to our class as follows:
```javascript 
ngOnInit():any{ 
    this.sub = this.route.params.subscribe(params => { 
        let exerciseName = params['id']; 
        if (exerciseName === 'new') { 
            exerciseName = ""; 
        } 
        this.exercise = this.exerciseBuilderService.startBuilding(exerciseName); 
    }); 
    this.buildExerciseForm(); 
} 
```
* Now let’s implement the buildExerciseForm method by adding the following code:
```javascript 
buildExerciseForm(){ 
    this.exerciseForm = this.formBuilder.group({ 
        'name': [this.exercise.name, [Validators.required, AlphaNumericValidator.invalidAlphaNumeric]], 
        'title': [this.exercise.title, Validators.required], 
        'description': [this.exercise.description, Validators.required], 
        'image': [this.exercise.image, Validators.required], 
        'nameSound': [this.exercise.nameSound], 
        'procedure': [this.exercise.procedure], 
        'videos': this.addVideoArray() 
    }) 
}
```
* To construct this new FormArray, add the following addVideoArray method to our class:
```javascript 
addVideoArray(){ 
    if(this.exercise.videos){ 
        this.exercise.videos.forEach((video : any) => { 
            this.videoArray.push(new FormControl(video, Validators.required)); 
        }); 
    } 
    return this.videoArray; 
}
```
#### Saving the form
* Add the following onSubmit method in our code:
```javascript 
onSubmit(formExercise:FormGroup){ 
    this.submitted = true; 
    if (!formExercise.valid) return; 
    this.mapFormValues(formExercise); 
    this.exerciseBuilderService.save(); 
    this.router.navigate(['/builder/exercises']); 
} 
``` 
* Finally add the following method, which assigns the values from our form to the exercise that will be saved:
```javascript 
mapFormValues(form: FormGroup){ 
    this.exercise.name = form.controls['name'].value; 
    this.exercise.title = form.controls['title'].value; 
    this.exercise.description = form.controls['description'].value; 
    this.exercise.image = form.controls['image'].value; 
    this.exercise.nameSound = form.controls['nameSound'].value; 
    this.exercise.procedure = form.controls['procedure'].value; 
    this.exercise.videos = form.controls['videos'].value; 
} 
```
#### Custom validators
Add a TypeScript file with the name **alphanumeric-validator.ts** in the **workout-builder** folder under **trainer/src/components**. 

In that file first add the following import: 
```javascript
import FormControl from @angular/forms
```
Then add the following class to that file:
```javascript 
export class AlphaNumericValidator { 
    static invalidAlphaNumeric(control: FormControl):{ [key:string]:boolean } { 
        if ( control.value.length && !control.value.match(/^[a-z0-9]+$/i) ){ 
            return {invalidAlphaNumeric: true }; 
        } 
        return null; 
    } 
}
``` 
  
#### Adding the Form Model to our HTML View
Copy in the code for **exercise.component.html** from [Checkpoint 4.6](https://github.com/chandermani/angular2byexample/tree/checkpoint4.6).
Open exercise.component.html and notice the following form tag
```javascript 
<form [formGroup]="exerciseForm" (ngSubmit)="onSubmit(exerciseForm)" novalidate>
```  
#### Adding Form Controls to our Form inputs 
Next we start constructing the inputs for our form.  We’ll start with the input for the name of our exercise:
```javascript  
<input name="name" formControlName="name" class="form-control" id="name" placeholder="Enter exercise name. Must be unique.">
```  
#### Adding Validation 
The next thing that we do is add a validation message to the control that will display in the event of a validation error:
```javascript 
<label *ngIf="exerciseForm.controls.name.hasError('required') && (exerciseForm.controls.name.touched || submitted)" class="alert alert-danger validation-message">Name is required</label>
```  
But wait a minute, how can we validate this input?  Haven’t we have removed the required attribute from our the tag? If you look back at the code for the form model, you see the following mapping for the name control:
```javascript 
'name': [this.exercise.name, Validators.required],
``` 
The second element in the mapping array assigns the required validator to the name form control.   
#### Adding Dynamic Form Controls 
 Since we don’t know how many videos the user may want to add, we will have to build the input fields for these videos dynamically as the user clicks the Add Video button.  Here’s how it will look:

![alt text](Dynamic.PNG "Dynamic Controls")

We first use ngFor to loop through our list of videos. Then we add assign the index in our videos to a local variable i. No surprises so far.
```javascript  
<div *ngFor="let video of videoArray.controls; let i=index" class="form-group">
```  
Inside the loop we do three things. First, we add a button to allow the user to delete a video:
```javascript 
<button type="button" (click)="deleteVideo(i)" title="Delete this video." class="btn alert-danger pull-right"> 
    <span class="glyphicon glyphicon-trash text-danger"></span> 
</button>
```  
We bind a deleteVideo method in our component class to the button’s click event and pass it the index of the video being deleted. 
Next we dynamically add a video input field for each of the videos currently in our exercise:
```javascript  
<input type="text" class="form-control" [formControlName]="i" placeholder="Add a related youtube video identified."/>
``` 
We then add a validation message for each of the video input fields 
```javascript 
<label *ngIf="exerciseForm.controls['videos'].controls[i].hasError('required') && (exerciseForm.controls['videos'].controls[i].touched || submitted)" class="alert alert-danger validation-message">Video identifier is required</label>
```  
The validation message follows the same pattern for displaying the message that we have used elsewhere in this chapter. We drill into the exerciseFormControls group to find the particular control by its index. Again the syntax is verbose but easy enough to understand.

#### Integrating the custom validator into our forms
So how do we plug our custom validator into our form? If we are using model driven forms, the answer is pretty simple.  We add it just like a built-in validator when we build our form in code.  Let’s do that.

* Open exercise.component.ts and first add an import for our custom validator:
```javascript 
import {AlphaNumericValidator} from "./alphanumeric-validator";
``` 
* Then, modify the form builder code to add the validator to the name control:
```javascript 
buildExerciseForm(){ 
    this.exerciseForm = this._formBuilder.group({ 
'name': [this.exercise.name, [Validators.required, AlphaNumericValidator.invalidAlphaNumeric]], 
. . . [other form controls] . . . 
    }); 
} 
```
* The final step is to incorporate the appropriate validation message for the control in to our template.  Open workout.component.html and add the following label just below the label that displays the message for the required validator:
```javascript 
<label *ngIf="exerciseForm.controls.name.hasError('invalidAlphaNumeric') && (exerciseForm.controls.name.touched || submitted)" class="alert alert-danger validation-message">Name must be alphanumeric</label>
```
### Homework
* Read Chapter 5
* Grab the repo for Checkpoint 5.1
* Sign up for a [Mongo Lab](https://mlab.com/) account  