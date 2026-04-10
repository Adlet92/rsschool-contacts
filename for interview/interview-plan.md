**Data Types**

1. Primitives vs Objects

Javascript has 2 data types: Primitives and Objects.

Primitives are immutable and stored directly in the memory. Primitives has 7 types: string, number, Boolean, null, undefined, symbol, biginit.
They can only store a single data, have no methods and are immutable.


```
// We cannot mutate the string
let car = "car"
console.log(car) // car
car.toUpperCase()
console.log(car) // car
car[0] = "b"
console.log(car) // car

// But we can assign a new value to the same variable
car = car.toUpperCase()
console.log(car) // CAR
```

They are stored as values and not as references.

**Object types** stored by references. Objects include: objects, arrays, functions, dates, sets, maps. Stored by the reference. Two variables can point to the same object.
```
Const obj1 = {name: “Alex”}
Const obj2 = obj1
obj2.name = “Bob”
Console.log(obj1.name) // Bob
```
Interview Q: Why does modifying obj2 affect obj1?
A: Because both reference same object.

2. Type checking (typeof, instanceof)

typeof returns a string describing the type of a value
```
typeof 42            // "number"
typeof "hello"       // "string"
typeof true          // "boolean"
typeof undefined     // "undefined"
typeof Symbol()      // "symbol"
typeof 10n           // "bigint"
```

```
typeof null // "object"
```
**Historical bug in JS**
- null is NOT an object
- but JS still reports "object"

```
typeof [1,2,3] // "object"
```
Not useful for arrays! Correct way to use
```
Array.isArray([1,2,3]) // true
```

```
typeof function(){} // "function"
```

Functions are technically objects, but JS gives special "function"

**instanceof**

Checks whether an object is an instance of a constructor

```
[] instanceof Array      // true
{} instanceof Object     // true
new Date() instanceof Date // true
```

How it works
```
function Person() {}

const p = new Person();

p instanceof Person // true
```

**Practical Examples (INTERVIEW STYLE)**

Example 1

```
typeof []           // "object"
[] instanceof Array // true
```

Example 2
```
typeof null           // "object"
null instanceof Object // false
```

Example 3

```
function A(){}
const a = new A();

typeof a        // "object"
a instanceof A  // true
```

When to use what
Use typeof when: checking primitives
```
typeof value === "string"
```
Use instanceof when: checking class / constructor

```
user instanceof User
```

Q3: Difference between typeof and instanceof?

typeof → primitives

instanceof → prototype chain


3. Type conversions, == vs === (practical tasks)

If interviewer asks: “When would you use ==?”

Strong answer:

Almost never. Only in very specific cases like value == null to check both null and undefined.

```
5 == "5" // true
```
converts types before comparing

HOW == ACTUALLY WORKS (INTERVIEW GOLD)
Basic rules:
1. If types differ → convert
2. If one is boolean → convert to number
3. If string + number → convert string to number
4. If object → convert to primitive


**Variables**

1. var vs let vs const

var declarations are globally scoped or function/locally scoped.
The scope is global when a var variable is declared outside a function. This means that any variable that is declared with var outside a function block is available for use in the whole window.

var is function scoped when it is declared within a function. This means that it is available and can be accessed only within that function.

```
var greeter = "hey hi";

    function newFunction() {
        var hello = "hello";
    }
```

Here, greeter is globally scoped because it exists outside a function while hello is function scoped. So we cannot access the variable hello outside of a function. So if we do this:
```
var tester = "hey hi";

    function newFunction() {
        var hello = "hello";
    }
    console.log(hello); // error: hello is not defined
```

var variables can be re-declared and updated
This means that we can do this within the same scope and won't get an error.

```
var greeter = "hey hi";
    var greeter = "say Hello instead";
```
or

```
var greeter = "hey hi";
    greeter = "say Hello instead";
```

Hoisting is a JavaScript mechanism where variables and function declarations are moved to the top of their scope before code execution. This means that if we do this:
```
console.log (greeter);
    var greeter = "say hello"
```
it is interpreted as this:
```
var greeter;
    console.log(greeter); // greeter is undefined
    greeter = "say hello"
```
So var variables are hoisted to the top of their scope and initialized with a value of undefined.

Problem with var
There's a weakness that comes with var. I'll use the example below to explain:
```
var greeter = "hey hi";
    var times = 4;

    if (times > 3) {
        var greeter = "say Hello instead";
    }

    console.log(greeter) // "say Hello instead"
```
So, since times > 3 returns true, greeter is redefined to "say Hello instead". While this is not a problem if you knowingly want greeter to be redefined, it becomes a problem when you do not realize that a variable greeter has already been defined before.

If you have used greeter in other parts of your code, you might be surprised at the output you might get. This will likely cause a lot of bugs in your code. This is why let and const are necessary.


**let** is block scoped
A block is a chunk of code bounded by {}. A block lives in curly braces. Anything within curly braces is a block.

So a variable declared in a block with let is only available for use within that block. Let me explain this with an example:
```
let greeting = "say Hi";
   let times = 4;

   if (times > 3) {
        let hello = "say Hello instead";
        console.log(hello);// "say Hello instead"
    }
   console.log(hello) // hello is not defined
```
**let can be updated but not re-declared.**

Just like var, a variable declared with let can be updated within its scope. Unlike var, a let variable cannot be re-declared within its scope. So while this will work:
```
let greeting = "say Hi";
    greeting = "say Hello instead";
```
this will return an error:
```
    let greeting = "say Hi";
    let greeting = "say Hello instead"; // error: Identifier 'greeting' has already been declared
```

However, if the same variable is defined in different scopes, there will be no error:
```
let greeting = "say Hi";
    if (true) {
        let greeting = "say Hello instead";
        console.log(greeting); // "say Hello instead"
    }
    console.log(greeting); // "say Hi"
```
Why is there no error? This is because both instances are treated as different variables since they have different scopes.
This fact makes let a better choice than var. Since a variable cannot be declared more than once within a scope, then the problem discussed earlier that occurs with var does not happen.

**Hoisting of let**

Just like var, let declarations are hoisted to the top. Unlike var which is initialized as undefined, the let keyword is not initialized. So if you try to use a let variable before declaration, you'll get a Reference Error.

**Const**

Variables declared with the **const** maintain constant values. const declarations share some similarities with let declarations.

**const declarations are block scoped**

Like let declarations, const declarations can only be accessed within the block they were declared.

**const cannot be updated or re-declared**

This behavior is somehow different when it comes to objects declared with const. While a const object cannot be updated, the properties of this objects can be updated. Therefore, if we declare a const object as this:
```
const greeting = {
        message: "say Hi",
        times: 4
    }
```
we can do this:
```
greeting.message = "say Hello instead";
```
**Hoisting of const**

Just like let, const declarations are hoisted to the top but are not initialized.


**Loops**

Loops offer a quick and easy way to do something repeatedly.

```
for (let step = 0; step < 5; step++) {
  // Runs 5 times, with values of step 0 through 4.
  console.log("Walking east one step");
}
```
There are many different kinds of loops, but they all essentially do the same thing: they repeat an action some number of times.

The various loop mechanisms offer different ways to determine the start and end points of the loop. There are various situations that are more easily served by one type of loop over the others.

**for statement**

A for loop repeats until a specified condition evaluates to false.
```
for (initialization; condition; afterthought)
  statement
```
**do...while statement**

The do...while statement repeats until a specified condition evaluates to false.
```
do
  statement
while (condition);
```

