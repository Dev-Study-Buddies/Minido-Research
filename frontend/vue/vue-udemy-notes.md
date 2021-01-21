# Vue notes
*[course](https://www.udemy.com/course/build-web-apps-with-vuejs-firebase/learn)*

## Template Syntax

**Interpolation**

```html
<span> Message: {{ msg }}</span>

<span v-once>This will never change: {{ msg }}</span>
```

* the mustache tag gets replaced by the value of the `msg` property from the corresponding component instance
* the `v-once` directive will only update the value in the HTML once (so subsequent changes of `msg` will not be reflected in HTML)
* Text inside the mustache tag is evaluated as a string (or as very limited JS code)
  * to evaulate HTML, use the `v-html` directive
    * Ex: `<p>Using v-html directive: <span v-html="rawHtml"></span></p>`
    * See: [example](https://v3.vuejs.org/guide/template-syntax.html#raw-html)
    * Has risk of XSS attacks
* Can not be used inside HTML attributes
  * use `v-bind` directives to bind component data with HTML instead
  * Ex: `<div v-bind:id="dynamicId"></div>`
  * if the bounded value is null or undefined, then the associated attribute won't be included on the rendered HTML element

**Directives**

* Custom HTML attributes that are prefixed with `v-` and consists of javascript expressions (like invoking methods)
  * these methods should be simple and declarative; if it becomes complex, invoke computed properties instead
* Makes changes to the DOM (HTML elements) when the value/ result of its js expression changes
* Arguments
  * Denoted by a colon after directive name
    - Ex: `<a v-bind:href="url">...</a>`
      - `href` is the argument which tells the `v-bind` directive to bind the element's `href` attribute to the value of the expression `url`
    - Ex: `<a v-on:click="doSomething">...</a>`
      - `click` is the event this `v-on` directive will listen for and execute `doSomething()` method in response
  * Dynamic arguments can be created by wrappring the arg in sq. brackets
    - Ex: `<a v-bind:[attribute]="url>...</a>`
      - `attribute` will be dynamically set by the underlying Vue component instance
* Shorthands
  * `v-bind`:
    ```html
    <!-- full syntax -->
    <a v-bind:href="url"> ... </a>

    <!-- shorthand -->
    <a :href="url"> ... </a>

    <!-- shorthand with dynamic argument -->
    <a :[key]="url"> ... </a>
    ```
  * `v-bind`:
    ```html
    <!-- full syntax -->
    <a v-on:click="doSomething"> ... </a>

    <!-- shorthand -->
    <a @click="doSomething"> ... </a>

    <!-- shorthand with dynamic argument -->
    <a @[event]="doSomething"> ... </a>
    ```

* Ex: `<span v-bind:title="message">Hover mouse over this to see msg</span>`
    - the attribute `title` in `span` is binded with `message` in the vue component (not shown here)



## Class and Style bindings

**HTML Class binding**

* `:class` is short for `v-bind:class`
* Examples
  ```html
  <!-- this div's class will be `active` is `isActive` is true--->
  <div :class="{ active: isActive }"></div>

  <!-- multiple classes ex--->
  <div
  class="static"
  :class="{ active: isActive, 'text-danger': hasError }"></div>
  ```

  ```ts
  // Common pattern to do multiple classes
  // HTML: <div :class="classObject"></div>
  data() {
    return {
      isActive: true,
      error: null
    }
  },
  computed: {
    classObject() {
      return {
        active: this.isActive && !this.error,
        'text-danger': this.error && this.error.type === 'fatal'
      }
    }
  }
  ```

  **HTML Style Bindings**
  * *skipped*
  * See: [docs](https://v3.vuejs.org/guide/class-and-style.html#binding-inline-styles)


## Directives (Rendering)

**Conditional Rendering**

* `v-if` conditionally renders blocks
  * render if true, dont render if false
  * Ex: `<h1 v-if="awesome">Vue is awesome!</h1>`
  * `v-else` and `v-else-if` can be used with `v-if`
* `v-if`: To conditionally render more than one element, use `<template>`
  ```html
  <template v-if="ok">
    <h1>Title</h1>
    <p>Paragraph 1</p>
    <p>Paragraph 2</p>
  </template>
  ```
* `v-show` - conditionally renders HTML elements
  * difference with `v-if`: 
    * only toggles the CSS property of the element
      * so HTML elements remains in the DOM; they just don't appear visually (because of CSS)
    * does not support `<template>`
    * does not work with `v-else`
* use `v-show` for toggling on and off elements very often
  * use `v-if` when conidtions unlikely to change much for toggle

**List Rendering**

* `v-for` used to render list of items based on an array 
  * syntax: `<li v-for="item in items"> {{ item }}</li>`
  * syntax: `<li v-for="(item, index) in items">{{ index }}-{{ item }}</li>`
    * `index` is optional second arg for getting index of items in array
    * can replace `in` with `of`
* `v-for` can be used to iterate through properties of object
  * syntax: `<li v-for="value in myObject">{{ value }}</li>`
  * syntax: `<li v-for="(value, name, index) in myObject">{{index}}.{{name}}:{{value}}</li>`
    * `name` is optional second argument to get the property's name (key)
* `v-for` can be used with Range (number)
  * syntax: `<span v-for="n in 10" :key="n">{{ n }} </span>`
* `v-for` can be used with `<template>` tags to render multiple elements 
* "in-place patch" strategy
  * default way Vue updates items when items change in list (apparently doesn't maintain order)
  * to change this, use `:key` to have vue render items in a way that maintains order on `:key`
  * See: [docs](https://v3.vuejs.org/guide/list.html#maintaining-state)
* Don't use `v-if` & `v-for` in the same element

## Event Handling

* `v-on` directive is used to listen to DOM events (also input events) and execute JS when triggered
  * shorthand: `@`
* Example
  ```html
  <div id="basic-event">
    <button @click="counter += 1">Add 1</button>
    <p>The button above has been clicked {{ counter }} times.</p>
  </div>
  ```
* `$event` can be used to pass the original DOM event that triggered the `v-on` directive, to its method
* to have multiple methods in event handler, delimit with comma
* Event modifiers
  * these are directive postfixes that call the `events` methods 
    * Ex: `<a @click.stop="doThis"></a>`
  * See: [docs](https://v3.vuejs.org/guide/events.html#event-modifiers)

## Form Input Bindings

* `v-model` directive creates two-way data bindings on form input, textarea, and select elements with Vue `data` 
  * updates data on user input events 
  * ignores the `value, checked, selected` attributes on HTML form elements
    * Therefore, declare the intial value in JS code rather than in HTML
* Example
  ```html
  <input v-model="searchText" />
  <!-- Same as: --->
  <input :value="searchText" @input="searchText = $event.target.value" />
  <!-- on component, looks like this: --->
  <custom-input
    :model-value="searchText"
    @update:model-value="searchText = $event"
  ></custom-input>
  ```
* See: [docs](https://v3.vuejs.org/guide/forms.html#form-input-bindings)


## Components

**Components**
  * reusuable HTML element instances with a name

**Application instance & Root component**
```js
const app = Vue.createApp({/*options*/})
```

* the code above creates the root component, root of all components registered to it
  * Similar to `express` `app` object that is the root of all routers, middleware, etc
* `app` has configuration options such as defining `error-handling` and other properties that can be used by components registered with the `app`
* `app.mount(#attribute-name)`
  * returns root component instance
  * used to "mount" the `app` to the DOM element by calling the `#attribute-name` in a HTML element
      - Ex: `<div id="attribute-name"></div>`
* components can be globally registered with `app`
  * globablly registered componsned can be used in the template of any component within the `app`
  * ex:
  ```js
  // Create a Vue application
  const app = Vue.createApp({})

  // Define a new global component called button-counter
  app.component('button-counter', {
    data() {
      return {
        count: 0
      }
    },
    template: `
      <button @click="count++">
        You clicked me {{ count }} times.
      </button>`
  })

  // #compnents-demo is the HTML tag
  app.mount('#components-demo')
  ```

  ```html
  div id="components-demo">
  <button-counter></button-counter>
  </div>
  ```

**Component Instance Properties**
* component options that are used to set properties such as methods, fields, etc to be used in the HTML template
* `data` 
  * this option is a function that returns a `data` object that represents various components properties and fields
  * added to Vue's reactivity system (any changes to $data can be reacted to by Vue)
  * accessed with `$data`
* `props`
  * Used to pass data from a parent component down to its child components
  * unlike `data`, these are custom HTML attributes
* `methods`
  * object that contains methods for the component
    * used to react on events happening in the DOM
    * used by `methods` or `watchers` to invoke in their own methods
  * `this` is refers to the component instance in the methods
    * for this reason, avoid using arrow functions
  * methods are commonly used as event listeners
  * when debouncing or throttling methods, it is better to do this in the `created()` lifecycle hook so component instances throttle/ debounce separately from other component instances
* `computed`
  * object that contains methods that react to data changes for the component
  * similiar to `methods`
    * difference: `computed` methods are cached
      * Because they are expensive and should only be executed when any reactive dependencies (ex:`$data`) changes
      * "if something independent of the computed property changes on the page and the UI is re-rendered, the cached result will be returned, and the computed property will not be re-calculated, sparing us a potentially expensive operation."
  * Use cases
    * when you need to compose new data from existing data sources
    * on variables in HTML that is built from on or more data properties
    * simplify complicated, nest property names 
    * when need to reference a value from HTML
    * when need to listen to changes from multiple data properties
  * they are by default `getters`
  * to use as a setter, see ex:
    ```ts
      computed: {
        fullName: {
        // getter
        get() {
          return this.firstName + ' ' + this.lastName
        },
        // setter
        set(newValue) {
          const names = newValue.split(' ')
          this.firstName = names[0]
          this.lastName = names[names.length - 1]
        }
      }
    }

    // example
    vm.fullName = 'John Doe' // vm.firstName = John, vm.lastName = Doe
    ```
  
* `watch`
  * object that contains methods for performing async or expensive operations in response to changing data
  
  

**LifeCycle**
* lifecycle hooks can be used to add logic in between the various life cycle steps of a component
  * ex: `create(){//...log creation of component}`
* See: [LifeCycle Diagram](https://v3.vuejs.org/images/lifecycle.svg)

**Custom Child Components Events**
* you can use `$emit('eventname')` to emit custom events and listen for those events with `@eventname`
  * you can emit values with the event by including it in as the second parameter in `$emit()`
    * access this value with `$event`
    * ex: `<blog-post ... @enlarge-text="postFontSize += $event"></blog-post>`
    * ex: `<blog-post ... @enlarge-text="onEnlargeText"></blog-post>` (`$event` is automatically passed into method as arg)
* you can use custom events with `v-model` as well
  ```html
  <input v-model="searchText" />
  <!-- Same as: --->
  <input :value="searchText" @input="searchText = $event.target.value" />
  <!-- on component, looks like this: --->
  <custom-input
    :model-value="searchText"
    @update:model-value="searchText = $event"
  ></custom-input>
  ```
* See: [docs](https://v3.vuejs.org/guide/component-basics.html#listening-to-child-components-events)

**HTML element: `<slot)>`**
  * used to make components act like HTML elements
    * allows you to pass data to component directly
  * example:
  ```html
    <alert-box>
    Something bad happened.
  </alert-box>
  <!-- output: Error! Something bad happened. --->
  ```
  const app = Vue.createApp({})

  app.component('alert-box', {
    template: `
      <div class="demo-alert-box">
        <strong>Error!</strong>
        <slot></slot>
      </div>
    `
  })

  app.mount('#slots-demo')
  ```js
  // component code
  ```

**Dynamic Components**

* Lets you dynamically switch between components with `is` attribute
* See: [docs](https://v3.vuejs.org/guide/component-basics.html#dynamic-components)

**DOM HTML Restrictions**
* Elements like `<table>` restricts what elements can appear inside them
  * to get around this, use: `v-is` directive
* See: [docs](https://v3.vuejs.org/guide/component-basics.html#dom-template-parsing-caveats)

