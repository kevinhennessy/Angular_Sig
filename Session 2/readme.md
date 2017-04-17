# Session 2 - Routing, Forms

Slides for the session can be found at [kevinhennessy.github.io/angular2/Session2](http://kevinhennessy.github.io/angular2/Session2)

In this session we will be working with a series of checkpoints for Chapter 4 of the book.The starting repository for the book chapter Checkpoint 4.1 which is located at [github.com/chandermani/angular2byexample/tree/checkpoint4.1](https://github.com/chandermani/angular2byexample/tree/checkpoint4.1). 

## Pre-reqs
This session assumes you have read chapter 4 of the book.

## Getting set up with Personal Trainer

### Project Setup
Make sure you have git installed and in your favorite local folder: clone the source repository, install gulp, install packages, checkout the starting branch, and run the app:
```
$ git clone https://github.com/chandermani/angular2byexample.git

$ npm install -g gulp

$ cd angular2byexample/trainer

$ npm install

$ git checkout checkpoint4.1

$ gulp play
```

Now open the application folder in your favorite javascript editor (VS Code, Sublime, Atom, WebStorm, etc).

**Tip: It is strongly encouraged for you to use git to pull the application code to your local workstation.**  
This will make it much easier to track your changes, move between checkpoints, and restart if necessary.  However, if you don't have git installed you can pull down the code base as a zip file:
[https://github.com/chandermani/angular2byexample/archive/checkpoint4.1.zip](https://github.com/chandermani/angular2byexample/archive/checkpoint4.1.zip)  &leftarrow; You can substitute a given checkpoint number in the URL to download the corresponding zip file.

### Using git Branches for Checkpoints
The code in the source repository progresses through the exercises outlined in the book using git branches.  As we proceed through these exercises below we will use these branches walk through different steps in the process.  If you get behind or can't get one section working, you can always catch up by moving to the next branch.  As shown above you can move to a new branch using the git checkout command.  Ex:
```
$ git checkout checkpoint4.1
```

During this presentation we will be working with checkpoints between 4.1 and 4.6.

### Use of Gulp for builds

### Personal Trainer
7 Minute Workout is an exercise/workout plan that requires us to perform a set of twelve exercises in quick succession within the seven minute time span. 7 Minute Workout has become quite popular due to its benefits and the short duration of the workout. We cannot confirm or refute the claims but doing any form of strenuous physical activity is better than doing nothing at all. If you are interested to know more about the workout, then check out [http://well.blogs.nytimes.com/2013/05/09/the-scientific-7-minute-workout/](http://well.blogs.nytimes.com/2013/05/09/the-scientific-7-minute-workout/).

### Project Organization
The application is organized based on the Angular style guidelines:  


- trainer
    - src 
        - components 
            - features				
        - services
            - shared services
        - static
            - static files (styles, images, audio)

### Use of Gulp for builds
Gulp is used in the book and this presentation as a rudimentary build system for our application.  It provides a build pipeline which coordinates the typescript transpilation, startup of the web server, and live reloading.  This is a very simple build pipeline and is used only to facilitate the learning of Angular concepts.  A detailed discussion of the build pipeline is outside the scope of this presentation.  There are a myriad of choices for setting up Angular 2 build pipelines and you would likely want to use something different in your production application.  Here is more information about available tools and recommendataions from the Angular team:

[https://angular.io/docs/ts/latest/guide/deployment.html](https://angular.io/docs/ts/latest/guide/deployment.html)

### 
## Routing
### Essentials
Angular supports SPA development using its routing infrastructure. This infrastructure tracks browser URLs, enables hyperlink generation, exposes routing events, and provides a set of directives/components for view.

There are four major framework pieces that work together to support the Angular routing infrastructure:

- **The Router (Router)**: The primary infrastructure piece that actually provides component navigation.
- **Routing configuration (Route)**: The component router is dependent upon the routing configuration for setting up routes.
- **RouterOutlet component**: The RouterOutlet component is the placeholder container (host) where route-specific views are loaded.
- **RouterLink directive**: This generates hyperlinks that can be embedded in the anchor tags for navigation

We setup routing in our application by making the following changes:

- package.json
    - @angular/router is added to provide routing capabilites.
- systemjs.config.js 
    - Update SystemJS to load the router module.
- index.html 
    - Add base html tag so ng routing will know the root of the site.
- app.routes.ts 
    - Route configuration for the app.
    - A path with ** denotes a catch-all path or the wildcard route for the app.
- app.component.ts
    - Template needs to contain \<router-outlet>\</router-outlet>
    - RouterOutlet is an Angular component directive that acts as a placeholder for a child component to load on route change. It integrates with the router to load the appropriate component based on the current browser URL and the route definition.
- start.html 
    - Add a link to the workout route.  Notice the routeLink property binding.  This is used throughout the application.  The start template is just one example.
- workout-runner.component.ts 
    - Navigates programatically using the router service: this.router.navigate( ['/finish'] );
    
### Getting Started
In this section, we will be starting with Checkpoint 4.1
which introduces the following:
* Sharing the workout model as a service
* Personal trainer layout
* Adding skeleton files for each component
* Personal trainer navigation with routes
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
### Lazy Loading of Routes
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
### Implementing Sub and Side Level Navigation
We'll review the code in Checkpoint 4.3
### Implementing workout and exercise lists
We'll review the code in Checkpoint 4.4
* Workout Service
* Workout and exercise list components
* Workout and exercist list views
### Building a Workout
We'll review the relevant code in Checkpoint 4.5
* Finishing left nav
* Adding Workout Builder Service
* Adding exercises using ExerciseNav
* Route parameters
* Route guards
* Implement the Workout Component
## Forms
### Template Driven Forms
[Kumanan]
### Model Driven Forms
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
* Next copy exercise-builder-service.ts from the workout-builder/builder-services folder under trainer/src/components in checkpoint 4.6. and import it into workout-builder.module.ts:
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
Add a TypeScript file with the name alphanumeric-validator.ts. In that file first import FormControl from @angular/forms, then add the following class to that file:
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
Copy in the code for exercise.component.html from checkpoint 4.6.

### Homework
* Read Chapter 5
* Grab the repo for Checkpoint 5.1
* Sign up for a Mongo Lab account 