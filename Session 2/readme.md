# Session 2 - Routing, Forms

Slides for the session can be found at [kevinhennessy.github.io/angular2/Session2](http://kevinhennessy.github.io/angular2/Session2)

In this session we will be working with a series of checkpoints for Chapter 4 of the book.The starting repository for the book chapter Checkpoint 4.1 which is located at [github.com/chandermani/angular2byexample/tree/checkpoint4.1](https://github.com/chandermani/angular2byexample/tree/checkpoint4.1). 

## Pre-reqs
This session assumes you have read chapter 4 of the book.

## Getting set up with Personal Trainer
[David]
## Routing
### Essentials
[David]
### Getting Started
In this section, we will be starting with Checkpoint 4.1
which introduces the following:
* Sharing the workout model as a service
* Personal trainer layout
* Adding skeleton files for each component
* Personal trainer navigation with routes
* Code changes:
### Child Routing
In this section we'll be adding the code for child routes. The finished version is at Checkpoint 4.2
#### Adding the child routing component
* In the workout-builder directory, add a new TypeScript file named workout-builder.routes.ts.
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
* Next update the @NgModule decoratore to the following:
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
### Lazy Loading
In this section we'll be adding the code for lazy loading. The finished version is at Checkpoint 4.3.
* First, modify app.routes.ts to add the following separate route configuration under the existing imports:
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
* Finally go back to the app-module.ts and remove the WorkoutBuilderModule import in the @NgModule configuration in that file.
## Forms



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
    'forms',   
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
        FormsModule, //forms module reference
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

### Validation: 
* required
* min
* max
* pattern


[Kumanan]
### Model Driven Forms
[Kevin]
### Homework
* Read Chapter 5
* Grab the repo for Checkpoint 5.1
* Sign up for a Mongo Lab account 