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

statement is always executed once before the condition is checked. If condition is true, the statement executes again. At the end of every execution, the condition is checked. When the condition is false, execution stops.
```
let i = 0;
do {
  i += 1;
  console.log(i);
} while (i < 5);
```

**while statement**

A while statement executes its statements as long as a specified condition evaluates to true.

```
while (condition)
  statement
```
```
let n = 0;
let x = 0;
while (n < 3) {
  n++;
  x += n;
}
```

**break statement**

Use the break statement to terminate a loop.
```
for (let i = 0; i < a.length; i++) {
  if (a[i] === theValue) {
    break;
  }
}
```
**continue statement**

The continue statement can be used to restart a while, do-while, for, or label statement.
```
let i = 0;
let n = 0;
while (i < 5) {
  i++;
  if (i === 3) {
    continue;
  }
  n += i;
  console.log(n);
}
// Logs:
// 1 3 7 12
```
If you comment out the continue;, the loop would run till the end and you would see 1,3,6,10,15.

**for...in**

The for...in loop iterates over the enumerable properties of an object.
The for...in loop is primarily used for objects to access their property names (keys).

```
const person = {fname:"John", lname:"Doe", age:25};

let text = "";
for (let x in person) {
  text += person[x];
}
```
The JavaScript for in statement can also loop over the properties of an Array:
const numbers = [45, 4, 9, 16, 25];
```
let txt = "";
for (let x in numbers) {
  txt += numbers[x];
}
```

**for...of**

The following example shows the difference between a for...of loop and a for...in loop. While for...in iterates over property names, for...of iterates over property values:
```
const arr = [3, 5, 7];
arr.foo = "hello";

for (const i in arr) {
  console.log(i);
}
// "0" "1" "2" "foo"

for (const i of arr) {
  console.log(i);
}
// Logs: 3 5 7
```

**Arrays**

It stores multiple values and elements in one variable.
These values can be of any data type — meaning you can store a string, number, boolean, and other data types in one variable.

**How to Declare an Array**
```
let myArray = [];
```
You can also use the array constructor to create or declare an array.
```
let myArray = new Array();
console.log(myArray); // []

let myArray = new Array("John Doe", 24, true);
```
When you pass a single digit into an array constructor, it will fill the array with the number of empty values you entered.
```
let myArray = new Array(4);
console.log(myArray); // [,,,]
```
Mutating methods:
Push, pop, shift, unshift, sort, splice, reverse.

**push()** Method

```
let nobleGases = ['He', 'Ne', 'Ar', 'Kr', 'Xn'];
nobleGases[5] = 'Rn';
console.log(nobleGases); // ['He', 'Ne', 'Ar', 'Kr', 'Xn', 'Rn']
```
```
let nobleGases = ['He', 'Ne', 'Ar', 'Kr', 'Xn'];
nobleGases[5] = 'Rn';
console.log(nobleGases); // ['He', 'Ne', 'Ar', 'Kr', 'Xn', 'Rn']
```
We can append multiple elements with push(), indicating their values separated by a comma:
```
let halogens = ['F', 'Cl'];
console.log(halogens); // ['F', 'Cl']

halogens.push('Br', 'I', 'At'); // 5
// push() returns the length of the modified array
console.log(halogens); // ['F', 'Cl', 'Br', 'I', 'At']
```
**unshift()** Method

Similar to push(), the unshift() method adds one or more elements to the beginning of an array and returns the length of the modified array

**pop()** Method

If you need to remove the last element of an array, you can use the pop() method.

**shift()** Method

Similarly, the shift() method removes the first element from an array and returns it.

**splice()** Method

If you need to remove one or more elements from a specific position of an array, you can use the splice() method.

