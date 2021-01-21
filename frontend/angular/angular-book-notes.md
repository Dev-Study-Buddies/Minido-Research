# Angular Learning
 From the book *Writing Your First Angular Web application* book

## Project Setup

 **Requirements**
 * Node
 * NPM
 * Typescript
 * Angular CLI

**Angular CLI Commands**

```bash
# creates new project from scratch
ng new <project-name>

# runs application 
ng serve

# generates component with all relevent files
# places it as a directory into the `/app` directory
ng generate component <component-name>
```

**Modules**

Input 
  - Allows you to pass in a vlaue from the parent template

HostBinding
  - Syntax; `@HostBinding()`
  - used to set properties on the host element


**Files Descriptions**

`index.html`
  - this is where the application is rendered with the `<app-root>` tag

`app/app.component.html` 
  - defines the HTML/ components that will injected into the webpage
    - through `<app-root>` in `index.js`

`angular.json` 
  - angular configurations

`main.ts` 
  - entry point for the angular app; bootstraps the application
  * boots an angular module `AppModule`

`AppModule` 
  - specifies which component to use as the top level component; also declares the components 


## Concepts

**Components**
* HTML tags that have custom functionality attached to them
* Defined inside `.ts` file 
  * ex: `app.component.ts`
* Consists of two parts:
  * [Decorator](https://www.typescriptlang.org/docs/handbook/decorators.html) - provides metadata to HTML template:
    * selector (html tag name)
    * templateURL (html)
    * styleUrls (css)
  * Class
    * Defines properties & fields
    * Uses typescript classes to carry out business logic
    * Defines getters and setters


**Change Detection**
  - Where angular synchronizes UI with component data to synchronize any changes between the two
  - Events that can trigger "change detection":
    - When component data is updated
    - When User interacts with UI

**Data binding syntax**

* Interpolation `{{ value }}`:  
  - Used to read values from expression context to substitute string values dynamically into your HTML templates
    - Expression Context is assigned in this order: 
      - template variables in the HTML
      - names in the directive's context (?)
      - the component (class) associated with the HTML template
  - Ex: `<div title="Hello {{name}}>`
    - Binds the class property `name` with the interpolated string `"Hello <name>"`
  - Ex: 
    ```
    app.component.ts: 
    ---> customer: string ='Bob';
    
    app.component.html:
    ---> <h3>Current Customer: {{ customer }}</h3>
    
    output: 
    ---> Current Customer: Bob
    ```
* Template Expressions `{{ expression }}`:
  * Angular evaulates the expression inside `{{ }}`
    * Converts the results from expression to strings
    * Assigns the value to a binding target:
      - HTML element (ex: `<p>1 + 1 = {{1 + 1}}.</p> // --> "1 + 1 = 2"`)
      - Component
      - Directive
  * Functions and other js code are legal inside template expressions
  * Angular executes template expressions every change detection cycle
    * To keep things smooth, keep expressions simple or cache values from expressions

    
  
* `[ ]` 
  - passes values from the application to the class properties
* `( )` used to respond to events/ bind actions to events (ex: `<button (click)="addArticle(x,y)`>)
* `#`   used to tell Angular to bind values/ HTML tags to a local variable (ex: `<input name="link" id="link" #newlink>``data input from <input> will be stored in newlink`)
  * binded variables such as `newlink` become an object of the type of HTML tag where `#` is used; so DOM manipulation can be done on such variables (example `newlink.value`)

**NgModules**

* Properties:
  * `Declarations` 
    - specifies the components that are defined in this module
    - these components must be declared here to be used in the templates
  * `imports` 
    - describes the dependencies this module has
  * `providers`
    - used for dependency injection
  * `bootstrap` 
    - tells Angular which component to use as top-level compnent when this app runs