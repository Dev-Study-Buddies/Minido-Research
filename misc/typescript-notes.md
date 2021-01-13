Notes for "Understanding TypeScript" Udemy [course](https://www.udemy.com/course/understanding-typescript/)

## TypeScript Types Overview

**Object Types**

```ts
// Various ways to declare object types
// {} == object true
const personA: {} = {
  name: "Max",
  age: 30
};

const personB: object = {
  name: "Max",
  age: 30
};

const personC: {
  name: string;
  age: number;
} = {
  name: Max,
  age: 30
}

// nest object types
const personD: {
  name: string;
  age: number;
  address: {
    street: string;
    city: string;
  }
} = {
  name: "Max",
  age: 30,
  address: {
    street: "42 wallaby way",
    city: "Sydney"
  }
}

console.log(personA.nickname); // Error; property "nickname" does not exist on type 'object'
```

**Tuples**

```ts
// Tuples allow you to express an array with a fixed number of elements whose with various types
const person: {
  name: string;
  age: number;
  hobbies: string[]; //array
  role: [number, string]; //tuple; exactly 2 elements of number, string type, or else error
}
```

**Enum**
```ts
enum RoleA {ADMIN, AUTHOR};
RoleA.ADMIN // output -> 0
enum RoleB {ADMIN = "admin", AUTHOR = "author"};
enum RoleC {ADMIN = 100, AUTHOR = "AUTHOR"}; 
```

**Union**
```ts
const input: number | string; 
input = 1; //valid
input = "apple"; //valid
input = true; //invalid
```

**Literal Types & Type operator**
```ts
// type is used to create your custom types
type ValidInputs = number | string;
type User = {name: string; age: number;};

// Literal types are like constants
// Cool is a type alias for "cool" | "not cool"
type Cool = "cool" | "not cool";

function personIsCool(name: string, cool: Cool){
  if(cool === "cool") console.log(name, " is cool");
  if(cool === "not cool") console.log(name, " is not cool");
}

personIsCool("bob", "cool"); //bob is cool
personIsCool("bobA", "idk"); //Error: Argument of type "idk" is not assignable to parameter of type "Cool"
```

**Function Type**
```ts
// You can specify the "shape" of a function as a type
// You can just use `const test: Function;` to declare something as function, but then that means that `test` could be ANY function (so Function is kinda like "any"; its too broad)

function add(n1: number, n2: number){
  return n1+n2;
}

function printA():void {
  console.log("a");
}

let sum: (a: number, b: number) => number;

sum = add; // valid
sum = printA; // invalid

// callbacks
function addAndHandle(n1: number, n2: number, cb: (num: number) => void {
  const result = n1 + n2;
  cb(result);
}

addAndHandle(10, 20, (result) => {console.log(result);}); // output 30
```

**Unknown Types**
```ts
// Unknown is more restrictive type than any (it has some typechecking whereas "any" has none)
// Used when you don't know the type of a value, but know what you want to do with it if it is a certain value
//     ex: good for dynamic content where you may be provided values of multiple, "unknown" types 
// Use Case: can be used to narrow down "unknown" variables to something more specific using typeof checks

let input: unknown;
let name: string;

input = 5; //valid
input = "Max"; //valid
name = input; //invalid...can't assign variable of type "unknown" to "string"

// type checking unknown
if( typeof input === 'string'){
  name = input;
}
```

**never type**
```ts
// never is just a more descriptive form of void
// tells developers that the function is intended to never return a value
// USE CASE: util functions where a function is used to throw exceptions

// return type is never instead of void (if we didn't specify never, return type would be void)
function generateError(message: string, code: number): never {
  throw {message: message, errCode: code};
}
```

**Intersection types**

* Creates an "intersection" of types where it combines multiple types (as opposed to union that lets you use either type)
```ts
// for object types, its alternative to interface and inheritance
// type; note how name property is shared between the two....
type Admin = {
  name: string;
  privileges: string[];
}

type Employee = {
  name: string;
  startDate: Date;
}

type ElevatedEmployee = Admin & Employee;

const employee1: ElevatedEmployee = {
  name: 'Max',
  privileges: ['create-server'],
  startDate: new Date(),
}

// interface
interface Admin {
  name: string;
  privileges: string[];
}

interface Employee {
  name: string;
  startDate: Date;
}

interface ElevatedEmployee extends Admin, Employee {}

const employee2: ElevatedEmployee = {
  name: 'Max',
  privileges: ['create-server'],
  startDate: new Date(),
}

// Intersection of "types" (as opposed to objects)
type Combinable = string | number;
type Numeric = number | boolean;
type Universal Combinable & Numeric;
```

* Type guards
```ts
// type guard with Javascript types
type Combinable = string | number;

function add(a: Combinable, b: Combinable) {

    // This is called a type guard; gives you flexibility to work with various types and handle them correctly 
    if(typeof a === 'string' || typeof b === 'string'){
      return a.toString() + b.toString();
    }

    return a + b;
}

// type guard with custom types

type Admin = {
  name: string;
  privileges: string[];
}

type Employee = {
  name: string;
  startDate: Date;
}

type UnknownEmployee = Employee | Admin;

function printEmployeeInformation(emp: UnknownEmployee) {
  // Error since typeguard because typeof only works with Javascript types
  if(typeof emp === 'Admin'){
  // error because compiler not sure if its an Emmployee or admin
    console.log(emp.privileges); 
  }

  if(emp.privileges)... // this doesn't work as a type guard either 

  // valid type guard for checking if a property exists in an object 
  // and therefore find out if its the correct type for the operation
  if('privileges' in emp){
    console.log(emp.privileges); // works
  }
  
}

// type guard for classes: use the `instanceof` operator

// discriminated union type is a pattern for using type guards for union types 

// where you create a literal member (or use an exisiting one) in a type & interface declaration

//  then use if/else, switch statements to check against that "literal member" property to see what type it is
interface Square {
    kind: "square";
    size: number;
}

interface Rectangle {
    kind: "rectangle";
    width: number;
    height: number;
}
type Shape = Square | Rectangle;

function area(s: Shape) {
    switch (s.kind) {
        case "square": return s.size * s.size;
        case "rectangle": return s.width * s.height;
        case "circle": return Math.PI * s.radius * s.radius;
        default: const _exhaustiveCheck: never = s;
    }
}
```

**type casting**
```ts
const clueless: unknown = "1";

// type cast clueless to number
const clueNum: number = <number>clueless;
// another format
const clueNumPreferred = clueless as number;
```

**Index types**

*dynamic property names*
*duck typing*

* You can define indexable types where you can iterate through them like arrays
* Indexable types have an *index signature* that describes the types that we can use...as an index (key) and their return value type
* index signatures can be either string or number
* Indexable types are very handy for defining the return values of the properties of dynamic objects.
* Good for when you don't know which property names you want or will be using in an object
* More information: [article](https://levelup.gitconnected.com/introduction-to-typescript-interfaces-indexable-types-d66958523518), [article2](https://www.logicbig.com/tutorials/misc/typescript/indexable-types.html)

```ts
interface NameArray {
    [index: number]: string;
}
let nameArray: NameArray = ["John", "Jane"];
const john = nameArray[0];
console.log(john);

// another example
class Animal {
  name: string = '';
}
class Cat extends Animal {
  breed: string = '';
}
interface Zoo {
    [x: number]: Cat;
    [x: string]: Animal;
}

// another example
interface CustomErrors{
  [property: string]: string;
}

const myErrors: CustomErrors{
  email: 'Not a valid email',
  username: 'Not a valid username',
  // etc....you can add as many types here as you want in the form of string: string
}
```

**Function overloads**
```ts

type Combinable = string | number;
// Overloading function (or calling the same function with different parameters)
function add(a: number, b:number): number;
function add(a: number, b:string): string;
function add(a: string, b:number): string;
function add(a: string, b:string): string;
function add(a: Combinable, b: Combinable) {
  if(typeof a === 'string' || typeof b === 'string'){
    return a.toString() + b.toString();
  }
  return a + b;
}
```

**Optional Chaining**
*good for accessing properties that may not exist on HTTP request objects*
```ts
// when foo is defined, foo.bar.baz() will be computed; 
//but when foo is null or undefined, 
//stop what we’re doing and just return undefined.
let x = foo?.bar.baz();

// javascript equivalanet
let x = foo === null || foo === undefined ? undefined : foo.bar.baz();
```

**Nullish coalescing**
*handles potentially null data*
```ts
const userInput = '';
// if input is false (?), 0, empty, null or undefined, then store default value
const storedData = userInput || 'default value'; // default value

// Nullish operator ?? is for dealing specifically with ONLY null/ undefined values, not falsy values
const storedData2 = userInput ?? 'DEFAULT'; // 
```
## TypeScript Configuration & Compiler


* INSTALLATION: `npm install -g typescript

* COMPILATION & EXECUTION: 
  * invoke `tsc` command on typescript files to compile them to javascript
  * *NOTE: Will still let you compile *with* errors by default (to disable, set `noEmitOnError` to true)*
  * Run the compiled js files with node; can't run ts files directly

* WATCH:
  * `tsc <filename> -w` (or `-watch`)
  * Watches files for changes and recompiles when file is saved with changes

* CREATING PROJECT: 
  * `tsc --init` this will add a `tsconfig.json` to the directory the command was invoked in
  * allows you to just run `tsc` to compile all files where `tsconfig.json` resides
  * OR You can specify which files to include/ exclude for compilation in `tsconfig.json`
  * see more about `tsconfig.json` in the [docs](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

* COMPILER OPTIONS (WIP)
  * `target` - specify ECMAScript version of the compile files
  * `lib` - specify global libraries typescript should know about; by default, `DOM, ES6, DOM.ITERABLE, SCRIPTHOST` are the default libraries included (which means you can call upon DOM functions without error or importing manually); customizing `lib` will override the defaults and require you to specify all libraries to be included
  * `rootDir` - specifies which folder to look for ts files; prevents folders outside of rootDir from being included in the `outDir`
  * `outDir` - specify where to place the compiled js files
  * `removeComments` - removes comments in TS files from the compiled JS files
  * `noEmit` - allows you to "compile" your ts files without actually generating js files; good if you just want to check for errors and not compile
  * `noEmitOnError` - specifies whether JS files should be generated if there is compilation error; default is to generate files even if error (false)
  * `strict` - set to true to enable various strict compilation checks
  * additional checks options - enable these for code quality purposes

## Some OOP notes

* to prevent weird issues with `this` in classes, here is one solution
```ts
class Department {
    name: string;

    constructor(){...}

    // This is a TypeScript thing
    // Passing in `this` does not mean this method takes a `this` parameter
    // It just means that `this` should refer to an instance of the Department class (this class), and nothing else
    describe(this: Department){...}
}

const acc = new Department("xxx");
const acc2 = {describe: acc.describe};

acc2.descibe(); //invalid because `this` refers to instance of acc2 and not instance of Department
```

* You can avoid needing to declare class properties in a class, by declaring them in the constructor parameters
```ts
class ITDepartment extends Department {
    constructor (id: string, public admins: string[]){
      //...
    }
}

// VERSUS

admins: string[];

class ITDepartment extends Department {
    constructor (id: string, admins: string[]){
      //...
    }
}
```

* In order to use `this` in a subclass, `super()` must first be explicitly invoked

* `get` and `set` are used to create getters and setters
```ts
class Employee {
  private _fullName: string = "";

  get fullName(): string {
    return this._fullName;
  }

  set fullName(newName: string) {
    if (newName && newName.length > fullNameMaxLength) {
      throw new Error("fullName has a max length of " + fullNameMaxLength);
    }

    this._fullName = newName;
  }
}
```

* `readonly` is used to set a value once, and never again

* interfaces can also be used for defining the shape of functions (not just class objects)

```ts
// with type
type AddNums = (a: number, b: number) => number;

let sum: AddNums;

add = (n1: number, n2: number) => {return n1+n2;};

// with interface
interface AddNums {
  (a: number, b: number) => number;
}

let sum: AddNums;

add = (n1: number, n2: number) => {return n1+n2;};

// Atm, I don't think there is much difference between the two
```

* Optional parameters & properties
```ts
interface Person {
  readonly name: string;
  job?: string; //optional
}
```

* interfaces are not compiled to js
  * They don't show up in the compiled JS files

## Generics
```ts
// the type in the brackets <> mean that whatever its attached to, that thing is of THAT type

// Array of strings
const names: Array<string>;
// Arrayof strings and numbers
const names2: Array<string | number>;
// Promise that eventually returns a string
const promise: Promise<string>;

// An example of why we would want to use generics: type safety
const promiseEx: Promise<number> = new Promise((resolve, reject) => {
  setTimeout( () => {resolve(10);}, 2000);
  })
});

