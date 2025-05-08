## interview questions blog task ##


## 1. What are some differences between interfaces and types in TypeScript?

interfaces and types... they are like two ways to tell TypeScript what shape my data should have.

*   **Interface:** This one is good for describing how objects look Like, an object must have a `name` (string) and `age` (number). The good thing is, i can `extend` an interface, like add more stuff to it later. Also, classes can use interfaces to promise they will have certain things.

*   **Type:** This one is more strong. It can also describe objects, same like interface. But type can do more. It can say a variable can be a `number` OR a `string` (this is called union). It can also make like a specific list of types together (like a tuple). But, once I make a type, then i cannot add more things to it later like interface.

So, interfaces are good for object shapes and class contracts, and I can extend them. Types are more flexible for unions, tuples, and other patterns, but I0 cannot add more to them later.

## 2. What is the use of the `keyof` keyword in TypeScript? Provide an example.

The `keyof` keyword is useful. It helps me get all the property names from an object type. It makes a "union" type of all the names.

Why use it? It helps me make functions safer. Like, a function that takes an object and wants to get a property. I can use `keyof` to make sure the user only gives me a name that is *actually* inside that object.

Example:

```ts
type Person = {
  name: string;
  age: number;
  city: string;
};

// keyof Person will give me "name" | "age" | "city"

function getPersonProperty<T, K extends keyof T>(person: T, key: K): T[K] {
  return person[key]; // TypeScript checks if 'key' is a real property of 'person'
}

const myPerson: Person = {
  name: "Jamal",
  age: 30,
  city: "Dhaka"
};

let personName = getPersonProperty(myPerson, "name"); // This is okay, "name" is a keyof Person
let personAge = getPersonProperty(myPerson, "age");   // This is okay
let personAddress = getPersonProperty(myPerson, "address"); // This would be error! Because "address" is not a keyof Person

console.log(personName); // Output: Jamal
console.log(personAge);  // Output: 30

See? keyof helps me make sure I ask for a property name that really exists in the object type.

3. Explain the difference between `any`, `unknown`, and `never`, types in TypeScript.

## These are special types:

any: This is like turning off TypeScript checking. If I say a variable is any, I can put anything in it (number, string, object) and I can do anything with it (call it like a function, access any property). This is easy when I don't know the type, but it is dangerous because TypeScript will not find my mistakes at the time I write code. Mistakes will happen when I run the code.

unknown: This is better than any. It means "I don't know the type yet." Like any, I can put anything in a variable of type unknown. BUT, if I want to use the value (like call a method or access a property), TypeScript says "Hold on! I don't know what type this is! You must check it first." then I have to use typeof or other checks (type guards) to tell TypeScript the type before I use it. It is safer.

never: This means something that will never happen. Like, a function that always throws an error and never finishes. Or a code path that can never be reached because of other logic. If a function's return type is never, it means the function will not return normally.

So, any = no safety, unknown = safe, must check type, never = this will not happen.

4. What is the use of enums in TypeScript? Provide an example of a numeric and string enum.

## Enums are like lists of names that have a special meaning. They help make my code more readable and understandable. Instead of using numbers like 0, 1, 2 for something, I can give them names like Red, Green, Blue. It's much easier to read Color.Red than just the number 0.

TypeScript enums can be numeric or string.
Numeric Enum: By default, enums are numbers starting from 0. Or I can give the first one a number, and the rest will follow.
// Numeric enum (starts at 0 by default)
enum Direction {
  Up,    // 0
  Down,  // 1
  Left,  // 2
  Right  // 3
}

let myDirection: Direction = Direction.Up;
console.log(myDirection); // Output: 0

// Or I can start at a different number
enum Status {
  Loading = 100,
  Success, // 101
  Error    // 102
}
let currentStatus: Status = Status.Success;
console.log(currentStatus); // Output: 101

String Enum: I can also use strings for the values. This is also good for readability.
// String enum
enum UserRole {
  Admin = "ADMIN",
  Editor = "EDITOR",
  Viewer = "VIEWER"
}

let userOneRole: UserRole = UserRole.Admin;
console.log(userOneRole); // Output: ADMIN

Enums make my code cleaner because I use names instead of magic numbers or strings.

5. What is type inference in TypeScript? Why is it helpful?

## Type inference is a smart thing TypeScript does. It means TypeScript can guess or figure out the type of a variable or the return type of a function even if I don't write the type name myself.

Example:
let count = 5; // I did not say ': number', but TypeScript knows 'count' is a number because I put 5 in it.

let message = "Hello world"; // TypeScript knows 'message' is a string.

function add(a: number, b: number) {
  return a + b; // TypeScript knows this function will return a number.
}

let result = add(2, 3); // TypeScript knows 'result' will be a number.

Why is it helpful?
Less writing: I don't need to write types everywhere, which makes the code shorter and faster to write.
Still safe: Even if I don't write the type, TypeScript still checks if I use the variable correctly based on the type it figured out. So I get the safety without writing a lot.

6. How does TypeScript help in improving code quality and project maintainability?

## TypeScript is very helpful for writing better code and keeping a project easy to manage for a long time.
Finds mistakes early: The biggest thing is that TypeScript checks my code before I run it (at "compile time"). It finds many type-related errors (like trying to add a number to a string by mistake) that would only happen when I run the program in normal JavaScript. Finding mistakes early saves a lot of time and headache.

Better help from code editors (IDE support): Because TypeScript knows the types, code editors like VS Code can give me much better help. They can automatically finish my code (autocompletion), tell me if I made a mistake right away (error highlighting), and help me change code safely (refactoring).
Code is like documentation: When I put types on my functions and variables, other people (or myself later!) can easily see what type of data a function needs or what type of data a variable will hold.

This explains the code structure clearly without writing separate documents sometimes. It makes working together easier. So, fewer bugs, faster writing with editor help, and easier to understand code for everyone.

7. Provide an example of using union and intersection types in TypeScript.


## Union Type: This means a variable or parameter can be one type OR another type. I use the pipe symbol (|).
// A variable can be a number OR a string
type Id = number | string;

let userId: Id;

userId = 123;     // Okay, number is allowed
userId = "abc456"; // Okay, string is allowed
// userId = true;   // Error! boolean is not in the union

This is good when a function can accept different types of input.


Intersection Type: This means a new type that combines properties from multiple types. I use the ampersand symbol (&). It means the new type must have all the properties from the types it is combining.

interface Draggable {
  drag(): void;
}

interface Resizable {
  resize(): void;
}

// A UIWidget must be both Draggable AND Resizable
type UIWidget = Draggable & Resizable;

let myWidget: UIWidget = {
  drag: () => { console.log("Dragging..."); },
  resize: () => { console.log("Resizing..."); }
};

myWidget.drag();   // Okay
myWidget.resize(); // Okay
// myWidget.close(); // Error! close() is not in UIWidget```

Union means "one of these", Intersection means "all of these together".