The first parameter of splice() is the starting index, while the second is the number of items to remove from the array.
```
let nobleGases = ['He', 'Ne', 'Ar', 'Kr', 'Xn', 'Rn'];
console.log(nobleGases); // ['He', 'Ne', 'Ar', 'Kr', 'Xn', 'Rn']

nobleGases.splice(1, 3); // ['Ne', 'Ar', 'Kr']
// splice() returns an array with removed elements
console.log(nobleGases); // ['He', 'Xn', 'Rn']
```
Using splice() you can add elements, too.
```
let nobleGases = ['He', 'Ne', 'Cl', 'Rn'];
console.log(nobleGases); // ['He', 'Ne', 'Cl', 'Rn']

nobleGases.splice(2, 1, 'Ar', 'Kr', 'Xn'); // ['Cl']
// splice() returns an array with removed elements
console.log(nobleGases); // ['He', 'Ne', 'Ar', 'Kr', 'Xn', 'Rn']
```
If you don't need to remove any elements from the array, you can simply use zero as the second argument. The elements will be added starting at the specified index, without removing any item:
```
let nobleGases = ['He', 'Ne', 'Rn'];
console.log(nobleGases); // ['He', 'Ne', 'Rn']

nobleGases.splice(2, 0, 'Ar', 'Kr', 'Xn'); // []
// splice() returns an array with removed elements
console.log(nobleGases); // ['He', 'Ne', 'Ar', 'Kr', 'Xn', 'Rn']
```

Non-mutating methods(return a new array): Map, filter, slice, toSorted, concat.

**concat()** Method

If you need to combine two or more arrays – that is create a single array containing each element of the arrays you want to merge – you can use the concat() method. This method does not change the original arrays and returns a new array.

```
let alkali = ['Li', 'Na', 'K'];
let moreAlkali = ['Rb', 'Cs', 'Fr'];
let alkEarth = ['Be', 'Mg', 'Ca'];

alkali.concat(moreAlkali);
// ['Li', 'Na', 'K', 'Rb', 'Cs', 'Fr']

alkali.concat(moreAlkali, alkEarth);
// ['Li', 'Na', 'K', 'Rb', 'Cs', 'Fr', 'Be', 'Mg', 'Ca']
```

**push()** Method & the Spread Operator

```
let alkali = ['Li', 'Na', 'K'];
let moreAlkali = ['Rb', 'Cs', 'Fr'];
let alkEarth = ['Be', 'Mg', 'Ca'];

alkali.push(...moreAlkali); // 6
console.log(alkali); // ['Li', 'Na', 'K', 'Rb', 'Cs', 'Fr']
```
You cannot use push() without the spread syntax in its arguments, unless you want to nest the whole array moreAlkali as the last element of alkali. In that case, the result would be ['Li', 'Na', 'K', ['Rb', 'Cs', 'Fr']] – an array composed of 4 elements with the last being an array.

**slice()** Method

The slice() method allows you to copy an entire array – or just a portion of it – without mutating it.
```
array.slice(start, end)
```

```
let dough = ['flour', 'water', 'yeast', 'salt'];

let doughCopy = dough.slice();
console.log(doughCopy); // ['flour', 'water', 'yeast', 'salt']
```

**map()** Method

The map() method generates a new array containing the result of calling a callback function on every element of an array.

```
// Syntax
array.map((element, index, array) => {})
```
Loop through items. Return a new transformed array.
```
Example: const doubled = arr.map(x => x*2);
```

**includes()** Method

If you need to know whether a value is included in an array, you can call the includes() method on it.

This method returns true if the value is found. Otherwise, false.
```
let dMinor = ['D', 'E', 'F', 'G', 'A', 'B♭', 'C'];

dMinor.includes('E'); // true
dMinor.includes('E', 2); // false
```

**indexOf()** Method

If you need to know the index at which a specific value can be found in an array, you should use the indexOf() method.

```
// Syntax
array.indexOf(value, startingIndex)
```
It returns only the first index at which the specified value is found, otherwise, it returns -1. The second parameter is the index for where to start searching for the value – the default is zero.
```
let dMinor = ['D', 'E', 'F', 'G', 'A', 'B♭', 'C'];

dMinor.indexOf('E'); // 1
dMinor.indexOf('E', 2); // -1
```

**find()** Methods

find() enable you to search for the first that satisfies a certain condition in an array, respectively.

