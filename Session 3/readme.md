# Session 3 - Dependency Injection, Http Module and RxJs
Slides for the session can be found at [kevinhennessy.github.io/angular2/Session3](http://kevinhennessy.github.io/angular2/Session3)

In this session we will be working with a series of checkpoints for Chapter 5 of the book.The starting repository for the book chapter Checkpoint 5.1 which is located at [github.com/chandermani/angular2byexample/tree/checkpoint5.1](https://github.com/chandermani/angular2byexample/tree/checkpoint5.1). 

## Pre-reqs
This session assumes you have read chapter 5 of the book.

## Depedency Injection
### What is Dependency Injection?
* It's a coding pattern in which a class receives its dependencies from external sources rather than creating them itself.
    * A class should have no knowledge of what it needs to use – we don’t want to know a ton about our collaborators – it's not its job to instantiate its collaborators.
    * Testing is another issue – we want to just test the code within the class not its dependencies 
* **Dependency injection** is a way to have one class that gets a reference to another class that is injected somehow. 
* The "somehow" is a **dependency injection framework**.
### Angular Dependency Injection -- Chapter 2 -- pp 130-141
#### Angular has its own built-in dependency injection framework.
* It uses **providers** for dependency injection.
* Each component,  directive and module has a **provider** property.
* **Services** are assigned to providers.
* The **provider** creates an instance of the service to be injected.
* The **service** is then injected into a component (or directive) through constructor assignment.
#### Getting Started with Angular Dependency Injection
* You don't have to create an Angular injector. Angular creates an application-wide injector for you during the bootstrap process.
 * You do have to configure the injector by registering the providers that create the services the application requires.
#### Registering providers
* In an **NgModule**
    * A provider registered here will be accessible in the entire application
    * There is an exception for lazy loaded modules. For a lazy-loaded module, Angular creates a child injector and adds the module's providers to the child injector.
* In a component or directive decorator (Chapter 6 -- pp 334-37) using
    * **providers** property - available to its view children, content children and their descendants
    * **viewProviders** property - can only be injected in the view children
* In general, prefer registering feature-specific providers in modules to registering in components.
* Do not specify app-wide singleton providers in a shared module. A lazy-loaded module that imports that shared module makes its own copy of the service.
 
#### What are services?
* You can take any JavaScript object and use it as a service
* But typically a service is a class
* A service is a singleton    
* It remains nothing more than a class until you register it with a provider.
#### Four ways to register services with providers
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
            WorkoutService
            ],
        ```
    * You can also use a long hand version:
        ```javascript
        providers: [
            ...
            { provide:WorkoutService, useClass:WorkoutService}
            ],
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
2. useValue see toastr example - 
3. useFactory is another option
4. useExisting allows us to use an existing object instance
* How do we inject dependencies into our components?
    * First import the class for our dependency from its file:
    ```javascript
    import { WorkoutService } from "../../../services/workout-service";    
    ```

    * Then inject it into the constructor like so:
    ```javascript
    constructor(public workoutService:WorkoutService){}
    ```
    * Angular uses the TypeScript type declaration (here WorkoutService) as a token to lookup our service in the DI container.
        * You cannot use interfaces with DI in Angular
    * Angular first looks in the component for a provider for the type and if not found then moves up the component tree all the way to the root injector to find it.
    * OpaqueToken & @Inject() ties in with useValue??
    * @Injectable