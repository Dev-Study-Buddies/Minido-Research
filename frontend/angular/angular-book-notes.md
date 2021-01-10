# Angular Learning
 From the book *Writing Your First Angular Web application* book

 ## Requirements

 * Node
 * NPM
 * Typescript
 * Angular CLI

## Some Angular CLI Commands

* `ng new <project-name>`                  - creates new project from scratch
* `ng serve`                               - runs application 
* `ng generate component <component-name>` - generates component and places the folder in `/app` folder

## Files and File structure

* `index.html` - this is where the application is rendered with `<app-root>`
* `app/app.component.html` defines the HTML/ components that will injected into the webpage (through `index.js`)
* `angular.json` - defines project configuration, such as main file
* `main.ts` - entry point for the app and bootstraps the application
  * boots an angular module `AppModule`
* `AppModule` specifies which component to use as the top level component; also declares the components 


## Modules

* `Input` - Allows you to pass in a vlaue from the parent template
* `HostBinding` - `@HostBinding()` decorator; used to set properties on the host element

## Terminologies

* *Component* - HTML tags that have custom functionality attached to them
* *Component Host* - the element that the component is attached to; Used to refer to the HTML tag selector of the component

## In-depth concepts/ explanations

**Components**

* Has 2 parts (defined inside `.ts` file):
  * [Decorator](https://www.typescriptlang.org/docs/handbook/decorators.html) - provides metadata such as:
    * selector (html tag name)
    * templateURL (html)
    * styleUrls (css)
  * Class
    * Defines properties & logic

**Interacting with Data**

* `{{ }}` template tags used to display the value of the any class properties 
* `[ ]` used to pass values from the application to the class properties
* `( )` used to respond to events/ bind actions to events (ex: `<button (click)="addArticle(x,y)`>)
* `#`   used to tell Angular to bind values/ HTML tags to a local variable (ex: `<input name="link" id="link" #newlink>``data input from <input> will be stored in newlink`)
  * binded variables such as `newlink` become an object of the type of HTML tag where `#` is used; so DOM manipulation can be done on such variables (example `newlink.value`)

**NgModules**

* This is what gets loaded up when application runs
* Contains 4 properties:
  * Declarations - specifies the components that are defined in this module
    * these components must be declared here to be used in the templates
  * imports - describes the dependencies this module has
  * providers - used for dependency injection
  * bootstrap - tells Angular which component to use as top-level compnent when this app runs