```
let animals = [
    {no: 1, track: 'Pigs on the Wing (Part One)'},
    {no: 2, track: 'Dogs'},
    {no: 3, track: 'Pigs (Three Different Ones)'},
    {no: 4, track: 'Sheep'},
    {no: 5, track: 'Pigs on the Wing (Part Two)'}
];

animals.find(el => el['track'].includes('Pigs'));
// {no: 1, track: 'Pigs on the Wing (Part One)'}

animals.find(el => el['track'].includes('Horses'));
// undefined
```

**filter()** Method

This method provides you a way to filter the array elements that satisfy a certain criterion.

```
let animals = [
    {no: 1, track: 'Pigs on the Wing (Part One)'},
    {no: 2, track: 'Dogs'},
    {no: 3, track: 'Pigs (Three Different Ones)'},
    {no: 4, track: 'Sheep'},
    {no: 5, track: 'Pigs on the Wing (Part Two)'}
];

animals.filter(el => el['track'].includes('Pigs'));
// [
// {no: 1, track: 'Pigs on the Wing (Part One)'},
// {no: 3, track: 'Pigs (Three Different Ones)'},
// {no: 5, track: 'Pigs on the Wing (Part Two)'}
// ]
```

**sort()** Method

**forEach()** Method

The forEach() method is similar to map(). It executes a function on every array element, but it has no return value. For this reason, a forEach() call can be used only at the end of a chain.

```
Const numbers = [1, 2, 3, 4]
Const sum = numbers.reduce((acc, cur) => acc + cur, 0)
Console.log(sum) // 10
```

**Array vs Set**

Sets are used to store collections of unique value without allowing duplication. The set is similar to the array and supports both insertion and deletion methods of the array. Sets are faster than arrays in terms of searching as they use a hash table internally for storing data and can be used to replace duplicates from other data types.


**Objects**

In JavaScript, objects are key-value collections.
The most common way of creating and object.
```
const user = {
  name: "John",
  age: 30
};
```
You can also create objects dynamically and assign properties later, which is useful when building objects step by step.
```
const user = {};
user.name = "John";
```
**Getting Keys, Values, Entries**

To iterate over objects, I usually rely on built-in methods.
```
Object.keys(user);   // ["name", "age"]
Object.values(user); // ["John", 30]
Object.entries(user); // [["name", "John"], ["age", 30]]
```
I especially like Object.entries because it works nicely with destructuring in loops.
```
for (const [key, value] of Object.entries(user)) {
  console.log(key, value);
}
```
**Shallow vs Deep Copy**

**Shallow Copy**

I can create a shallow copy using spread syntax.
```
const copy = { ...user };
```

**Deep Copy**

For deep copying, one quick solution is JSON serialization.
```
JSON.parse(JSON.stringify(user));
```
But it has limitations — it removes functions, undefined, and breaks special objects like Date or Map.

A better modern solution is structuredClone.
```
const copy = structuredClone(user);
```

**Destructuring**

I use destructuring a lot to extract values from objects.

```
const { name, age } = user;
```
I can also rename variables or set default values.
```
const { name: userName, country = "USA" } = user;
```
**Object.freeze and Object.seal**

These are used to control mutability.

- Object.freeze makes the object completely immutable
- Object.seal allows modifying existing properties but prevents adding or removing them

```
const user = Object.freeze({ name: "John" });
user.name = "Mike"; // ignored
```

**Getter and Setter**

I use getters and setters when I want computed or controlled access to properties.
```
const user = {
  firstName: "John",
  lastName: "Doe",

  get fullName() {
    return this.firstName + " " + this.lastName;
  },

  set fullName(value) {
    [this.firstName, this.lastName] = value.split(" ");
  }
};
```

**Object vs Map**

Objects used for simple key-value storage with string keys.
But when I need more flexibility, I use Map.

```
const map = new Map();
map.set("name", "John");
```
Key differences:

- Objects only allow string or symbol keys
- Maps allow any type as keys
- Maps preserve insertion order
- Maps are better for frequent additions/removals