promise.then( data => data.split('')); // Error! Can't call split on number

//////////////////////
// Ex of generic function; note, T is just a variable for any type
function getArray<T>(items : T[] ) : T[] {
    return new Array<T>().concat(items);
}

let myNumArr = getArray<number>([100, 200, 300]);
let myStrArr = getArray<string>(["Hello", "World"]);

myNumArr.push(400); // OK
myStrArr.push("Hello TypeScript"); // OK

//////////////////////
// generic constraints (< T extends X>)
// lets you call X methods & properties on the parameter
// But does not restrict you to it
// TODO: add better example of this ^
function getArray<T extends Person>(items : T[] ) : T[] {
    return new Array<T>().concat(items);
}

//////////////////////
// generic "keyof" constraint
// lets you specify that a parameter will be a property of an object
// Since we don't know beforehand what the property name will be, we use generics and keyof to ensure it will be a valid property
function extractAndConvert<T extends object, U extends keyof T>(obj: T, key: U){
  return 'Value: ', obj[key];
}

extractAndConvert({name: 'bob'}, 'name');

//////////////////////
// generic class
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
```

**Partial Type**
*wraps an object of the type T in Partial<T>, and makes all the properties of the object of type T optional*
* Use Case: lets you work with objects that aren't of type T (yet)

```ts

