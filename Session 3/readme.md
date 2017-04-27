# Session 3 - Dependency Injection, Http Module and RxJs
Slides for the session can be found at [kevinhennessy.github.io/angular2/Session3](http://kevinhennessy.github.io/angular2/Session3)

In this session we will be working with a series of checkpoints for Chapter 5 of the book.The starting repository for the book chapter Checkpoint 5.1 which is located at [github.com/chandermani/angular2byexample/tree/checkpoint5.1](https://github.com/chandermani/angular2byexample/tree/checkpoint5.1). 

## Pre-reqs
This session assumes you have read chapter 5 of the book.

## Depedency Injection
### Why Dependency Injection
* It's a coding pattern in which a class receives its dependencies from external sources rather than creating them itself.
    * It is a way to have one class that gets a reference to another class that is injected somehow.
    * A class should have no knowledge of what it needs to use – we don’t want to know a ton about our collaborators – it's not its job to instantiate its collaborators.
* Testing is another issue – we want to just test the code within the class not its dependencies 
* Injecting is done through constructor assignment in our components.

### Angular Dependency Injection -- Chapter 2 -- pp 130-141
* Angular has its own dependency injection framework.
* It uses providers for dependency injection.
    * Dependency injector creates the instance
* Where to set providers
    * In the root module
    * In other modules
        * will still be available throughout the app
        * exception for lazy loaded modules
    * In component decorator (Chapter 6 -- pp 334-37) using
        * **providers** property - available to its view children, content children and their descendants
        * **viewProviders** property - can only be injected in the view children
* Services are assigned to providers
    * A service is nothing more than a class in Angular. 
    * It remains nothing more than a class until you register it with an Angular injector.        
* We assign services to providers in four ways
    1. Default that works in 99% of cases.
        * See the example in **services.module.ts** in the trainer/src/components/services folder.
        * We import the service class into the root or other module (not an instance -- the dependency injector does that). 
        ```javascript
        import { WorkoutService } from './workout-service';
        ```
        * In the providers array we list the services class type.
        ```javascript
        providers: [
            ...
            WorkoutService],
        ```
        * You can also use a long hand version:
            ```javascript
            providers: [
                ...
                { provide:WorkoutService, useClass:WorkoutService],
            ```
            The first value is the token, the second the class that it points to.

            You can use this in testing to swap in mock services:
            ```javascript
            beforeEach( async(() => {
                TestBed.configureTestingModule({
                    imports: [ HttpModule ],
                    providers: [
                        WorkoutService,
                        { provide: XHRBackend, useClass: MockBackend }
                    ]
                })
            }));
            ``` 
    2. useValue
    3. useFactory is another option
    4. useExisting allows us to use an existing object instance
* How do we inject dependencies into our components?
    * We typically inject them in the constructor like so:



    