```
obj[1] === obj["1"]; // true
```
Objects convert keys to strings, which can cause issues.


**Functions**

**Declaration vs Expression vs Arrow functions**

```
//functional declaration

function sum(a, b) {
  return a + b;
}
```

```
//functional expression

const sum = function(a, b) {
  return a + b;
};
```

```
//Arrow Function

const sum = (a, b) => a + b;
```
However, arrow functions don’t have their own this, which is an important difference.

**Default Parameters**

To define default values for function parameters.
```
function greet(name = "Guest") {
  return `Hello, ${name}`;
}
```
If no argument is passed, the default is used.

**Classes**

A class in JavaScript is like a template or blueprint for creating objects.
It helps us create multiple objects with the same structure and behavior, instead of writing the same code again and again.

**Constructor**

The constructor is a special method used to initialize new instances of a class.

```
class User {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

const user = new User("John", 30);
```
**Public vs Private Fields and Methods**

By default, class fields and methods are public.
```
class User {
  name = "John";

  sayHi() {
    console.log(this.name);
  }
}
```

JavaScript also supports private fields using the # syntax.
```
class User {
  #password;

  constructor(password) {
    this.#password = password;
  }

  checkPassword(value) {
    return this.#password === value;
  }
}
```
These fields are truly private — they cannot be accessed outside the class.

```
user.#password; // ❌ Syntax error
```

**Static Methods & Properties**

Static methods and properties belong to the class itself, not to individual objects created from that class.

```
class MathUtils {
  static PI = 3.14;

  static add(a, b) {
    return a + b;
  }
}

console.log(MathUtils.PI);      // 3.14
console.log(MathUtils.add(2,3)); // 5
```

**Inheritance**

Classes can inherit from other classes using extends.
Inheritance allows one class to reuse properties and methods from another class.

```
class Animal {
  speak() {
    console.log("Animal makes a sound");
  }
}

class Dog extends Animal {
  bark() {
    console.log("Woof");
  }
}

const dog = new Dog();
dog.speak(); // inherited
dog.bark();  // own method
```

**super** keyword

super is used to access the parent class.

```
class Animal {
  constructor(name) {
    this.name = name;
    this.createdAt = Date.now(); // shared logic
  }

  getInfo() {
    return `Animal: ${this.name}`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // reuse parent logic
    this.breed = breed;
  }

  getInfo() {
    return `${super.getInfo()}, Breed: ${this.breed}`;
  }
}

const dog = new Dog("Rex", "Labrador");
console.log(dog.getInfo());
```

**Getters and Setters**

Getters and setters in classes allow us to control how properties are accessed and modified, while still using them like normal properties.

```
class User {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  get fullName() {
    return this.firstName + " " + this.lastName;
  }

  set fullName(value) {
    const parts = value.split(" ");
    this.firstName = parts[0];
    this.lastName = parts[1];
  }
}

const user = new User("John", "Doe");

console.log(user.fullName); // getter
user.fullName = "Jane Smith"; // setter
```

**Asynchronous JavaScript**

**Callbacks**

A callback is a function passed into another function to be executed later.

```
function fetchData(callback) {
  setTimeout(() => {
    callback("Data loaded");
  }, 1000);
}

fetchData((data) => {
  console.log(data);
});
```
The function doesn’t return immediately — instead, it calls the callback when the data is ready.

**Promises**

A Promise in JavaScript is an object that represents the result of an asynchronous operation, and it gives us a structured way to handle success and failure instead of using callbacks.

Why Promises exist

Before Promises, we used callbacks, which often led to deeply nested code.

Promises solve:

- readability
- chaining
- error handling


States
- pending: initial state
- fulfilled: operation succeeded
- rejected: operation failed

A Promise can change state only once.

```
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Success");
    // reject("Error"); // ignored if resolve already called
  }, 1000);
});
```

**then, catch, finally**

How Promises actually work:
- “I will give you a result later”
- You attach handlers (then, catch) to react when it's ready

.then() — Success Handling

.then() is used to handle the resolved (fulfilled) value of a Promise.