interface CourseGoal {
  title: string;
  description: string;
  completeUntil: Date;
}

// Valid
function createCourseGoal(t: string, desc: string, d: Date): CourseGoal { 
  return {title: t, description: desc, completeUntil: d};
}

// Invalid because {} is not a CourseGoal object
function createCourseGoal(t: string, desc: string, d: Date): CourseGoal { 
  let c = {};
  // let c2 : CourseGoal = {}; ...not sure if this is invalid too...I will assume it is
  c.title = t;               // error
  c.description = desc;      // error
  c.completeUntil = d;       // error
  
  return c;
}

// Valid;
// Partial type tells TS that at then
function createCourseGoal(t: string, desc: string, d: Date): CourseGoal { 
  let c : Partial<CourseGoal>= {};
  c.title = t;      
  c.description = desc;
  c.completeUntil = d;
  
  return c as CourseGoal;
}
```

**Readonly Generic Type**
*prevents modification of object after initialization
```ts
const names: Readonly<string[]> = ['Bob', 'Anna'];
names.push('Tim'); // error
names.pop(); // error
```
**Generics vs Union**
* Generics = allows multiple types, but locks in the type
* Unions = allows multiple types, and lets you use any of the allowed types
```ts
<T extends string | number | boolean> // if T is string, then T is string uniformly throughout the program; it can't be anything else

