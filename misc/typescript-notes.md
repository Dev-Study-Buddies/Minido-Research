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
enum RoleB {ADMIN = "admin, AUTHOR = "author"};
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

## MISC 

*Or, "idk in which section to put this information..."*

* `!` is used to tell compiler that the developer knows for sure the value is not null, and so compiler shouldn't complain about potential null values