```
promise.then(result => {
  console.log(result);
});
```
.catch() — Error Handling

.catch() handles rejected Promises or errors thrown in .then().
```
promise.catch(error => {
  console.error(error);
});
```
.finally() — Cleanup

.finally() runs regardless of success or failure.
```
promise.finally(() => {
  console.log("Always runs");
});
```
then handles success, catch handles errors, and finally runs regardless of outcome.

A Promise represents the result of an async operation and allows chaining via .then(), centralized error handling via .catch(), and cleanup via .finally(). It helps avoid callback hell and makes async code more readable and maintainable.

**Promise Methods**

```
Promise.all([p1, p2, p3])
  .then(results => console.log(results));
```
Behavior
- waits for ALL promises
- fails if ONE fails

```
Promise.all([
  Promise.resolve(1),
  Promise.reject("Error"),
]);
```
rejects immediately

**Promise.race**
```
Promise.race([p1, p2]);
```
returns first settled promise (resolve OR reject)

**Promise.allSettled**
```
Promise.allSettled([p1, p2]);
```
- waits for all
- returns results for each

**Promise.any**
```
Promise.any([p1, p2]);
```
- resolves with FIRST successful promise
- ignores failures (unless all fail)

Method	Behavior

all -	all must succeed

race -	first settles

allSettled -	wait for all

any	- first success

**async/await**

async/await is syntactic helper over Promises — it doesn’t replace them, it just makes them easier to read and write.


**Client Side (Browser APIs)**

**Global Object Window**

The **document** object represents the HTML page. It allows us to read and manipulate the DOM.
```
document.getElementById("app");
document.querySelector(".item");
```
We use it to:

- select elements
- change content
- handle events

```
document.querySelector("button").addEventListener("click", () => {
  console.log("Clicked");
});
```

The **location** object can be used to get the current page address (URL) and to redirect the browser to a new page.

```
location.href = "https://example.com"; // redirect
location.reload(); // reload page
```

**history** represents the session history stack of the current browser tab. We can think of it as a list of pages the user visited in this tab.

Internally, the browser keeps something like:
```
["/home", "/products", "/product/1"]
```
And a pointer to current position:
```
          👇
["/home", "/products", "/product/1"]
```
**history** is mainly used for client-side routing and navigation control without reloading the page.

**navigator** gives information about the user’s environment — browser, device, capabilities — and also provides access to some system-level APIs.

There are two main uses:

- Information (what device/browser)
- Capabilities (what the browser can do)

I used it only for geolocation.


On the client side, we have several ways to store data in the browser: **localStorage**, **sessionStorage**, and **cookies**. Each has different behavior, lifetime, and use cases.

**localStorage** is used to store data in the browser persistently, meaning it stays even after the browser is closed.

```
localStorage.setItem("name", "John");
localStorage.getItem("name"); // "John"
localStorage.removeItem("name");
localStorage.clear();
```
Key characteristics
- Stores key-value pairs (strings only)
- Persistent (no expiration)
- Shared across tabs (same origin)
- ~5MB storage limit
- Synchronous API

Real-world use cases
- theme (dark/light)
- user preferences
- caching small data


**sessionStorage** is similar to localStorage, but it is cleared when the tab is closed.

Real-world use cases
- form progress
- temporary tokens
- wizard steps


**Cookies** are small pieces of data stored in the browser and automatically sent to the server with every HTTP request.

```
document.cookie = "username=John";
```

Setting with options
```
document.cookie = "token=abc; max-age=3600; path=/";
```

Key characteristics
- Sent with every request (important!)
- Small size (~4KB)
- Can have expiration
- Used for authentication/session


I avoid storing sensitive data like JWT tokens in localStorage because it’s vulnerable to XSS attacks. Instead, I prefer HttpOnly cookies for authentication.

**DOM Manipulation**

**getElementById**: This method retrieves an element by its unique ID attribute.
```
let element = document.getElementById("myElement");
```
getElementsByClassName: This method returns a collection of elements with the specified class name.
```
let elements = document.getElementsByClassName("myClass");
```
getElementsByTagName: This method returns a list of collection of elements with the specified tag name.
```
let elements = document.getElementsByTagName("div");
```
querySelector: This method retrieves the first element that matches a specified CSS selector.