t: string | number | boolean // t can be string, number, or boolean. Its mixed. You can insert string, number, and boolean values
```

## Decorators
*meta programming*

* To enable decorators, make sure target is set to ES6 and experimentalDecorators is set to true
* Class decorators runs when Javascript loads your class file, not when class object is constructed
* You can add multiple decorators to a class; execution order is bottom up (closest to the class, and then goes up from there)
* Property decorators require 2 parameters

```ts
//class decorator
// when function is attached to a class as decorator, class constructor is the first parameter
// you can do Logger(_: Function) to tell typescript to ignore passing in the consstructor
function Logger(constructor: Function) {
  console.log('Logging...');
  console.log(constructor);
}

@Logger
class Person{
  name = 'Bob';
  constructor(){
    console.log('Creating person object...);
  }
}

// when this is executed, `Logggin...` + constructor code, 
//will be displayed first, before `creating person object` message

//////////////////////
// Decorator factory
// Lets you customize decorator functions by passing paramters into the Decorator

// this function returns a new function
function Logger(logString: string) {
  return function(constructor: Function) {
    console.log(logString);
    console.log(constructor);
  }
}

// now it requires a (), because it needs to execute the function that is being returned by Logger
// Note, we can pass in values into the decorator now
@Logger('LOGGING-PERSON')
class Person{
  name = 'Bob';
  constructor(){
    console.log('Creating person object...');
  }
}

// Another example; this example will create HTML content with the class, similiar to components in angular

function WithTemplate(template: string, hookId: string){
  return function(constructor: any){
    const hookE1 = document.getElementById(hookId);
    const p = new constructor();
    if(hookE1){
      hookE1.innerHTML = template;
      hookE1.querySelector('h1')!.textContent = p.name;
    }
  }
}

@WithTemplate('<h1>MY Person Object</h1>', 'app')
class Person{
  name = 'Bob';
  constructor(){
    console.log('Creating person object...');
  }
}

//////////////////////
// Property decorator

function Log(target: any, propetyName: string | Symbol){
  console.log('Property decorator');
  console.log(target, propertyName);
}
class Product {
  @Log
  title: string;
  private _price: number;

  set price(val: number){
    if(val > 0)
      this._price = val;
    else 
      throw new Error('Invalid price, should be postive);
  }

  constructor(t: string, p: number){
    this.title = t;
    this._price = p;
  }
}
```

## MISC 

*Or, "idk in which section to put this information..."*

* `!` is used to tell compiler that the developer knows for sure the value is not null, and so compiler shouldn't complain about potential null values