querySelectorAll: This method retrieves all elements that match a CSS selector.


**Creating elements (createElement, createTextNode)**

To create DOM elements dynamically we use createElement.
```
const div = document.createElement("div");
```

createElement creates an element, and createTextNode creates text content.
```
const text = document.createTextNode("Hello");
div.appendChild(text);
```

**appendChild**

```
parent.appendChild(div);
```
- Adds as last child
- Returns appended element

**append**

```
parent.append(div);
```
Can accept:
- multiple elements
- strings

```
parent.append("Hello");       // ✅ works
parent.appendChild("Hello");  // ❌ error
```

**insertBefore** allows me to insert a node before a specific child inside a parent element.

```
const parent = document.querySelector("ul");

const newItem = document.createElement("li");
newItem.textContent = "New item";

const secondItem = parent.children[1];

parent.insertBefore(newItem, secondItem);
```
before
```
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
</ul>
```
after
```
<ul>
  <li>Item 1</li>
  <li>New item</li>
  <li>Item 2</li>
</ul>
```

**insertAdjacentHTML** allows me to insert HTML string directly into the DOM at a specific position, without recreating the entire element.

syntax
```
element.insertAdjacentHTML(position, htmlString);
```
```
element.insertAdjacentHTML("beforebegin", "<p>Hi</p>");
```
BEFORE the element itself
```
element.insertAdjacentHTML("afterbegin", "<p>Hi</p>");
```
FIRST child inside element

```
element.insertAdjacentHTML("beforeend", "<p>Hi</p>");
```
LAST child inside element

```
element.insertAdjacentHTML("afterend", "<p>Hi</p>");
```
AFTER the element

**Visual example**
```
<div id="box">Content</div>
```
"beforebegin"
```
<p>Hi</p>
<div id="box">Content</div>
```
"afterbegin"
```
<div id="box">
  <p>Hi</p>
  Content
</div>
```


**HTML/CSS Basics**

**Selectors**

1.1 Element Selector
```
div {
  color: red;
}
```
1.2 Class Selector
```
.button {
  background: blue;
}
```
```
<button class="button">Click</button>
```
Reusable, most common in real projects

1.3 ID Selector
```
#header {
  height: 60px;
}
```
```
<div id="header"></div>
```
Should be unique per page

1.4 Attribute Selector
```
input[type="text"] {
  border: 1px solid gray;
}
```
Other variations:
```
a[target="_blank"]   /* exact match */
a[href^="https"]    /* starts with */
a[href$=".pdf"]     /* ends with */
a[href*="google"]   /* contains */
```

Selector specificity and weights

Inline styles	1000
ID	100
Class/attr/pseudo-class	10
Element/pseudo-element	1
```
#id { color: red; }        /* 100 */
.class { color: blue; }    /* 10 */
div { color: green; }      /* 1 */
```
#id wins

Important Rules
1. Higher specificity wins
2. If equal → last one wins
```
.button {
  color: red;
}

.button {
  color: blue;
}
```
blue wins

!important overrides everything (almost)
```
.button {
  color: red !important;
}
```

Edge Case (INTERVIEW FAVORITE)

```
#id .class {
  color: red;
}

.class {
  color: blue;
}
```
#id .class wins (higher specificity)

**Combinator Selectors**

These define relationships between elements.
3.1 Descendant ( ) — space
```
div p {
  color: red;
}
```
Targets ALL p inside div (any depth)
```
<div>
  <section>
    <p>✔ matched</p>
  </section>
</div>
```
3.2 Child (>)
```
div > p {
  color: red;
}
```
Only direct children
```
<div>
  <p>✔ matched</p>
  <section>
    <p>❌ NOT matched</p>
  </section>
</div>
```
3.3 Adjacent sibling (+)
```
h1 + p {
  color: red;
}
```
First p immediately after h1
```
<h1></h1>
<p>✔ matched</p>
<p>❌ NOT matched</p>
```
3.4 General sibling (~)
```
h1 ~ p {
  color: red;
}
```
All p after h1 (same level)
```
<h1></h1>
<p>✔</p>
<p>✔</p>
```

**Pseudo-classes vs Pseudo-elements**

Pseudo-classes (:)

Represent a state of an element

Examples:
- :hover
- :focus
- :active
- :nth-child()

✔ Affect existing elements

Pseudo-elements (::)

Represent virtual elements (parts of DOM)
Examples:
- ::before
- ::after
- ::first-letter

✔ Create or target parts of elements


**CSS Units**

Two big groups:

✅ Absolute units

Fixed, do NOT depend on anything

✅ Relative units

Depend on parent / root / viewport

Absolute Units
📌 px (pixels)

```
div {
  width: 200px;
}
```
Most commonly used absolute unit

3. Relative Units

3.1 em

Relative to parent’s font-size
```
.parent {
  font-size: 20px;
}

.child {
  font-size: 2em; /* 40px */
}
```
3.2 rem

Relative to root (html) font-size
```
html {
  font-size: 16px;
}

h1 {
  font-size: 2rem; /* 32px */
}
```
3.3 %

Relative to parent
```
div {
  width: 50%;
}
```
50% of parent width

3.4 vw / vh

Relative to viewport

```
div {
  width: 50vw;  /* 50% of screen width */
  height: 100vh; /* full screen height */
}
```

Tasks

Easy Level
1. Sum of Array Elements
Write a function that takes an array of numbers and returns their sum.
Example: sumArray([1, 2, 3, 4]) → 10

Solution 1 — Basic loop (baseline)
```
function sumArray(arr) {
  let sum = 0;

  for (let i = 0; i < arr.length; i++) {
    sum += arr[i];
  }

  return sum;
}
```
Solution 2 — for...of

```
function sumArray(arr) {
  let sum = 0;

  for (const num of arr) {
    sum += num;
  }

  return sum;
}
```

Solution 3 — reduce (senior-level expectation)
```
function sumArray(arr) {
  return arr.reduce((acc, curr) => acc + curr, 0);
}
```

2. Reverse a String (NO reverse())

Write a function that reverses a string without using built-in reverse method.
Example: reverseString("hello") → "olleh"

Solution 1 — Loop (classic)
```
function reverseString(str) {
  let result = "";

  for (let i = str.length - 1; i >= 0; i--) {
    result += str[i];
  }

  return result;
}
```

Solution 2 — Two pointers (strong signal)
```
function reverseString(str) {
  const arr = str.split("");
  let left = 0;
  let right = arr.length - 1;

  while (left < right) {
    [arr[left], arr[right]] = [arr[right], arr[left]];
    left++;
    right--;
  }

  return arr.join("");
}
```
Solution 3 — Recursion
```
function reverseString(str) {
  if (str === "") return "";
  return reverseString(str.slice(1)) + str[0];
}
```
3. Find Maximum Number

Write a function that finds the largest number in an array without using Math.max.
Example: findMax([3, 7, 2, 9, 1]) → 9

Solution 1 — Loop
```
function findMax(arr) {
  let max = arr[0];

  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
      max = arr[i];
    }
  }

  return max;
}
```
Solution 2 — for...of
```
function findMax(arr) {
  let max = -Infinity;

  for (const num of arr) {
    if (num > max) max = num;
  }

  return max;
}
```
Solution 3 — reduce
```
function findMax(arr) {
  return arr.reduce((max, curr) => (curr > max ? curr : max), -Infinity);
}
```

4. Count Vowels

Write a function that counts the number of vowels (a, e, i, o, u) in a string.
Example: countVowels("hello world") → 3

Solution 1 — Loop + includes
```
function countVowels(str) {
  const vowels = "aeiou";
  let count = 0;

  for (const char of str.toLowerCase()) {
    if (vowels.includes(char)) {
      count++;
    }
  }

  return count;
}
```

Solution 2 — Set (more optimal lookup)
```
function countVowels(str) {
  const vowels = new Set(["a", "e", "i", "o", "u"]);
  let count = 0;

  for (const char of str.toLowerCase()) {
    if (vowels.has(char)) count++;
  }

  return count;
}
```
Solution 3 — Regex
```
function countVowels(str) {
  const matches = str.match(/[aeiou]/gi);
  return matches ? matches.length : 0;
}
```

**Medium tasks**
6. Remove Duplicates

Write a function that removes duplicate values from an array.
Example: removeDuplicates([1, 2, 2, 3, 4, 4, 5]) → [1, 2, 3, 4, 5]

Solution 1 — Set (most expected)
```
function removeDuplicates(arr) {
  return [...new Set(arr)];
}
```
Solution 2 — filter + indexOf
```
function removeDuplicates(arr) {
  return arr.filter((item, index) => arr.indexOf(item) === index);
}
```
Solution 3 — reduce
```
function removeDuplicates(arr) {
  return arr.reduce((acc, curr) => {
    if (!acc.includes(curr)) acc.push(curr);
    return acc;
  }, []);
}
```
8. Flatten Array

Write a function that flattens a nested array one level deep.
Example: flattenArray([1, [2, 3], [4, [5]]]) → [1, 2, 3, 4, [5]]

Solution 1
```
function flattenArray(arr) {
  return arr.flat(1);
}
```
Solution 2
```
function flattenArray(arr) {
  return arr.reduce((acc, curr) => {
    return acc.concat(curr);
  }, []);
}
```
Solution 3
```
function flattenArray(arr) {
  const result = [];

  for (const item of arr) {
    if (Array.isArray(item)) {
      result.push(...item);
    } else {
      result.push(item);
    }
  }

  return result;
}
```

9. Object Property Counter

Write a function that counts how many times each element appears in an array and returns an object.
Example: countOccurrences(['a', 'b', 'a', 'c', 'b', 'a']) → {a: 3, b: 2, c: 1}

Solution 1 — Basic object
```
function countOccurrences(arr) {
  const result = {};

  for (const item of arr) {
    result[item] = (result[item] || 0) + 1;
  }

  return result;
}
```
Solution 2 — reduce
```
function countOccurrences(arr) {
  return arr.reduce((acc, curr) => {
    acc[curr] = (acc[curr] || 0) + 1;
    return acc;
  }, {});
}
```
Solution 3 — Map (better for non-strings)
```
function countOccurrences(arr) {
  const map = new Map();

  for (const item of arr) {
    map.set(item, (map.get(item) || 0) + 1);
  }

  return Object.fromEntries(map);
}
```
Solution 4 - for loop
```
function countOccurrences(arr) {
  const result = {};

  for (let i = 0; i < arr.length; i++) {
    const value = arr[i];

    if (result[value]) {
      result[value] = result[value] + 1;
    } else {
      result[value] = 1;
    }
  }

  return result;
}
```
10. Capitalize Words

Write a function that capitalizes the first letter of each word in a sentence.
Example: capitalizeWords("hello world from javascript") → "Hello World From Javascript"

Solution 1 — Split/map/join
```
function capitalizeWords(str) {
  return str
    .split(" ")
    .map(word => word[0].toUpperCase() + word.slice(1))
    .join(" ");
}
```
Solution 2 — Safe version
```
function capitalizeWords(str) {
  return str
    .trim()
    .split(/\s+/)
    .map(word => word.charAt(0).toUpperCase() + word.slice(1))
    .join(" ");
}
```
Solution 3 — Regex
```
function capitalizeWords(str) {
  return str.replace(/\b\w/g, char => char.toUpperCase());
}
```
Solution 4 - for loop
```
function capitalizeWords(str) {
  let result = "";
  let capitalizeNext = true;

  for (let i = 0; i < str.length; i++) {
    const char = str[i];

    if (char === " ") {
      result += char;
      capitalizeNext = true;
    } else if (capitalizeNext) {
      result += char.toUpperCase();
      capitalizeNext = false;
    } else {
      result += char;
    }
  }

  return result;
}
```
