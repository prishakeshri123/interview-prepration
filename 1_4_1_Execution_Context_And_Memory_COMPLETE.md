# 📖 1.4.1: Execution Context & Memory
## Complete Mastery Guide: Teach + Interview + ELI5 + Quiz + Challenge

---

## 🎯 PART 0: 30-Second Rule 0 Explanation

### The Concept in 30 Seconds

**Execution Context** is the "environment" where JavaScript code runs—it tracks what variables exist, what function is running, and what values are available. **Memory** is where JavaScript stores those variables and values in your computer's RAM. When code runs, JavaScript creates execution contexts (call stack), stores data in memory (heap), and cleans up when done (garbage collection).

### Real-World Why This Matters

Understanding execution context and memory is **the difference between debugging efficiently and spending hours lost**.

**Real-world scenarios:**

1. **Memory Leaks**: A global variable holds onto data forever, eating RAM. Your app crashes after hours. You need to understand where memory lives to fix it.

2. **Scope Issues**: A variable you thought was global is actually local. Your code breaks because you don't understand execution context.

3. **Performance**: Storing too much in memory = slow app. Creating millions of execution contexts = slow app. You need mental models of how this works.

4. **Callback Issues**: Inside a callback, `this` points somewhere unexpected. This happens because each function call creates a new execution context with its own `this`.

### Production Impact

At companies like Google and Meta:

- **Memory profiling** is a daily task. Bad memory management = app crashes for millions of users
- **Execution context bugs** are top causes of production issues
- **Call stack overflows** happen when recursion doesn't respect execution context rules
- **Garbage collection pauses** can make UX lag (companies spend billions optimizing this)

Netflix estimates **memory leaks cost them ~$2M/year** in extra server resources. Understanding execution context would have prevented it.

---

## 📚 PART 1: TEACH - Deep Dive Learning

### What Is Execution Context?

**Execution Context** is the "environment" where JavaScript code executes. It's like the stage where your code performs. Each time a function runs, a new execution context is created. Each execution context has:

1. **Variable Environment**: Where variables (`var`, `let`, `const`) live
2. **Lexical Environment**: Where function definitions and their scope live
3. **`this` Binding**: What `this` refers to in that context

**Brief History:**

- **ES5**: Introduced formal execution context spec
- **ES6/ES2015**: Introduced `let` and `const` with block scope (created new lexical environments)
- **Modern**: Call stack visualization in DevTools makes this visible

### Core Concepts

#### 1. The Call Stack (CRITICAL)

The **call stack** is a data structure that tracks execution contexts. When you call a function, a new context goes on the stack. When the function returns, its context is removed.

**Visual Example:**

```
Call Stack Evolution:

Step 1: Program starts
┌─────────────────────┐
│ Global Execution    │  <- Main context
│ Context             │
└─────────────────────┘

Step 2: function greet() called
┌─────────────────────┐
│ greet() context     │  <- New context on stack
├─────────────────────┤
│ Global Execution    │
│ Context             │
└─────────────────────┘

Step 3: greet() calls sayHello()
┌─────────────────────┐
│ sayHello() context  │  <- Another new context
├─────────────────────┤
│ greet() context     │
├─────────────────────┤
│ Global Execution    │
│ Context             │
└─────────────────────┘

Step 4: sayHello() returns
┌─────────────────────┐
│ greet() context     │  <- sayHello removed
├─────────────────────┤
│ Global Execution    │
│ Context             │
└─────────────────────┘

Step 5: greet() returns
┌─────────────────────┐
│ Global Execution    │  <- greet removed
│ Context             │
└─────────────────────┘
```

**Code Example:**

```javascript
// ✅ Understanding the call stack

function first() {
  console.log('First function');
  second(); // Adds second() to call stack
}

function second() {
  console.log('Second function');
  third();  // Adds third() to call stack
}

function third() {
  console.log('Third function');
  // No more calls, returns
}

first(); // Adds first() to call stack

// Call stack progression:
// 1. first() called → [first]
// 2. second() called → [first, second]
// 3. third() called → [first, second, third]
// 4. third() returns → [first, second]
// 5. second() returns → [first]
// 6. first() returns → []
// 7. Done

// Console output:
// First function
// Second function
// Third function
```

**Call Stack Overflow Example (GOTCHA):**

```javascript
// ❌ Stack overflow - too many contexts!
function infiniteRecursion() {
  infiniteRecursion(); // Calls itself forever
  // Call stack grows: [infiniteRecursion, infiniteRecursion, infiniteRecursion, ...]
  // Eventually: Maximum call stack size exceeded ERROR
}

infiniteRecursion();

// ✅ Safe recursion - respects call stack
function countdown(n) {
  if (n <= 0) return; // BASE CASE - stops recursion
  
  console.log(n);
  countdown(n - 1); // Call stack grows, then shrinks
}

countdown(5); // [countdown(5), countdown(4), countdown(3), ...]
// Output: 5, 4, 3, 2, 1
```

#### 2. The Memory Heap

The **heap** is where JavaScript stores complex objects and arrays. Unlike primitives (which are small and live in the call stack), objects are large and dynamic, so they live in heap memory.

**Stack vs Heap:**

```
┌──────────────────────────────────────────────────┐
│ MEMORY DIAGRAM                                   │
├──────────────────────────────────────────────────┤
│                                                  │
│ CALL STACK (Automatic, organized)               │
│ ┌─────────────────────────────────────────┐     │
│ │ Execution Context                       │     │
│ │ ├─ Primitive values (numbers, strings)  │     │
│ │ ├─ Variable references (point to heap)  │     │
│ │ └─ Function references                  │     │
│ └─────────────────────────────────────────┘     │
│                                                  │
│ HEAP (Manual, scattered)                        │
│ ┌─────────────────────────────────────────┐     │
│ │ Objects and Arrays (scattered in heap)  │     │
│ │ ├─ { name: "Alice", age: 30 }           │     │
│ │ ├─ [1, 2, 3, 4, 5, ...]                 │     │
│ │ ├─ { x: 10, y: 20, z: 30 }              │     │
│ │ └─ Large data structures                │     │
│ └─────────────────────────────────────────┘     │
│                                                  │
└──────────────────────────────────────────────────┘
```

**Code Example:**

```javascript
// ✅ Understanding stack vs heap

// PRIMITIVES go on the STACK
let age = 30;           // Stack: age → 30
let name = "Alice";     // Stack: name → "Alice"
let active = true;      // Stack: active → true

// OBJECTS go on the HEAP
let user = {
  name: "Alice",
  age: 30
};
// Stack: user → [pointer to heap]
// Heap: { name: "Alice", age: 30 }

// ❌ What happens with object assignment
let user1 = { name: "Alice" };
let user2 = user1;  // Both point to SAME object in heap!

user2.name = "Bob";
console.log(user1.name); // "Bob" - user1 changed too!

// ✅ Why? Both variables point to same memory location
// user1 → [heap location #1000]
// user2 → [heap location #1000]
// When we change what's at #1000, both see the change

// ✅ To create independent copy, use spread operator
let user3 = { ...user1 }; // New object, different heap location
user3.name = "Charlie";
console.log(user1.name); // "Bob" - user1 unchanged
```

**Real-world impact:**

```javascript
// ❌ Common mistake: Modifying array passed to function
function addItem(items) {
  items.push("new item"); // Modifies ORIGINAL array!
}

const myItems = ["item1"];
addItem(myItems);
console.log(myItems); // ["item1", "new item"] - unintended change!

// ✅ Better: Create new array, don't modify original
function addItem(items) {
  return [...items, "new item"]; // Return new array
}

const myItems = ["item1"];
const updatedItems = addItem(myItems);
console.log(myItems); // ["item1"] - unchanged
console.log(updatedItems); // ["item1", "new item"] - new array
```

#### 3. Global Execution Context (GEC)

The **Global Execution Context** is created when JavaScript starts. It's the root context where global variables and functions live.

```javascript
// Global Execution Context is created here
var globalVar = "I'm global";
let globalLet = "I'm also global";
const globalConst = "Me too";

function globalFunction() {
  console.log("Global function");
}

// In browser:
console.log(window.globalVar);      // "I'm global" (var is in window)
console.log(window.globalLet);      // undefined (let not in window)
console.log(globalLet);              // "I'm also global" (but accessible)

// In Node.js:
console.log(global.globalVar);      // "I'm global"

// ❌ Polluting global scope (BAD)
function badFunction() {
  globalCounter = 0;  // Accidentally creates global! (no var/let/const)
}

// ✅ Using module scope (GOOD)
function goodFunction() {
  let localCounter = 0; // Local to function
}
```

#### 4. Function Execution Context

Each time a function is called, a new **Function Execution Context** is created.

```javascript
// ✅ Creating function execution contexts

function createUser(name, age) {
  // New execution context created with:
  // - name parameter
  // - age parameter
  // - local variables
  
  let email = `${name}@example.com`; // Local variable
  const user = { name, age, email };  // Local object
  
  return user;
}

// Calling function 3 times = 3 separate execution contexts
const user1 = createUser("Alice", 30); // Context 1
const user2 = createUser("Bob", 25);   // Context 2
const user3 = createUser("Charlie", 35); // Context 3

// Each has its own variables:
// Context 1: name="Alice", age=30, email="alice@example.com"
// Context 2: name="Bob", age=25, email="bob@example.com"
// Context 3: name="Charlie", age=35, email="charlie@example.com"

// When function returns, execution context is destroyed
// Variables are garbage collected
```

**Function context with `this`:**

```javascript
// ✅ Understanding this in execution context

const person = {
  name: "Alice",
  greet: function() {
    console.log(`Hello, I'm ${this.name}`);
    // 'this' in this context refers to 'person' object
  }
};

person.greet(); // Output: "Hello, I'm Alice"

// But if you extract the method:
const greet = person.greet;
greet(); // Output: "Hello, I'm undefined"
// 'this' is now undefined (or window in non-strict mode)
// Because 'this' is determined by execution context

// ✅ Binding 'this' explicitly
const boundGreet = person.greet.bind(person);
boundGreet(); // Output: "Hello, I'm Alice"
// bind() creates new context with 'this' = person
```

#### 5. Lexical Environment (Scope Chain)

**Lexical Environment** defines which variables a function can access. JavaScript looks for variables in this order:

1. **Local Scope**: Variables in current execution context
2. **Outer Function Scope**: Variables in parent function
3. **Global Scope**: Global variables

```javascript
// ✅ Scope chain demonstration

let global = "I'm global";

function outer() {
  let outerVar = "I'm in outer";
  
  function inner() {
    let innerVar = "I'm in inner";
    
    console.log(innerVar);   // "I'm in inner" - local scope
    console.log(outerVar);   // "I'm in outer" - outer function scope
    console.log(global);     // "I'm global" - global scope
  }
  
  inner();
}

outer();

// Scope Chain for inner():
// [inner's variables] → [outer's variables] → [global variables]

// ❌ Reverse lookup DOESN'T work
function outer2() {
  function inner2() {
    let innerOnly = "only in inner";
  }
  
  inner2();
  console.log(innerOnly); // ReferenceError! Can't access inner's variables
}

// ✅ Understanding scope chain
function demonstrate() {
  let x = 1;
  
  function level1() {
    let y = 2;
    
    function level2() {
      let z = 3;
      console.log(x, y, z); // 1, 2, 3 - all accessible
    }
    
    level2();
  }
  
  level1();
}

demonstrate();
```

#### 6. Garbage Collection (Memory Cleanup)

**Garbage Collection** is the automatic process that frees memory from variables no longer needed.

**Two main strategies:**

**A) Mark & Sweep (Most Common):**
```javascript
// Mark & Sweep algorithm

let user = { name: "Alice" };
let temp = user;

// Both point to same object
// Objects marked as "reachable"

user = null;    // user no longer points to object
                // But temp still does, so object stays in memory

temp = null;    // Now nobody points to object
                // Object is marked as "garbage"
                // GC collects it and frees memory

// ✅ After GC: Memory freed, object is gone
```

**B) Reference Counting:**
```javascript
// Reference counting (older approach)

let obj = { data: "important" };  // Reference count: 1
let ref1 = obj;                   // Reference count: 2
let ref2 = obj;                   // Reference count: 3

obj = null;     // Reference count: 2
ref1 = null;    // Reference count: 1
ref2 = null;    // Reference count: 0
                // Object garbage collected!
```

**Memory Leaks (When GC fails):**

```javascript
// ❌ Global variable leak
function createUser() {
  userData = { name: "Alice", data: [1,2,3] }; // Forgot let/const!
  // This lives forever in global scope
  // Memory never freed!
}

createUser();
// userData still in memory, can't be garbage collected

// ✅ Fix: Use proper scope
function createUser() {
  const userData = { name: "Alice", data: [1,2,3] }; // Proper scope
}

createUser();
// userData garbage collected after function returns

// ❌ Event listener leak
button.addEventListener("click", function() {
  console.log("clicked!");
}); // If button is removed from DOM, listener still in memory

// ✅ Clean up when done
const handler = () => console.log("clicked!");
button.addEventListener("click", handler);
// Later, when removing button:
button.removeEventListener("click", handler);
```

#### 7. Hoisting

**Hoisting** is JavaScript moving variable and function declarations to the top of their scope before code executes.

```javascript
// ✅ Function hoisting (functions fully hoisted)

console.log(add(5, 3)); // Works! Output: 8
// Function declarations are fully hoisted to top

function add(a, b) {
  return a + b;
}

// ❌ Variable hoisting (tricky!)

console.log(x); // undefined (not an error!)
var x = 5;
console.log(x); // 5

// JavaScript sees it as:
// var x;              <- Declaration hoisted
// console.log(x);     <- x is undefined here
// x = 5;              <- Assignment NOT hoisted
// console.log(x);     <- x is 5 here

// ✅ let and const (safer - TDZ)

console.log(y); // ReferenceError: Cannot access 'y' before initialization
let y = 5;

// let/const are hoisted but in "Temporal Dead Zone"
// You can't access them before declaration
// This prevents accidental undefined errors
```

### Visual Explanations

#### Execution Context Lifecycle

```
┌────────────────────────────────────────────────────────────────┐
│ EXECUTION CONTEXT CREATION PHASE                               │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│ Step 1: Create Variable Environment                           │
│   ├─ Declare all variables with 'undefined' (hoisting)        │
│   ├─ var → undefined                                          │
│   ├─ let/const → TDZ (Temporal Dead Zone)                     │
│   └─ function → full function                                 │
│                                                                │
│ Step 2: Bind 'this'                                           │
│   └─ this = object.method (method call)                       │
│   └─ this = undefined (regular function, strict)             │
│   └─ this = window (regular function, non-strict)            │
│                                                                │
│ Step 3: Create Outer Reference (Scope Chain)                  │
│   └─ Link to parent execution context                         │
│   └─ Can access parent variables                              │
│                                                                │
└────────────────────────────────────────────────────────────────┘
         ↓
┌────────────────────────────────────────────────────────────────┐
│ EXECUTION CONTEXT EXECUTION PHASE                              │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│ Step 1: Execute Code Line by Line                             │
│   ├─ Assign values to variables                               │
│   ├─ Call functions (create new contexts)                     │
│   └─ Evaluate expressions                                     │
│                                                                │
│ Step 2: Track Call Stack                                      │
│   └─ Add/remove execution contexts                            │
│                                                                │
│ Step 3: Memory Management                                     │
│   ├─ Objects stored in heap                                   │
│   ├─ References stored in stack                               │
│   └─ Garbage collection runs                                  │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

#### Memory Lifecycle

```
OBJECT IN MEMORY:

Creation Phase:
  let user = { name: "Alice", age: 30 };
  Stack: user → [heap pointer #5000]
  Heap #5000: { name: "Alice", age: 30 }

Usage Phase:
  console.log(user.name); // Access via pointer
  user.age = 31;          // Modify object in heap

Reference Phase (Multiple References):
  let u1 = user;          // u1 → [#5000]
  let u2 = user;          // u2 → [#5000]
  // Object has 3 references (user, u1, u2)

Cleanup Phase:
  u1 = null;              // 2 references left
  u2 = null;              // 1 reference left
  user = null;            // 0 references left
  
  Garbage Collector marks as "garbage" and collects memory
  Heap #5000: [freed for other objects]
```

### Code Examples

#### Complete Execution Context Example

```javascript
// ✅ Complex example showing everything

var globalVar = "global";
let globalLet = "global let";

function outer(param1) {
  // New execution context: outer
  // Variable environment: param1, outerVar
  // 'this' is bound based on how outer() is called
  
  var outerVar = "outer";
  let outerLet = "outer let";
  
  function inner(param2) {
    // New execution context: inner
    // Can access: param2, innerVar, param1, outerVar, globalVar
    
    var innerVar = "inner";
    let innerLet = "inner let";
    
    console.log(param2);      // param2 value
    console.log(innerVar);    // "inner"
    console.log(param1);      // param1 value (from outer scope)
    console.log(outerVar);    // "outer" (from outer scope)
    console.log(globalVar);   // "global" (from global scope)
  }
  
  inner("inner param");
  
  // console.log(param2);  // ReferenceError! param2 is in inner, not here
}

outer("outer param");

// Execution progression:
// 1. Global context created with globalVar, globalLet, outer
// 2. outer() called → new context pushed to call stack
// 3. inner() called → new context pushed to call stack
// 4. inner() logs values → accesses scope chain
// 5. inner() returns → context popped from stack
// 6. outer() returns → context popped from stack
// 7. Program ends → global context cleaned up
```

#### Memory Management Example

```javascript
// ✅ Memory management patterns

// Pattern 1: Proper cleanup
function processLargeData() {
  let largeArray = new Array(1000000).fill(0); // 4MB in memory
  let result = largeArray.map(x => x * 2);
  return result;
  // largeArray is out of scope, garbage collected
  // Memory freed automatically
}

const result = processLargeData();
console.log(result.length); // 1000000

// Pattern 2: Preventing memory leaks
class EventEmitter {
  constructor() {
    this.listeners = [];
  }
  
  on(event, callback) {
    this.listeners.push({ event, callback }); // Stores reference
  }
  
  off(event, callback) {
    this.listeners = this.listeners.filter(
      l => !(l.event === event && l.callback === callback)
    ); // Removes reference, allows GC
  }
}

const emitter = new EventEmitter();
const handler = () => console.log("event!");

emitter.on("click", handler);
// ... later when done:
emitter.off("click", handler); // Clean up, prevent memory leak

// Pattern 3: Closure memory leak
function createFunctions() {
  const largeData = new Array(1000000).fill("data"); // 4MB
  
  return {
    // ❌ Both functions have closure over largeData
    func1: () => largeData[0],
    func2: () => largeData[1],
    // largeData never freed while functions exist!
  };
}

// ✅ Better: Only store what you need
function createFunctions() {
  const largeData = new Array(1000000).fill("data");
  const first = largeData[0];
  const second = largeData[1];
  
  return {
    func1: () => first,
    func2: () => second,
    // largeData can be GC'd after function returns
  };
}
```

#### Call Stack in DevTools

```javascript
// ✅ Using DevTools to see call stack

function level3() {
  debugger; // Opens DevTools here
  // Call stack shows:
  // level3
  // level2
  // level1
  // global
}

function level2() {
  level3();
}

function level1() {
  level2();
}

level1();

// In DevTools Debugger:
// When paused at debugger, you see call stack:
// ┌─────────────────┐
// │ level3          │ <- Current function
// ├─────────────────┤
// │ level2          │ <- Called level3
// ├─────────────────┤
// │ level1          │ <- Called level2
// ├─────────────────┤
// │ (anonymous)     │ <- Global/main script
// └─────────────────┘

// Also shows variable environment for each context
// Can inspect local variables at each level
```

### Real-World Applications

#### Google Chrome DevTools - Profiling Memory

Google engineers use memory profiling to prevent leaks:

```javascript
// Real-world scenario: Memory leak in SPA

class PageManager {
  constructor() {
    this.users = []; // Will grow forever!
  }
  
  loadUsers() {
    // Each page load adds more users to array
    fetch("/api/users").then(data => {
      this.users.push(...data); // ❌ Accumulates forever
    });
  }
}

// ✅ Fixed version
class PageManager {
  constructor() {
    this.users = []; // Reset on new page
  }
  
  loadUsers() {
    this.users = []; // Clear old data
    fetch("/api/users").then(data => {
      this.users = data; // Replace, not accumulate
    });
  }
  
  destroy() {
    this.users = []; // Clean up when page closes
  }
}
```

#### Facebook React - Execution Context for Components

React components create execution contexts:

```javascript
// React component execution contexts

function UserProfile({ userId }) {
  // New execution context created for each render
  // Has: userId prop, state, hooks
  
  const [user, setUser] = React.useState(null);
  
  React.useEffect(() => {
    // New execution context for effect
    // Has closure over userId, user, setUser
    
    fetch(`/api/user/${userId}`).then(data => {
      setUser(data);
      // When setUser called, new render triggers
      // New execution context created with updated state
    });
  }, [userId]);
  
  return <div>{user?.name}</div>;
  // Each render = new execution context
  // Previous context cleaned up, GC frees memory
}
```

#### Netflix - Streaming Performance

Netflix optimizes execution context to prevent buffering:

```javascript
// Large video processing - optimize execution contexts

function processVideoFrame() {
  // ❌ Creating huge context
  const frame = new Uint8Array(1920 * 1080 * 3); // 6MB
  const processed = frame.map(pixel => pixel * 2); // Expensive
  
  // Function returns, memory freed
  return processed;
  // But if we call this 60 times/sec = memory thrashing
}

// ✅ Better: Reuse buffers, limit context size
class VideoProcessor {
  constructor() {
    this.frameBuffer = new Uint8Array(1920 * 1080 * 3);
  }
  
  processFrame(rawData) {
    // Reuse buffer, small execution context
    for (let i = 0; i < rawData.length; i++) {
      this.frameBuffer[i] = rawData[i] * 2;
    }
    return this.frameBuffer;
  }
}

// Memory stable, no thrashing, smooth playback
```

### Common Gotchas (The 90% Fail Here)

**Gotcha #1: Misunderstanding `var` Hoisting**

```javascript
// ❌ Common mistake
function test() {
  console.log(x); // Expected error, but prints undefined!
  var x = 5;
}

// Why? var is hoisted, but assignment isn't
// JavaScript sees it as:
// var x;
// console.log(x); // undefined
// x = 5;

// ✅ Use let/const instead (hoisted but in TDZ)
function test() {
  console.log(y); // ReferenceError! Caught early
  let y = 5;
}
```

**Gotcha #2: `this` Binding Surprises**

```javascript
// ❌ Lost context with callbacks
const obj = {
  value: 42,
  getValue: function() {
    return this.value;
  }
};

const getValue = obj.getValue;
console.log(getValue()); // undefined (this is window or undefined)

// ✅ Bind explicitly
const boundGetValue = obj.getValue.bind(obj);
console.log(boundGetValue()); // 42

// ✅ Or use arrow function (preserves this)
const obj2 = {
  value: 42,
  getValue: () => {
    return this.value; // this from outer scope
  }
};
```

**Gotcha #3: Closure Memory Leaks**

```javascript
// ❌ Closure holds onto large data forever
function createHandlers() {
  const largeData = new Array(1000000).fill("x"); // 4MB
  
  return [
    () => largeData[0],
    () => largeData[1],
    // Even if you use first two, all 3 functions keep largeData in memory
    // largeData never garbage collected
  ];
}

// ✅ Extract only what you need
function createHandlers() {
  const largeData = new Array(1000000).fill("x");
  const first = largeData[0];
  const second = largeData[1];
  
  return [
    () => first,
    () => second,
    // largeData can be collected after function returns
  ];
}
```

**Gotcha #4: Unintended Global Variables**

```javascript
// ❌ Forgot var/let/const - creates global!
function process() {
  result = "I'm global now!"; // Missing var/let/const
}

process();
console.log(window.result); // "I'm global now!" - pollution!

// ✅ Always use var/let/const
function process() {
  let result = "I'm local"; // Proper scope
}

// ✅ Or use strict mode (catches this)
"use strict";
function process() {
  result = "I'm global now!"; // ReferenceError! Caught!
}
```

**Gotcha #5: Call Stack Overflow**

```javascript
// ❌ Stack overflow with recursion
function recursive() {
  recursive(); // No base case!
  // Call stack: [recursive, recursive, recursive, ...]
  // Eventually: Maximum call stack size exceeded
}

// ✅ Always have base case
function recursive(n) {
  if (n <= 0) return; // BASE CASE!
  recursive(n - 1);
}
```

### Advanced Patterns

#### Context in Async Code

```javascript
// ✅ Execution contexts with async/await

async function fetchUser(userId) {
  console.log("Context 1: fetching user");
  
  // await pauses execution context
  const user = await fetch(`/api/user/${userId}`);
  
  console.log("Context 2: user fetched (different context)");
  // This executes in a DIFFERENT context
  // Variables are preserved through async operations
}

// ✅ Understanding async context chains
function chainAsync() {
  const data = {};
  
  Promise.resolve(1)
    .then(value => {
      // New context, but has closure over 'data'
      data.first = value;
    })
    .then(() => {
      // Another context, still has closure over 'data'
      console.log(data.first); // 1
    });
}

chainAsync();
```

#### Context in Event Handlers

```javascript
// ✅ Event handler execution contexts

button.addEventListener("click", function(event) {
  console.log(this); // 'this' is the button element
  console.log(this === button); // true
  
  // New execution context created for each click
  let clickCount = 0; // Local to this click
  clickCount++;
  
  // Context destroyed after handler finishes
});

// Arrow function doesn't bind 'this'
button.addEventListener("click", (event) => {
  console.log(this); // 'this' from outer scope, not button
});
```

#### Memory Profiling Pattern

```javascript
// ✅ Checking memory usage in DevTools

// In browser console:
performance.memory.usedJSHeapSize / 1048576 // MB

// Sample memory tracking
function trackMemory() {
  const before = performance.memory.usedJSHeapSize;
  
  // Do something memory-intensive
  const largeArray = new Array(1000000).fill(0);
  
  const after = performance.memory.usedJSHeapSize;
  const diff = (after - before) / 1048576;
  
  console.log(`Memory used: ${diff.toFixed(2)} MB`);
  
  // Clean up
  largeArray = null;
  // (optional force GC with DevTools)
}

trackMemory();
```

---

## 🎬 PART 2: Explain Like I'm 5

### The Story Approach

Imagine your brain 🧠 is like a kitchen 🍳:

**Execution Context** is like the "recipe you're currently following."

When you start cooking, you create a mental context: "I'm making cookies." You remember the ingredients, the steps, what bowl you're using, etc. That's your current recipe (execution context).

If you start making pasta while making cookies, you create a NEW context: "I'm making pasta." Two separate recipes in your mind. When you finish pasta, you go back to the cookie recipe.

**Memory** is like your kitchen's storage:

- **Call Stack** is like your kitchen counter. You put things you're currently using on the counter. When you're done with a bowl, you put it away.
- **Heap** is like your pantry. You store big things (ingredients, tools) there. You keep references to them so you know where to find them.

**Garbage Collection** is like cleaning:

When you're done with a recipe, you clean up and throw away things you don't need anymore. If nobody is using something, you can throw it away.

### How It Works

**Step 1: Global Context**
You wake up in the morning. You have a general "morning routine" (global context) with things you always do: brush teeth, eat breakfast, etc.

**Step 2: Function Contexts**
You start a specific task like "make breakfast" (function context). This has its own mini-environment: pan, eggs, milk. Just for breakfast.

**Step 3: Nested Functions**
While making breakfast, you do "make eggs" (nested function). This has its own micro-environment: eggs, heat, spatula.

**Step 4: Memory**
You remember where things are (memory). You know eggs are in the fridge (reference), not in your pocket.

**Step 5: Cleanup**
When breakfast is done, you clean up. You don't need the pan anymore (garbage collection). You throw it back in the cupboard.

### Real Example

Think about making a phone call 📱:

1. **Global Context**: "I'm awake, I have a phone, I know how to call people"
2. **Function Context**: "I'm calling my friend Alice"
   - Where is her number? (variable)
   - What will I say? (variable)
   - Is she home? (variable)
3. **Nested Function Context**: "I'm dialing her number"
   - Each digit goes into the phone
4. **Memory**: The phone number stays in your memory (RAM)
5. **Garbage Collection**: After you hang up, the call context is gone. You forget exactly what you said (memory freed).

### Why This Matters

- **You can do multiple things**: Each task has its own context (kitchen counter has room for cookies AND pasta)
- **Things don't get mixed up**: Pasta recipe doesn't affect cookie recipe (separate contexts)
- **You don't remember everything forever**: After you're done, you clean up (garbage collection frees memory)
- **You know where things are**: References help you find what you need (pointers in memory)

---

## 🎯 PART 3: INTERVIEW QUESTIONS

### Level 1: EASY (Q1-Q3)

**Q1: "What is an execution context and why do we care about it?"**

**What interviewer is testing:**
- Basic understanding of how JavaScript executes code
- Understanding of scoping and variable access
- Ability to explain abstract concepts clearly

**Perfect Answer:**

"An execution context is the **environment where JavaScript code runs**. It's like the 'stage' for your code to perform on. It includes:

1. **Variables and values** available at that moment
2. **The `this` keyword** and what it refers to
3. **The scope chain** - which outer variables it can access

Why we care:

1. **Debugging**: When a variable is undefined or has wrong value, understanding context helps explain why
2. **Scope issues**: Understanding context explains why a variable is accessible in one place but not another
3. **Memory management**: Understanding contexts helps us write code that doesn't leak memory
4. **`this` binding**: Knowing how context works explains why `this` sometimes surprises us

**Example:**

```javascript
function greet(name) {
  // New execution context created when greet() is called
  // Variables: name parameter
  // this: depends on how greet() was called
  console.log(Hello, \${name}\`);
}

greet('Alice'); // Execution context with name='Alice'
greet('Bob');   // Different execution context with name='Bob'
```

Every time you call a function, a new execution context is created. When the function returns, that context is destroyed. This is essential to how JavaScript works."

**Common mistakes:**
❌ Confusing execution context with scope
❌ Not understanding multiple contexts can exist simultaneously
❌ Thinking execution context is only about variables
✅ Understanding it's the complete environment (variables, this, scope chain)

**Follow-up questions:**
- "What's in an execution context?"
- "What's the difference between global context and function context?"
- "How does the call stack relate to execution contexts?"

**Score:** 9/10 because explains what it is and why it matters.

---

**Q2: "Explain the difference between the call stack and the memory heap."**

**What interviewer is testing:**
- Understanding of memory management in JavaScript
- Knowledge of primitives vs objects
- Ability to explain complex concepts simply

**Perfect Answer:**

"The **call stack** and **memory heap** are two different places where JavaScript stores data.

**Call Stack:**
- Stores **primitive values** (numbers, strings, booleans)
- Stores **references** to objects
- Automatically organized - LIFO (Last In First Out)
- Cleaned up automatically when functions return
- Limited size - if you overflow it, you get stack overflow error

```javascript
let age = 30;  // Stored on stack
```

**Memory Heap:**
- Stores **objects and arrays** (complex data)
- Large, unorganized memory area
- Accessed via references from the stack
- Cleaned up by garbage collector when no references exist
- Much larger than stack

```javascript
let user = { name: 'Alice' }; // Reference on stack, object on heap
```

**Analogy:**
- Stack = cashier's counter (small, organized, fast access)
- Heap = warehouse (large, disorganized, slower access)

**Why it matters:**

```javascript
let a = 5;           // Primitive on stack
let b = a;           // Copy the value
b = 10;              // a is still 5

let obj1 = { x: 5 }; // Reference on stack, object on heap
let obj2 = obj1;     // Copy the reference (point to same object!)
obj2.x = 10;         // obj1.x is also 10 now!
```

This is why modifying objects affects multiple references, but primitives don't."

**Common mistakes:**
❌ Thinking objects are stored on the stack
❌ Not understanding references point to heap
❌ Confusing where different data types live
✅ Knowing primitives on stack, objects on heap

**Follow-up questions:**
- "What happens to memory when a function returns?"
- "Why does assigning one object to another create a reference?"
- "How does garbage collection clean up the heap?"

**Score:** 9/10 because clear distinction and good examples.

---

**Q3: "What is hoisting and how does it work with `var`, `let`, and `const`?"**

**What interviewer is testing:**
- Understanding of JavaScript's variable lifecycle
- Knowledge of TDZ (Temporal Dead Zone)
- Understanding of best practices

**Perfect Answer:**

"**Hoisting** is JavaScript moving variable and function declarations to the top of their scope during the creation phase.

**How it works:**

**Functions:**
```javascript
console.log(add(2, 3)); // Works! Returns 5
// Function declarations are fully hoisted

function add(a, b) {
  return a + b;
}
```

**var:**
```javascript
console.log(x); // undefined (not an error!)
var x = 5;

// JavaScript sees this as:
// var x;           <- Declaration hoisted, x = undefined
// console.log(x);  <- x is undefined here
// x = 5;           <- Assignment NOT hoisted
// 
// Variables declared with var are initialized as 'undefined'
```

**let and const:**
```javascript
console.log(y); // ReferenceError! Cannot access before initialization
let y = 5;

// let/const are hoisted but in "Temporal Dead Zone"
// They exist but are inaccessible until declaration
// This prevents accidental undefined errors
```

**Why it matters:**

1. **var is unpredictable** - you can access before declaration, get undefined
2. **let/const are safer** - you get an error if accessing before declaration
3. **Functions hoist fully** - you can call functions before defining them
4. **Best practice**: Use `const` by default (prevents accidental reassignment)

**Practical example:**

```javascript
// ❌ Confusing with var
if (true) {
  console.log(x); // undefined (confusing!)
  var x = 5;
}

// ✅ Clear with let
if (true) {
  console.log(y); // ReferenceError (clear that something's wrong)
  let y = 5;
}
```

**Why const/let exist:** To prevent the confusing undefined errors that var causes."

**Common mistakes:**
❌ Thinking hoisting moves code to the top (it doesn't)
❌ Thinking let/const aren't hoisted (they are, in TDZ)
❌ Using var without understanding hoisting
✅ Using let/const and understanding TDZ

**Follow-up questions:**
- "What's the Temporal Dead Zone?"
- "Why is var considered bad practice?"
- "Can you have hoisting inside block scope?"

**Score:** 9/10 because explains all three, includes TDZ, and gives reasons.

---

### Level 2: MEDIUM (Q4-Q6)

**Q4: "Explain what happens when you call a function multiple times. How many execution contexts are created?"**

**What interviewer is testing:**
- Understanding that each function call creates a new context
- Understanding of call stack behavior
- Ability to trace execution mentally

**Perfect Answer:**

"Each function call creates a **new, separate execution context**. So calling a function multiple times creates multiple contexts.

**Example:**

```javascript
function add(a, b) {
  let sum = a + b;
  console.log(sum);
  return sum;
}

add(2, 3); // Call 1: New context with a=2, b=3, sum=5
add(5, 7); // Call 2: New context with a=5, b=7, sum=12
add(1, 1); // Call 3: New context with a=1, b=1, sum=2
```

**What happens:**

1. **Call 1:** New execution context created
   - Variables: a=2, b=3, sum=5
   - Code executes
   - Function returns, context destroyed

2. **Call 2:** Completely new execution context created
   - Variables: a=5, b=7, sum=12
   - Code executes
   - Function returns, context destroyed

3. **Call 3:** Another new execution context
   - Variables: a=1, b=1, sum=2
   - Code executes
   - Function returns, context destroyed

**Key point:** Each context is independent. Variables in one call don't affect other calls.

**Call Stack visualization:**

```
Call 1:
[add(2, 3)]

Call 2 (call 1 finished):
[add(5, 7)]

Call 3 (call 2 finished):
[add(1, 1)]
```

**Nested calls show stack depth:**

```javascript
function a() { b(); }
function b() { c(); }
function c() { console.log('deepest'); }

a();

// At deepest point, call stack is:
// [a, b, c] <- 3 contexts

// As they return:
// [a, b]    <- c done
// [a]       <- b done
// []        <- a done
```

**Why it matters:**

- Each call has fresh variables
- Stack overflow happens when too many contexts exist
- Recursion creates contexts until base case
- Understanding this helps debug why two calls might have different behavior"

**Common mistakes:**
❌ Thinking multiple calls reuse same context
❌ Not understanding call stack depth
❌ Confusing local variables across calls
✅ Knowing each call = fresh context

**Follow-up questions:**
- "What happens if a function calls itself?"
- "How deep can the call stack go?"
- "What causes a stack overflow?"

**Score:** 10/10 because explains each call, shows stack visualization, and explains why it matters.

---

**Q5: "Why does this code log `undefined` instead of causing an error, and how would you fix it?"**

```javascript
function test() {
  console.log(x);
  var x = 5;
}

test();
```

**What interviewer is testing:**
- Understanding of hoisting
- Understanding of var vs let/const
- Ability to identify and fix problems

**Perfect Answer:**

"This logs `undefined` because of **var hoisting**. Here's why:

**What JavaScript does (hoisting):**

JavaScript restructures the code before executing:

```javascript
function test() {
  var x;              // Declaration hoisted, x = undefined
  console.log(x);     // x is undefined here
  x = 5;              // Assignment NOT hoisted
}

test(); // Output: undefined
```

**Why it happens:**
During the creation phase of execution context, JavaScript:
1. Finds all `var` declarations
2. Creates them with value `undefined`
3. Doesn't hoist the assignment

**Why it's confusing:**
```javascript
// Expected: ReferenceError (x doesn't exist)
// Actual: undefined (x exists but is uninitialized)
console.log(x);
var x = 5;
```

**How to fix it (3 options):**

**Option 1: Use `let` (BEST - catches errors)**
```javascript
function test() {
  console.log(x); // ReferenceError! Clear that something's wrong
  let x = 5;
}
```

With `let`, you get Temporal Dead Zone - x is hoisted but inaccessible, so you get an error. This is safer because it catches mistakes.

**Option 2: Declare before use**
```javascript
function test() {
  var x = 5; // Declare first
  console.log(x); // 5
}
```

**Option 3: Use strict mode (catches the problem)**
```javascript
'use strict';
// Strict mode doesn't prevent hoisting but makes undefined behavior more predictable
```

**Best practice:**
Use `const` by default, `let` if you need to reassign:

```javascript
function test() {
  const x = 5;
  console.log(x); // 5, clear, no hoisting confusion
}
```

**Why `let/const` are better:**
- Temporal Dead Zone catches errors early
- Block scoping is more predictable
- You can't accidentally access before declaration
- No hoisting confusion"

**Common mistakes:**
❌ Saying this is a bug in JavaScript
❌ Not understanding hoisting
❌ Not knowing how to fix it
✅ Understanding hoisting, explaining fix, and preferring let/const

**Follow-up questions:**
- "What's the Temporal Dead Zone?"
- "Does this happen with `let`?"
- "Why did JavaScript design hoisting this way?"

**Score:** 10/10 because explains hoisting, shows the problem, and gives clear fixes.

---

**Q6: "Explain what happens to memory when this function is called, and why the object might not be garbage collected."**

```javascript
let globalObj = null;

function createUser() {
  const user = { name: "Alice", data: new Array(1000000) };
  globalObj = user;
  return user;
}

const myUser = createUser();
```

**What interviewer is testing:**
- Understanding of memory management
- Understanding of garbage collection
- Understanding of reference counting
- Ability to identify memory issues

**Perfect Answer:**

"This creates a memory issue because the object has **two references**, so it's never garbage collected.

**What happens:**

1. **createUser() is called**
   - New execution context created
   - Creates user object in heap
   - Object has: name property and 1 million element array (4MB memory)

2. **globalObj = user**
   - globalObj (global scope) now references the object
   - Object has 1 reference (from globalObj)

3. **return user**
   - myUser (function scope) now references the object
   - Object has 2 references (globalObj and myUser)

4. **Function returns, local context destroyed**
   - Local user variable is gone
   - But object stays in memory because:
     - globalObj still references it (global scope)
     - myUser still references it (function scope)

5. **Memory is never freed**

```
Memory lifecycle:

Creation:
  myUser → [heap #1000] ← globalObj
           { name: "Alice", data: [1M items] }
           References: 2

Later:
  myUser = null;  // myUser stops referencing
                  // References: 1 (still has globalObj)

  globalObj = null; // globalObj stops referencing
                    // References: 0
                    // NOW garbage collector can free memory

// Without clearing both references, memory stays allocated forever
```

**Why it's a problem:**

- **Global pollution**: globalObj exists forever
- **Memory leak**: The 4MB object stays in memory even when not needed
- **App bloat**: If this happens repeatedly, app uses more and more memory
- **Eventual crash**: Eventually out of RAM, app crashes

**How to fix it:**

**Option 1: Avoid global references**
```javascript
function createUser() {
  const user = { name: "Alice", data: new Array(1000000) };
  return user; // Only return, don't store globally
}

const myUser = createUser();
// myUser has 1 reference, no problem
```

**Option 2: Clean up when done**
```javascript
let globalObj = null;

function createUser() {
  const user = { name: "Alice", data: new Array(1000000) };
  globalObj = user;
  return user;
}

const myUser = createUser();

// Later, when done:
myUser = null;      // Remove reference
globalObj = null;   // Remove global reference
// Now object has 0 references, garbage collector frees memory
```

**Option 3: Use WeakMap (won't prevent garbage collection)**
```javascript
const users = new WeakMap();

function createUser(key) {
  const user = { name: "Alice" };
  users.set(key, user);
  return user;
  // If user is no longer referenced elsewhere, it can be GC'd
  // WeakMap won't keep it alive
}
```

**Key principle:** 
Objects are garbage collected only when they have **zero references**. Having references in:
- Local variables
- Global variables  
- Closures
- Event listeners

...all keep objects alive."

**Common mistakes:**
❌ Not realizing global references prevent garbage collection
❌ Thinking local variables prevent GC (they don't once function returns)
❌ Not knowing to clean up global references
✅ Understanding reference counting, identifying leaks, and knowing fixes

**Follow-up questions:**
- "What are event listener memory leaks?"
- "How do you detect memory leaks?"
- "What's WeakMap and when would you use it?"

**Score:** 10/10 because explains memory lifecycle, identifies the issue, and provides multiple fixes.

---

### Level 3: HARD (Q7-Q10)

**Q7: "Design a function that demonstrates understanding of execution context, scope chain, and closures. Explain what happens at each step."**

**What interviewer is testing:**
- Deep understanding of execution contexts
- Understanding of scope chain and lexical environment
- Ability to trace execution through multiple contexts
- Understanding of closures and memory

**Perfect Answer:**

"Let me build a function that demonstrates all these concepts:

```javascript
function outer(x) {
  // Execution Context 1: outer
  // Variable Environment: x, innerFunc, result
  // Lexical parent: global scope
  
  let name = "Outer";
  
  function inner(y) {
    // Execution Context 2: inner (created when called)
    // Variable Environment: y, z
    // Lexical parent: outer's scope
    // Scope chain: [inner] → [outer] → [global]
    
    let z = x + y; // Can access x from outer scope!
    
    return function innermost(w) {
      // Execution Context 3: innermost (created when called)
      // Variable Environment: w
      // Lexical parent: inner's scope
      // Scope chain: [innermost] → [inner] → [outer] → [global]
      
      // Can access: w (local), z (from inner), y (from inner), x (from outer), name (from outer)
      console.log(\`w=\${w}, z=\${z}, y=\${y}, x=\${x}, name=\${name}\`);
      
      return x + y + z + w;
    };
  }
  
  let result = inner(20);
  return result;
}

const closure = outer(10);
// outer returns a function (innermost) with closure over:
// - x = 10 (from outer)
// - y = 20 (from inner)
// - z = 30 (from inner)
// - name = "Outer" (from outer)

closure(5); // Calls innermost with w=5
// Output: w=5, z=30, y=20, x=10, name=Outer
// Returns: 10 + 20 + 30 + 5 = 65
```

**Step-by-step execution:**

**Step 1: outer(10) is called**
```
Call stack: [outer]

outer execution context:
├─ Variable Environment:
│  ├─ x = 10
│  ├─ name = "Outer"
│  ├─ innerFunc = function definition
│  └─ result = undefined
└─ Lexical Environment:
   └─ parent: global
```

**Step 2: inner(20) is called**
```
Call stack: [outer, inner]

inner execution context:
├─ Variable Environment:
│  ├─ y = 20
│  └─ z = 30 (computed as 10 + 20)
└─ Lexical Environment:
   ├─ parent: outer's scope
   └─ Can access x, name from outer
```

**Step 3: innermost function returned**
```
Call stack: back to [outer] (inner finished)

innermost function is returned with CLOSURE:
├─ Has references to:
│  ├─ x = 10 (from outer's context)
│  ├─ y = 20 (from inner's context)
│  ├─ z = 30 (from inner's context)
│  └─ name = "Outer" (from outer's context)
└─ These variables stay in memory because:
   └─ innermost still references them
```

**Step 4: outer returns**
```
Call stack: [] (outer finished)

But memory NOT freed:
├─ outer's execution context destroyed
├─ But x, name NOT garbage collected
├─ Why? innermost has closure over them
├─ They stay in memory as long as innermost exists
└─ This is a CLOSURE
```

**Step 5: closure(5) is called**
```
Call stack: [closure (which is innermost)]

innermost execution context:
├─ Variable Environment:
│  └─ w = 5
├─ Scope Chain:
│  ├─ [innermost] has: w
│  ├─ [inner] has: y, z (in closure)
│  ├─ [outer] has: x, name (in closure)
│  └─ [global] has: everything else
└─ Can access all variables through scope chain
```

**Memory visualization:**

```
After outer() and inner() return, but before closure() is called:

┌─────────────────────────────────────────┐
│ HEAP                                    │
├─────────────────────────────────────────┤
│ innermost function (closure):           │
│ ├─ References: x=10, y=20, z=30, name  │
│ └─ These variables kept alive by ref   │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│ CALL STACK                              │
├─────────────────────────────────────────┤
│ global context (empty)                  │
└─────────────────────────────────────────┘

closure still references x, y, z, name
→ They stay in memory (closure)
→ Not garbage collected
```

**Key concepts demonstrated:**

1. **Multiple execution contexts**: outer, inner, innermost each have their own
2. **Scope chain**: innermost can access variables from all parent scopes
3. **Lexical environment**: Scope is determined by where functions are defined, not where called
4. **Closure**: innermost closes over (captures) variables from parent scopes
5. **Memory preservation**: Variables live longer than their execution contexts due to closure

**Practical implications:**

```javascript
const closure1 = outer(10);
const closure2 = outer(20);

// closure1 remembers x=10, y=20
// closure2 remembers x=20, y=40
// Both exist simultaneously in memory
// Each with its own captured variables

closure1(5); // Returns 10+20+30+5 = 65
closure2(5); // Returns 20+40+60+5 = 125

// Memory used for closure1:
// - x, y, z, name from outer(10) and inner(20)
// - Stays as long as closure1 exists

// When done with closures:
closure1 = null; // Allows outer(10) variables to be GC'd
closure2 = null; // Allows outer(20) variables to be GC'd
```

**This is why closures are powerful:**
- They remember their creation context
- Perfect for data privacy (encapsulation)
- Perfect for function factories
- But can cause memory leaks if not managed"

**Common mistakes:**
❌ Not understanding closures persist after function returns
❌ Confusing scope chain with call stack
❌ Not realizing closures keep variables in memory
✅ Understanding multiple contexts, scope chain, and closure memory implications

**Follow-up questions:**
- "What if we called outer() 1000 times? How much memory?"
- "How would you prevent memory leaks with these closures?"
- "What's the difference between scope chain and call stack?"

**Score:** 10/10 because demonstrates mastery of all concepts with clear examples.

---

**Q8: "Explain how garbage collection works and when memory is freed. What's a memory leak and how do you find it?"**

**What interviewer is testing:**
- Understanding of garbage collection algorithms
- Understanding of memory leaks
- Practical debugging skills
- Production-aware thinking

**Perfect Answer:**

"**How Garbage Collection Works:**

Modern JavaScript uses **Mark & Sweep** algorithm:

**1. Mark Phase:**
```javascript
// Start with all reachable objects
function process() {
  let obj = { data: "important" }; // Mark as reachable
  return obj;
}

const result = process();
// obj is reachable from result variable

// Garbage collector:
// 1. Start from root (global, call stack)
// 2. Follow all references
// 3. Mark everything reachable
// 4. Mark everything else as garbage
```

**2. Sweep Phase:**
```javascript
// Collect unmarked objects

result = null; // obj is no longer reachable
               // Mark as garbage

// Garbage collector:
// 1. Find all unmarked objects
// 2. Free their memory
// 3. Compact heap (optional)

// Memory freed and available for reuse
```

**Visual example:**

```
Before GC:
┌─────────────────────────┐
│ Reachable objects       │ ✓
├─────────────────────────┤
│ {name: "Alice"}         │ ✓ (referenced by variable user)
│ {name: "Bob"}           │ ✗ (no references)
│ [1,2,3]                 │ ✓ (referenced by array)
│ {x: 10}                 │ ✗ (no references)
└─────────────────────────┘

After GC:
┌─────────────────────────┐
│ Remaining objects       │
├─────────────────────────┤
│ {name: "Alice"}         │ ✓
│ [1,2,3]                 │ ✓
└─────────────────────────┘
(Bob and {x: 10} freed)
```

**When memory is freed:**

```javascript
// Memory freed when object has ZERO references

let user = { name: "Alice" }; // Reference count: 1
let ref1 = user;              // Reference count: 2

user = null;                  // Reference count: 1
ref1 = null;                  // Reference count: 0
                              // Object garbage collected!

// Memory freed!
```

**Memory Leaks (Common Causes):**

**1. Global variables accumulating**
```javascript
// ❌ Memory leak
function process() {
  window.data = new Array(1000000); // Stores in global forever
}

process();
// data stays in memory forever, never garbage collected

// ✅ Fix: Use local scope
function process() {
  const data = new Array(1000000); // Local scope
}
// data freed after function returns
```

**2. Event listeners not removed**
```javascript
// ❌ Memory leak
button.addEventListener("click", () => {
  // This function holds references to large objects
  const largeData = new Array(1000000);
  console.log(largeData);
});

// If button is removed from DOM, listener still in memory!

// ✅ Fix: Remove listener when done
const handler = () => {
  const largeData = new Array(1000000);
};

button.addEventListener("click", handler);
// Later, when done:
button.removeEventListener("click", handler);
```

**3. Closures holding references**
```javascript
// ❌ Memory leak
function createLoop() {
  const hugeArray = new Array(1000000); // 4MB
  
  return [
    () => hugeArray[0],
    () => hugeArray[1],
    () => hugeArray[2],
  ];
  // Each function closes over hugeArray
  // hugeArray never freed while functions exist
}

// ✅ Fix: Only store needed values
function createLoop() {
  const hugeArray = new Array(1000000);
  const [first, second, third] = [hugeArray[0], hugeArray[1], hugeArray[2]];
  
  return [
    () => first,
    () => second,
    () => third,
  ];
  // hugeArray can be freed
}
```

**4. Circular references (less common in modern JS)**
```javascript
// ❌ Potential memory leak (older engines)
function createCycle() {
  const obj1 = {};
  const obj2 = {};
  
  obj1.ref = obj2;
  obj2.ref = obj1; // Circular reference
  
  return [obj1, obj2];
}

// Modern GC handles circular references, but still a problem
// if nothing points to either obj1 or obj2

// ✅ Remove circular references when done
obj1.ref = null;
obj2.ref = null;
```

**How to Find Memory Leaks:**

**1. Chrome DevTools Memory Profiler**

```javascript
// In DevTools Profiler:
// 1. Take heap snapshot (baseline)
// 2. Do action multiple times
// 3. Take another heap snapshot
// 4. Compare: is memory growing?

function leakyFunction() {
  window.cache = new Array(1000000);
}

// Steps to debug:
// 1. DevTools → Memory tab
// 2. Click "Take heap snapshot"
// 3. Call leakyFunction() 10 times
// 4. Take another snapshot
// 5. Look for growth in detached DOM nodes or arrays
```

**2. Chrome Timeline (Performance tab)**

```javascript
// Record memory usage over time
// If line goes up and never comes down = memory leak

const results = [];

function accumulate() {
  results.push(new Array(1000000)); // Accumulates forever
}

// In Performance tab:
// 1. Start recording
// 2. Call accumulate() 10 times
// 3. Stop recording
// 4. Memory line goes up = leak detected
```

**3. Node.js memory profiling**

```javascript
// Check memory in Node.js
const used = process.memoryUsage();

console.log(`
  Heap total: ${Math.round(used.heapTotal / 1024 / 1024)} MB
  Heap used: ${Math.round(used.heapUsed / 1024 / 1024)} MB
  External: ${Math.round(used.external / 1024 / 1024)} MB
`);

// Monitor over time:
setInterval(() => {
  const used = process.memoryUsage();
  const heapUsed = Math.round(used.heapUsed / 1024 / 1024);
  console.log(`Heap: ${heapUsed} MB`);
}, 1000);

// If growing infinitely = memory leak
```

**Real-world example (Netflix):**

```javascript
// ❌ Real leak in streaming app
class VideoPlayer {
  constructor() {
    this.frameBuffers = []; // Accumulates frames
  }
  
  playVideo(frameData) {
    for (let frame of frameData) {
      // Forgot to clear old frames!
      this.frameBuffers.push(frame);
    }
  }
}

// After playing 10 videos = 1000 frames in memory
// Player slows down, eventually crashes

// ✅ Fixed
class VideoPlayer {
  constructor() {
    this.frameBuffer = null; // Reuse buffer
    this.maxBufferSize = 100;
  }
  
  playVideo(frameData) {
    for (let frame of frameData) {
      this.frameBuffer = frame; // Reuse, don't accumulate
      process(this.frameBuffer);
    }
  }
}
```

**Key takeaway:**
- Objects freed when: **zero references in reachable scope**
- Leaks happen when: **forgot to remove references**
- Find leaks: **DevTools Memory tab, heap snapshots**
- Fix leaks: **remove global refs, clean up listeners, limit closures**"

**Common mistakes:**
❌ Not knowing how GC works
❌ Thinking circular references always leak (modern JS handles them)
❌ Not knowing how to find leaks
❌ Forgetting to remove event listeners
✅ Understanding mark & sweep, identifying leak patterns, knowing DevTools

**Follow-up questions:**
- "How do you handle memory leaks in React?"
- "What's the difference between weak references and regular references?"
- "How does garbage collection affect performance?"

**Score:** 10/10 because comprehensive, includes algorithms, patterns, debugging, and real examples.

---

**Q9: "Create a complex example demonstrating understanding of execution contexts, the call stack, and memory management. Include async operations."**

**What interviewer is testing:**
- Advanced understanding of execution contexts across async operations
- Understanding of event loop and task queue
- Memory management with async code
- Practical, complex scenario handling

**Perfect Answer:**

"Let me build a realistic example showing all these concepts:

```javascript
// Complex example: Async data processing with memory management

class DataProcessor {
  constructor() {
    this.cache = new Map();      // Reference in heap
    this.processing = false;      // Primitive on stack
  }
  
  async fetchAndProcess(userId) {
    // EC1: fetchAndProcess execution context created
    
    // Check cache first (understand references)
    if (this.cache.has(userId)) {
      return this.cache.get(userId); // Return cached reference
    }
    
    this.processing = true;
    
    try {
      // await pauses execution, returns to event loop
      const user = await this.fetchUser(userId);
      // EC1 resumes with user data
      
      // Processing function creates new context
      const processed = await this.processData(user);
      // EC1 resumes with processed data
      
      // Store in cache (memory on heap)
      this.cache.set(userId, processed);
      
      return processed;
      
    } catch (error) {
      console.error('Error processing:', error);
      // Clean up on error (important for GC)
      this.cache.delete(userId);
      throw error;
      
    } finally {
      this.processing = false; // Cleanup
    }
  }
  
  fetchUser(userId) {
    // EC2: fetchUser context created
    // Returns promise immediately (creates microtask)
    
    return new Promise((resolve, reject) => {
      // resolve and reject are in closure
      // They keep EC2 variables alive
      
      setTimeout(() => {
        // EC3: setTimeout callback context (much later)
        // Has closure over userId, resolve
        
        const userData = { userId, name: \`User \${userId}\` };
        // userData object in heap
        
        resolve(userData);
        // EC2 resumes with resolved value
      }, 100);
    });
  }
  
  async processData(user) {
    // EC4: processData context
    
    // Simulate async work
    await new Promise(resolve => {
      // EC5: Promise callback
      setTimeout(() => {
        // EC6: Another async callback
        // Has closure over resolve, user
        resolve();
      }, 50);
    });
    
    // Process user data
    const processed = {
      ...user,
      processed: true,
      timestamp: Date.now()
    };
    
    // Process object in heap
    return processed;
  }
  
  // Clean up when done (prevent memory leaks)
  clear() {
    this.cache.clear(); // Remove all references
    // Objects in heap can now be garbage collected
  }
}

// Usage and execution trace:
const processor = new DataProcessor();

// ========================================
// EXECUTION TRACE
// ========================================

// 1. Call fetchAndProcess(1)
processor.fetchAndProcess(1)
  .then(result => {
    console.log('Result:', result);
    // EC7: then callback context
  });

// 2. At this point:
// Call stack: [main script, event listeners setup]
// Microtasks: [fetchAndProcess.then]
// Timers: [setTimeout 100ms, setTimeout 50ms]
// Memory:
// - EC1 paused (awaiting promise)
// - EC2-6 not yet created
// - processor object in heap with empty cache

// 3. After 50ms (processData setTimeout completes):
// Call stack: [setTimeout callback #2 context]
// Memory:
// - EC6 running
// - EC4 waiting to resume
// - EC1 still paused

// 4. After 100ms (fetchUser setTimeout completes):
// Call stack: [setTimeout callback #1 context]
// Memory:
// - EC3 running
// - EC2 resume
// - User data object created in heap
// - Cache now has entry for userId 1

// 5. All async complete:
// Call stack: empty
// Memory:
// - All ECs destroyed
// - processor.cache has reference to processed data
// - Data stays in memory as long as cache exists

// Demonstrating memory leak potential:
const processor2 = new DataProcessor();

// ❌ Memory leak: Fetch without limit
for (let i = 0; i < 1000; i++) {
  processor2.fetchAndProcess(i);
  // Each fetch caches result
  // Cache grows indefinitely
  // Eventually runs out of memory
}

// ✅ Fixed: Limit cache size
class SmartDataProcessor extends DataProcessor {
  constructor(maxCacheSize = 100) {
    super();
    this.maxCacheSize = maxCacheSize;
  }
  
  async fetchAndProcess(userId) {
    // Keep cache size limited
    if (this.cache.size >= this.maxCacheSize) {
      // Remove oldest entry (simple FIFO)
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
      // Allow GC to collect removed entry
    }
    
    return super.fetchAndProcess(userId);
  }
}
```

**Detailed memory timeline:**

```
TIME EVOLUTION:

T0 (Start):
Stack: [main script]
Heap: processor object created
Microtask queue: empty
Task queue: empty

T10ms:
Stack: [async callback]
Heap: processor object
Microtask queue: [microtask 1]
Task queue: [timer 1, timer 2]

T50ms (First setTimeout completes):
Stack: [setTimeout callback]
Heap: processor, processed data object
Microtask queue: [microtask processing data]
Task queue: [timer 1]
Memory: EC4 running, EC6 created

T100ms (Second setTimeout completes):
Stack: [setTimeout callback]
Heap: processor, user data object, processed data
Microtask queue: [microtask return result]
Task queue: empty
Memory: EC3 running, EC2 resuming, user cached

T150ms (All complete):
Stack: empty
Heap: processor with cache containing user and processed data
Microtask queue: empty
Task queue: empty
Memory: All ECs destroyed, data in cache
```

**Execution Context for Each Phase:**

```
EC1: fetchAndProcess(userId=1)
├─ Created at: processor.fetchAndProcess(1) called
├─ Variables: userId=1, user=undefined, processed=undefined
├─ State: Paused at 'await fetchUser()'
├─ Scope: Has access to 'this.cache'
└─ Cleaned up: After function returns

EC2: fetchUser(userId=1)
├─ Created at: this.fetchUser(userId) called
├─ Returns: Promise immediately (don't wait)
├─ Closure: userId, resolve, reject live in Promise
└─ Resumes: When setTimeout completes

EC3: setTimeout callback (100ms later)
├─ Created at: setTimeout callback executes
├─ Variables: userData object created
├─ Closure: Has access to userId, resolve from EC2
├─ Action: Calls resolve(userData)
└─ Cleaned up: After callback returns

EC4: processData(user)
├─ Created at: await this.processData(user) called
├─ Variables: user object, processed object
├─ Paused at: 'await new Promise'
└─ Resumes: When inner setTimeout completes

EC5 & EC6: Promise callbacks
├─ EC5: Promise constructor (EC2)
├─ EC6: setTimeout inside promise (50ms)
├─ Similar pattern to EC2-EC3
└─ Closure over resolve in EC4
```

**Why this is complex and important:**

1. **Multiple execution contexts**: 6+ contexts created during execution
2. **Asynchronous flow**: Contexts pause, resume, interleave
3. **Closures**: Inner functions close over parent variables
4. **Memory preservation**: Variables live beyond their contexts via closures
5. **Memory leaks**: Cache grows unbounded if not limited
6. **Garbage collection**: Must manually clear references to allow GC

**Practical production lessons:**

```javascript
// Lesson 1: Always clean up
processor.clear(); // Call when done

// Lesson 2: Limit cache growth
if (this.cache.size > 1000) {
  this.cache.clear(); // Reset if too big
}

// Lesson 3: Handle errors (don't leave broken states)
try {
  await processor.fetchAndProcess(userId);
} catch (error) {
  processor.cache.delete(userId); // Remove failed entry
}

// Lesson 4: Monitor memory in production
setInterval(() => {
  const heapUsed = Math.round(process.memoryUsage().heapUsed / 1024 / 1024);
  console.log('Heap:', heapUsed, 'MB');
  
  if (heapUsed > MAX_HEAP_SIZE) {
    // Force cleanup
    processor.clear();
    // Or restart service
  }
}, 5000);
```"

**Common mistakes:**
❌ Not understanding async doesn't destroy contexts immediately
❌ Not realizing closures in promises keep variables alive
❌ Forgetting to clean up caches
❌ Not monitoring memory in production
✅ Understanding all concepts together, including leaks and cleanup

**Follow-up questions:**
- "How would you test this for memory leaks?"
- "What if fetchAndProcess was called 10,000 times?"
- "How does the event loop affect this code?"

**Score:** 10/10 because demonstrates mastery of all concepts in realistic scenario.

---

**Q10: "Explain how JavaScript's single-threaded execution model relates to execution contexts and the call stack. Include the event loop."**

**What interviewer is testing:**
- Understanding of JavaScript's threading model
- Understanding of the event loop
- Understanding of how it affects execution contexts
- Production-aware thinking about performance

**Perfect Answer:**

"JavaScript is **single-threaded**, which means **only one piece of code can execute at a time**. This is fundamental to understanding execution contexts and the call stack.

**What Single-Threaded Means:**

```javascript
// Only ONE execution context can be on the call stack at a time

function a() { b(); }
function b() { c(); }
function c() { /* do work */ }

a();

// Call stack:
// Step 1: [a]
// Step 2: [a, b]
// Step 3: [a, b, c]
// Step 4: [a, b]       (c done, popped)
// Step 5: [a]          (b done, popped)
// Step 6: []           (a done, popped)

// NEVER [a, b, c] executing SIMULTANEOUSLY
// Always sequential, one at a time
```

**Why Single-Threaded:**

1. **Simplicity**: No race conditions, no locks, no deadlocks
2. **Memory safety**: Variables aren't accessed by multiple threads
3. **Consistency**: Predictable order of execution

**The Problem with Single-Threaded:**

If a function takes too long, it blocks everything:

```javascript
// ❌ Blocking example
function slowLoop() {
  // This takes 5 seconds
  for (let i = 0; i < 1000000000; i++) {
    // Useless loop
  }
}

slowLoop(); // Blocks for 5 seconds!

// During these 5 seconds:
// - Can't click buttons
// - Can't scroll
// - Can't update UI
// - User sees frozen page

// Why? slowLoop is on call stack, nothing else can execute
```

**The Solution: Event Loop**

JavaScript uses the **Event Loop** to handle asynchronous operations without blocking:

```
┌────────────────────────────────────────────────────┐
│ EVENT LOOP (Coordinates Execution)                 │
├────────────────────────────────────────────────────┤
│                                                    │
│  CALL STACK      MICROTASK QUEUE    TASK QUEUE    │
│  ┌────────────┐  ┌──────────────┐  ┌────────────┐ │
│  │ Function A │  │ Promise 1    │  │ setTimeout │ │
│  │ Function B │  │ Promise 2    │  │ addEventListener
│  └────────────┘  └──────────────┘  └────────────┘ │
│                                                    │
│  Loop: While call stack not empty:               │
│  1. Execute call stack item                       │
│  2. If microtask queue: Execute all microtasks    │
│  3. Render (if needed)                            │
│  4. If task queue: Execute one task               │
│  5. Repeat                                        │
│                                                    │
└────────────────────────────────────────────────────┘
```

**Event Loop in Action:**

```javascript
console.log('1');  // Sync code: immediate

setTimeout(() => {
  console.log('2'); // Task queue: ~4ms delay
}, 0);

Promise.resolve()
  .then(() => {
    console.log('3'); // Microtask queue: immediate after sync
  });

console.log('4');  // Sync code: immediate

// Output order:
// 1 (synchronous, immediate)
// 4 (synchronous, immediate)
// 3 (microtask, runs after sync but before task)
// 2 (task, runs after microtasks)

// Why?
// Step 1: Execute '1' → stack empty
// Step 2: Execute '4' → stack empty
// Step 3: setTimeout scheduled → goes to task queue
// Step 4: Promise scheduled → goes to microtask queue
// Step 5: Stack empty, check microtask queue
// Step 6: Execute '3' from microtask
// Step 7: Microtask queue empty, check task queue
// Step 8: Execute '2' from task queue
```

**Detailed Timeline:**

```
TIME  CALL STACK    MICROTASK      TASK QUEUE    OUTPUT
────────────────────────────────────────────────────────
0ms   [main]        []             []            

      Execute console.log('1')
      
1ms   [main]        []             []            "1"

      Execute setTimeout → goes to task queue
      
2ms   [main]        []             [setTimeout]  

      Execute Promise.resolve() → goes to microtask queue
      
3ms   [main]        [Promise]      [setTimeout]

      Execute console.log('4')
      
4ms   [main]        [Promise]      [setTimeout]  "4"

      main script ends
      Stack empty!
      
5ms   []            [Promise]      [setTimeout]

      Check microtask queue (has Promise)
      Execute Promise callback
      
6ms   []            []             [setTimeout]  "3"

      Microtask queue empty
      Check task queue (has setTimeout)
      Execute setTimeout callback
      
7ms   []            []             []            "2"
```

**How This Relates to Execution Contexts:**

```javascript
// Each queue item creates its own execution context

console.log('A');  // EC in main context

setTimeout(() => {
  // EC created for setTimeout callback
  console.log('B');
}, 0);

Promise.resolve().then(() => {
  // EC created for Promise callback
  console.log('C');
});

console.log('D');  // EC in main context

// Execution contexts created in order: main, main, main, then:
// 1. setTimeout callback EC
// 2. Promise callback EC
// But Promise callback EC runs FIRST (microtask priority)

// Output: A, D, C, B
```

**The Single-Threaded Event Loop Pattern:**

```javascript
// ✅ Non-blocking pattern (uses event loop)

function nonBlocking() {
  // Start async work
  fetch('/data')
    .then(data => {
      // New execution context later
      processData(data);
    });
  
  // Return immediately, don't block
  return;
}

// ❌ Blocking pattern (freezes page)

function blocking() {
  // Do expensive work on call stack
  for (let i = 0; i < 1000000000; i++) {
    // Just spinning
  }
  
  // This blocks everything until done
}
```

**Practical Example: UI Updates**

```javascript
// ❌ Blocking - page freezes
button.addEventListener('click', () => {
  // Large computation on call stack
  for (let i = 0; i < 1000000000; i++) {
    // Computation
  }
  
  // Page frozen the entire time
  // User can't interact
});

// ✅ Non-blocking - page responsive
button.addEventListener('click', () => {
  // Schedule computation in tasks
  setTimeout(() => {
    for (let i = 0; i < 1000000000; i++) {
      // Computation
    }
  }, 0);
  
  // Page responsive
  // Computation happens in task queue
  // Browser can handle other events in between
});

// ✅ Even better - break into chunks
button.addEventListener('click', () => {
  let index = 0;
  const CHUNK_SIZE = 100000;
  const MAX = 1000000;
  
  function processChunk() {
    const end = Math.min(index + CHUNK_SIZE, MAX);
    
    for (let i = index; i < end; i++) {
      // Process
    }
    
    index = end;
    
    if (index < MAX) {
      // Schedule next chunk
      setTimeout(processChunk, 0);
      // Allows UI updates between chunks
    }
  }
  
  processChunk();
});
```

**Memory Management in Event Loop:**

```javascript
// Each execution context created by event loop has its own memory

let global = 'shared';

button.addEventListener('click', () => {
  // EC1 for first click
  let localClick1 = 'first click';
  // Memory: EC1 variables
});

button.addEventListener('click', () => {
  // EC2 for second click (different EC!)
  let localClick2 = 'second click';
  // Memory: EC2 variables (independent of EC1)
  
  // EC1 variables from first click can be garbage collected
});

// When first click happens:
// EC1 created with localClick1
// Event processed
// EC1 destroyed
// localClick1 garbage collected

// When second click happens:
// EC2 created with localClick2
// Event processed  
// EC2 destroyed
// localClick2 garbage collected

// No memory leak if properly cleaned up
```

**Performance Implication:**

Understanding the event loop is critical for performance:

```javascript
// ❌ Long running sync code blocks
function slowProcess() {
  for (let i = 0; i < 10000000; i++) {
    // Heavy computation
  }
}

// Metrics:
// - Time to interactive: 5+ seconds
// - Frames dropped: ~300
// - User frustration: Very high

// ✅ Async pattern
async function fastProcess() {
  for (let i = 0; i < 10000000; i += 100000) {
    // Process chunk
    if (i > 0) {
      await new Promise(resolve => setTimeout(resolve, 0));
      // Yields to event loop
      // Other tasks can run
    }
  }
}

// Metrics:
// - Time to interactive: <100ms
// - Frames dropped: 0
// - User experience: Smooth

// Why? Event loop can:
// - Handle user clicks
// - Render updates
// - Run other code
// - Between our chunks
```

**Key Takeaways:**

1. **Single-threaded**: Only one execution context on call stack
2. **Not blocking**: Event loop allows asynchronous operations
3. **Queues**: Microtasks run before tasks (Promise before setTimeout)
4. **Memory per context**: Each context has its own memory, GC'd when done
5. **Performance**: Understanding event loop = writing responsive apps

**Production implications:**

```javascript
// At Google/Meta, slow code = user leaves = lost revenue
// Understanding execution contexts, call stack, event loop = faster apps

// Rule of thumb:
// - If operation takes >50ms, split into smaller chunks
// - Use async/await instead of blocking loops
// - Monitor for long task warnings (DevTools)
// - Test on slow devices/networks
```"

**Common mistakes:**
❌ Thinking JavaScript is multi-threaded (it's not)
❌ Not understanding microtask vs task priority
❌ Writing blocking code that freezes UI
❌ Not knowing why setTimeout needed
✅ Understanding single-threaded, event loop, and how to write responsive code

**Follow-up questions:**
- "What's the difference between microtasks and macrotasks?"
- "Why does Promise run before setTimeout?"
- "How would you detect long-running tasks?"
- "How do Service Workers fit into this?"

**Score:** 10/10 because explains threading model, event loop mechanics, and practical implications.

---

## ⚡ PART 4: QUICK REFERENCE

### The 30-Second Pitch

**"JavaScript execution contexts are 'environments' where code runs. Each function call creates a new context on the call stack. The heap stores objects, the stack stores primitives and references. The event loop manages asynchronous code using queues (microtask and task). Garbage collection frees memory when objects have zero references. Understanding execution contexts, the call stack, memory, and the event loop is critical for writing correct, performant, and non-leaking JavaScript."**

### The 3 Critical Things

1. **Execution Context = Environment**: Each function call creates a new context with its own variables, `this` binding, and scope chain. Think of it as the stage where that function performs.

2. **Call Stack = Current Execution**: The call stack tracks what function is currently running. One context at a time. When a function returns, its context is removed.

3. **Memory Management = Manual Cleanup**: Objects in the heap are garbage collected only when they have zero references. Global variables, closures, and event listeners can prevent GC. Understand references to prevent leaks.

### Quick Answers

| Question | 30-Second Answer |
|----------|-----------------|
| What's execution context? | Environment where code runs. Includes variables, this, scope chain. New one per function call. |
| Stack vs Heap? | Stack: primitives, references, organized. Heap: objects, arrays, unorganized. |
| Why does hoisting happen? | During creation phase, JavaScript moves declarations to top (var) or TDZ (let/const). |
| What's a closure? | Function that remembers variables from parent scope. Keeps parent variables in memory. |
| Memory leak? | Object not garbage collected because it still has references. Fix: remove references. |
| How does GC work? | Mark & Sweep: mark reachable objects, sweep (delete) unreachable ones, free memory. |
| Event loop? | Coordinates execution: check call stack, microtasks, tasks in order. Allows async without blocking. |
| Why single-threaded? | Only one execution context on stack at a time. Simpler, safer. Uses event loop for concurrency. |
| setTimeout vs Promise? | Promise = microtask (before setTimeout), setTimeout = task (after microtasks). |
| Temporal Dead Zone? | Period where let/const exist but are inaccessible. Causes ReferenceError if accessed. |

### The Gotchas

- **Gotcha #1 - Var Hoisting Confusion**: `var` hoisted but initialized as undefined. Can access before declaration. Use let/const instead.
- **Gotcha #2 - Object Assignment**: Assigning object to variable copies reference, not object. Both point to same memory.
- **Gotcha #3 - Closure Memory Leaks**: Functions in closures keep parent variables in memory forever. Limit what's captured.
- **Gotcha #4 - Event Listener Leaks**: Event listener holds reference, preventing GC of listener and callback variables.
- **Gotcha #5 - Global Pollution**: Forgot var/let/const = global variable. Pollutes global scope, leaks memory.
- **Gotcha #6 - Stack Overflow**: Infinite recursion fills stack until error. Always need base case.
- **Gotcha #7 - This Binding**: `this` changes based on how function called. Extract method = loses context.
- **Gotcha #8 - Async Context**: await pauses context, resumes later. Different from normal sequential execution.

### Interview Strategy

**If you're strong:**
- Discuss garbage collection algorithms in detail
- Explain event loop with microtask/task distinction
- Create complex examples with closures and memory
- Discuss production memory profiling

**If you're medium:**
- Explain execution contexts, call stack, heap clearly
- Know hoisting and TDZ
- Understand closures and basic memory leaks
- Know how to use DevTools

**If you're weak:**
- Focus on: execution context basics, call stack, stack vs heap
- Understand function execution and variable scope
- Know what hoisting does (simplified)
- Say you'd like to learn garbage collection details

### Checklist

- [ ] Can explain execution context in 30 seconds
- [ ] Understand call stack (LIFO, grows/shrinks)
- [ ] Know difference: primitives on stack, objects on heap
- [ ] Understand hoisting (var vs let vs const)
- [ ] Understand closures and their memory implications
- [ ] Know how garbage collection works (mark & sweep)
- [ ] Can identify memory leaks (global vars, listeners, closures)
- [ ] Understand event loop (call stack → microtask → render → task)
- [ ] Know single-threaded JavaScript and how event loop helps
- [ ] Can use DevTools to find memory leaks

---

## 🧠 PART 5: QUIZ - RAPID FIRE (20 Questions)

### Q1: What does an execution context contain?

- A) Just variables
- B) Variables, this binding, and scope chain
- C) Only the call stack
- D) Only the heap memory

**Correct Answer:** B - An execution context contains variables (variable environment), the `this` binding, and the scope chain (lexical environment). It's the complete environment for code execution.

---

### Q2: How many execution contexts are on the call stack at the same time in JavaScript?

- A) Multiple (JavaScript is multi-threaded)
- B) Exactly one (JavaScript is single-threaded)
- C) All of them (they all exist simultaneously)
- D) Depends on the browser

**Correct Answer:** B - JavaScript is single-threaded. Only one execution context is on the call stack at a time. They execute sequentially in a LIFO (Last In First Out) order.

---

### Q3: What's stored on the call stack vs the heap?

- A) Everything on the stack, nothing on heap
- B) Primitives on stack, objects on heap (and references on stack)
- C) Objects on stack, primitives on heap
- D) It varies by browser

**Correct Answer:** B - Primitives (numbers, strings, booleans) and references are on the stack. Objects and arrays are on the heap. The stack stores pointers to heap objects.

---

### Q4: What happens when you assign an object to another variable?

- A) The object is copied to a new location
- B) The reference is copied, both point to same object
- C) A new object is automatically created
- D) Memory is duplicated

**Correct Answer:** B - Assigning an object copies the reference, not the object itself. Both variables point to the same object in memory. Modifying one affects both.

---

### Q5: What is the Temporal Dead Zone?

- A) A bug in JavaScript
- B) Period where let/const are hoisted but inaccessible
- C) A zone where functions can't execute
- D) Only related to setTimeout

**Correct Answer:** B - The Temporal Dead Zone (TDZ) is the period between a let/const being hoisted and its declaration. Accessing it causes ReferenceError. This prevents the undefined confusion of var.

---

### Q6: What does hoisting do with `var`?

- A) Moves the entire var statement to top
- B) Hoists declaration, initializes with undefined
- C) Doesn't hoist var at all
- D) Makes var inaccessible until declaration

**Correct Answer:** B - Hoisting moves the var declaration to the top of the scope and initializes it with undefined. The assignment stays where it is. This can cause confusing undefined values.

---

### Q7: What is a closure?

- A) A function that closes
- B) A function that remembers variables from parent scope
- C) A way to end a program
- D) Only related to callback functions

**Correct Answer:** B - A closure is a function that has access to (remembers) variables from its parent scope, even after the parent function returns. The parent variables stay in memory as long as the closure exists.

---

### Q8: When is an object garbage collected?

- A) When the function that created it returns
- B) When it has zero references in the reachable scope
- C) Automatically after a timer
- D) When explicitly deleted

**Correct Answer:** B - Objects are garbage collected when they have zero references that can be reached. If anything still points to them (global vars, closures, listeners), they stay in memory.

---

### Q9: What causes a memory leak?

- A) Using too many functions
- B) Objects kept in memory due to forgotten references
- C) Running your computer out of power
- D) Syntax errors in code

**Correct Answer:** B - A memory leak occurs when objects are kept in memory because references to them are never removed. Global variables, event listeners, and closures are common causes.

---

### Q10: What is the event loop?

- A) A bug in JavaScript
- B) Mechanism that coordinates: call stack, microtasks, tasks, rendering
- C) Only related to setTimeout
- D) Something you use to debug loops

**Correct Answer:** B - The event loop is the mechanism that coordinates JavaScript's single-threaded execution: checking call stack, processing microtasks, rendering, and processing tasks. It allows asynchronous code without blocking.

---

### Q11: What's the difference between microtasks and tasks?

- A) Microtasks are smaller versions of tasks
- B) Microtasks (Promise) run before tasks (setTimeout)
- C) No difference, they're the same
- D) Tasks run before microtasks

**Correct Answer:** B - Microtasks (Promises, queueMicrotask) run immediately after current code finishes. Tasks (setTimeout, events) run after all microtasks. This priority is essential to understanding async execution order.

---

### Q12: What happens when you call a function multiple times?

- A) The same execution context is reused
- B) A new execution context is created each time
- C) No execution context is created for functions
- D) All contexts exist simultaneously

**Correct Answer:** B - Each function call creates a new, independent execution context. Multiple calls mean multiple contexts with their own variables. This is why local variables don't persist between calls.

---

### Q13: What is the call stack?

- A) A list of all functions in the program
- B) A data structure tracking which function is currently executing
- C) A place where you store variables
- D) Only used for debugging

**Correct Answer:** B - The call stack is a LIFO (Last In First Out) data structure that tracks the current execution. When you call a function, it's pushed. When it returns, it's popped. One item at a time.

---

### Q14: What causes a stack overflow?

- A) Too many variables
- B) Too much memory used
- C) Recursion with no base case, filling stack
- D) Having too many files

**Correct Answer:** C - Stack overflow occurs when the call stack gets too deep, usually from infinite recursion. The stack has a limit. Without a base case to stop recursion, the stack fills and throws an error.

---

### Q15: What is `this` binding in an execution context?

- A) Always refers to global object
- B) Determined by how the function is called
- C) Always refers to the object containing the function
- D) Never important in modern JavaScript

**Correct Answer:** B - `this` is bound when a function is called. It depends on context: object method (object), arrow function (parent scope), regular function (window/undefined). Understanding execution context explains `this` behavior.

---

### Q16: How do you prevent a memory leak from closures?

- A) Don't use closures
- B) Only capture variables you actually need
- C) Use var instead of let
- D) Clear the browser cache

**Correct Answer:** B - Closures capture variables from parent scope. To prevent leaks, only capture what you need. Extract specific values instead of capturing entire objects or arrays.

---

### Q17: What's the relationship between execution context and scope?

- A) They're the same thing
- B) Execution context defines the scope
- C) Scope is determined by where code is written (lexical), execution context by where it runs
- D) No relationship

**Correct Answer:** C - Scope (lexical/static) is determined by where code is written in the source. Execution context (dynamic) is the runtime environment. Together they determine variable access via the scope chain.

---

### Q18: How does `await` affect execution context?

- A) It destroys the context
- B) It pauses the context, resumes later when Promise settles
- C) It creates multiple contexts
- D) It has no effect

**Correct Answer:** B - `await` pauses the execution context and returns control to the event loop. When the Promise settles, the context resumes with the resolved value. This is how async code doesn't block.

---

### Q19: When does garbage collection happen?

- A) On a specific schedule (every millisecond)
- B) When explicitly called
- C) When the engine decides (varies), usually when memory pressure increases
- D) Never automatically, must be manual

**Correct Answer:** C - Garbage collection runs automatically, but the exact timing is up to the JavaScript engine. Modern engines use generational GC and different strategies. It happens more frequently when memory pressure is high.

---

### Q20: What's the difference between a function context and the global context?

- A) There's no difference
- B) Global context exists once at startup, function contexts created/destroyed per call
- C) Function contexts are always nested in global
- D) Global context doesn't have variables

**Correct Answer:** B - Global execution context is created once when JavaScript starts and exists for the entire program. Function execution contexts are created when functions are called and destroyed when they return. Global context contains all global variables.

---

### Scoring

- **18-20: Expert** — You deeply understand execution contexts, memory, and JavaScript internals
- **15-17: Advanced** — Strong grasp of most concepts; may have gaps in event loop or GC
- **12-14: Intermediate** — Good foundation; understand basics, need to learn details
- **9-11: Beginner** — Core concepts understood; review Part 1 & 2 for deeper learning
- **<9: Review needed** — Study execution contexts, call stack, and memory fundamentals

---

## 🛠 PART 6: MACHINE CODING CHALLENGE

### The Challenge

**Build a memory-aware cache system that demonstrates understanding of execution context, memory management, and garbage collection. The cache should:**

1. Store objects efficiently
2. Have a max size limit (prevent unbounded memory growth)
3. Allow get/set operations
4. Track memory usage
5. Implement eviction policy (LRU - Least Recently Used)
6. Prevent memory leaks
7. Be safe for concurrent operations

### Clarifying Questions (Ask These in Interview)

- Should the cache be thread-safe? (JavaScript is single-threaded, but good to ask)
- What eviction policy? (LRU, FIFO, LFU?) (Let's do LRU)
- Should we track memory usage? (Yes, educate about memory)
- Can entries expire? (Good bonus, but not required for MVP)
- Should it work with both primitives and objects? (Yes, both)

### Solution Guide

#### Step 1: Basic Cache with Memory Tracking

```javascript
// ✅ Basic cache with memory awareness

class MemoryAwareCache {
  constructor(maxSize = 100, maxMemoryMB = 50) {
    this.maxSize = maxSize;
    this.maxMemoryBytes = maxMemoryMB * 1024 * 1024;
    
    this.cache = new Map();        // Store data
    this.accessOrder = [];         // Track access for LRU
    this.estimatedMemory = 0;      // Track memory usage
  }
  
  // Estimate size of value in bytes
  estimateSize(value) {
    if (typeof value === 'string') {
      return value.length * 2; // 2 bytes per character
    }
    if (typeof value === 'number') {
      return 8; // Numbers are 8 bytes
    }
    if (typeof value === 'object') {
      // Rough estimate for objects
      return JSON.stringify(value).length * 2;
    }
    return 8; // Default
  }
  
  get(key) {
    if (!this.cache.has(key)) {
      return null;
    }
    
    // Update access order (LRU)
    this.accessOrder = this.accessOrder.filter(k => k !== key);
    this.accessOrder.push(key);
    
    return this.cache.get(key);
  }
  
  set(key, value) {
    // If key already exists, remove it first
    if (this.cache.has(key)) {
      const oldValue = this.cache.get(key);
      this.estimatedMemory -= this.estimateSize(oldValue);
      this.cache.delete(key);
      this.accessOrder = this.accessOrder.filter(k => k !== key);
    }
    
    const valueSize = this.estimateSize(value);
    
    // Check if value fits in memory
    if (valueSize > this.maxMemoryBytes) {
      throw new Error('Value too large for cache');
    }
    
    // Add the value
    this.cache.set(key, value);
    this.estimatedMemory += valueSize;
    this.accessOrder.push(key);
    
    // Evict if necessary
    this.evictIfNeeded();
  }
  
  evictIfNeeded() {
    // Evict if over size limit
    while (this.cache.size > this.maxSize) {
      this.evictLRU();
    }
    
    // Evict if over memory limit
    while (this.estimatedMemory > this.maxMemoryBytes) {
      this.evictLRU();
    }
  }
  
  evictLRU() {
    // Remove least recently used (first in accessOrder)
    const lruKey = this.accessOrder.shift();
    const value = this.cache.get(lruKey);
    
    this.estimatedMemory -= this.estimateSize(value);
    this.cache.delete(lruKey);
  }
  
  // Get cache statistics
  getStats() {
    return {
      size: this.cache.size,
      maxSize: this.maxSize,
      estimatedMemoryMB: (this.estimatedMemory / 1024 / 1024).toFixed(2),
      maxMemoryMB: this.maxMemoryBytes / 1024 / 1024,
      utilizationPercent: ((this.cache.size / this.maxSize) * 100).toFixed(1),
    };
  }
  
  clear() {
    this.cache.clear();
    this.accessOrder = [];
    this.estimatedMemory = 0;
  }
}
```

#### Step 2: Advanced Features

```javascript
// ✅ Advanced cache with expiration and weak references

class AdvancedMemoryAwareCache extends MemoryAwareCache {
  constructor(maxSize = 100, maxMemoryMB = 50) {
    super(maxSize, maxMemoryMB);
    this.expiration = new Map(); // Track expiration
    this.hitCount = new Map();   // Track hit count (for stats)
  }
  
  set(key, value, ttlSeconds = null) {
    super.set(key, value);
    
    // Set expiration if provided
    if (ttlSeconds) {
      const expirationTime = Date.now() + (ttlSeconds * 1000);
      this.expiration.set(key, expirationTime);
      
      // Schedule cleanup
      setTimeout(() => {
        if (this.expiration.get(key) === expirationTime) {
          this.delete(key);
        }
      }, ttlSeconds * 1000);
    }
  }
  
  get(key) {
    // Check if expired
    if (this.expiration.has(key)) {
      if (Date.now() > this.expiration.get(key)) {
        this.delete(key);
        return null;
      }
    }
    
    // Track hits for stats
    if (this.cache.has(key)) {
      this.hitCount.set(key, (this.hitCount.get(key) || 0) + 1);
    }
    
    return super.get(key);
  }
  
  delete(key) {
    if (this.cache.has(key)) {
      const value = this.cache.get(key);
      this.estimatedMemory -= this.estimateSize(value);
      this.cache.delete(key);
      this.expiration.delete(key);
      this.hitCount.delete(key);
      this.accessOrder = this.accessOrder.filter(k => k !== key);
    }
  }
  
  // Get detailed statistics
  getDetailedStats() {
    const baseStats = this.getStats();
    
    // Calculate hit ratio
    const totalHits = Array.from(this.hitCount.values())
      .reduce((a, b) => a + b, 0);
    
    return {
      ...baseStats,
      totalHits,
      hitRatio: (totalHits / (totalHits + (this.maxSize - this.cache.size)))
        .toFixed(3),
      topKeys: Array.from(this.hitCount.entries())
        .sort(([,a], [,b]) => b - a)
        .slice(0, 5)
        .map(([key, hits]) => ({ key, hits })),
    };
  }
}
```

#### Step 3: Complete Example

```javascript
// ✅ Full working example

// Initialize cache
const cache = new AdvancedMemoryAwareCache(5, 1); // 5 items, 1MB max

// Add data
cache.set('user:1', { name: 'Alice', age: 30 }, 60); // Expires in 60 sec
cache.set('user:2', { name: 'Bob', age: 25 }, 60);
cache.set('user:3', { name: 'Charlie', age: 35 }, 60);

console.log('Stats after adding 3 users:');
console.log(cache.getStats());
// {
//   size: 3,
//   maxSize: 5,
//   estimatedMemoryMB: '0.00',
//   maxMemoryMB: 1,
//   utilizationPercent: '60.0'
// }

// Access user:1 multiple times (updates LRU)
cache.get('user:1');
cache.get('user:1');
cache.get('user:2');

// Add more data
cache.set('user:4', { name: 'David', age: 40 }, 60);
cache.set('user:5', { name: 'Eve', age: 28 }, 60);

// At limit, adding more triggers LRU eviction
cache.set('user:6', { name: 'Frank', age: 45 }, 60);

console.log('After eviction:');
console.log(cache.getStats());
// user:3 was evicted (least recently used)
// user:6 was added

console.log('Detailed stats:');
console.log(cache.getDetailedStats());

// Clean up
cache.clear();
```

### Evaluation Criteria

| Criterion | Points | What Interviewer Looks For |
|-----------|--------|---------------------------|
| **Memory Tracking** | /10 | Estimates memory correctly, enforces limits |
| **LRU Eviction** | /10 | Implements least recently used correctly |
| **Execution Context** | /10 | Shows understanding of how memory allocated/freed |
| **Garbage Collection** | /10 | References cleaned up, objects can be GC'd |
| **Code Quality** | /10 | Clean structure, good variable names, no leaks |
| **Total** | /50 | |

---

### Common Mistakes to Avoid

❌ **Not tracking access order**: LRU needs to know which items used recently
```javascript
// Wrong: Just checking if key exists
const lru = Object.keys(cache)[0];

// Right: Maintaining access order array
this.accessOrder = this.accessOrder.filter(k => k !== key);
this.accessOrder.push(key);
```

❌ **Memory not freed after eviction**:
```javascript
// Wrong: Delete but don't update memory counter
this.cache.delete(key);

// Right: Update memory counter
this.estimatedMemory -= this.estimateSize(value);
this.cache.delete(key);
```

❌ **Not cleaning up references**:
```javascript
// Wrong: Leaves dangling references
this.accessOrder = ... // Doesn't remove evicted key

// Right: Clean all references
this.accessOrder = this.accessOrder.filter(k => k !== key);
this.expiration.delete(key);
this.hitCount.delete(key);
```

❌ **No memory limit enforcement**:
```javascript
// Wrong: Just checks size limit
if (this.cache.size > this.maxSize) { ... }

// Right: Check both size and memory
while (this.cache.size > this.maxSize || 
       this.estimatedMemory > this.maxMemoryBytes) {
  this.evictLRU();
}
```

### Follow-up Questions Interviewer Might Ask

- "How would you handle concurrent operations?"
- "What if entries had different importance levels?"
- "How would you implement persistence (database)?"
- "How would you distribute cache across multiple servers?"
- "What about weak references for non-critical data?"

### Time Estimate

- Step 1 (Basic): 20 minutes
- Step 2 (Advanced): 25 minutes
- Step 3 (Testing): 10 minutes
- **Total: 45-60 minutes**

---

## ✅ PART 7: MASTERY CHECKLIST & NEXT STEPS

### Mastery Checklist

Before you claim mastery of Execution Context & Memory, verify:

#### Execution Context
- [ ] Can explain what execution context is (not just call stack)
- [ ] Understand creation phase vs execution phase
- [ ] Know variable environment, this binding, scope chain
- [ ] Understand lexical vs dynamic scope
- [ ] Can trace execution through nested functions
- [ ] Know how function declarations vs expressions behave

#### Call Stack
- [ ] Understand LIFO (Last In First Out) principle
- [ ] Can visualize stack as functions are called/returned
- [ ] Know what happens at each stack level
- [ ] Understand stack overflow and recursion limits
- [ ] Can use DevTools to inspect call stack
- [ ] Know how async operations interact with stack

#### Memory (Heap vs Stack)
- [ ] Understand primitives on stack, objects on heap
- [ ] Know what references are and how they work
- [ ] Understand why object assignment copies reference
- [ ] Know difference between shallow and deep copy
- [ ] Understand memory allocation for different types
- [ ] Can estimate object size in memory

#### Hoisting
- [ ] Understand var hoisting (declaration only)
- [ ] Understand let/const hoisting (TDZ)
- [ ] Know function hoisting (complete)
- [ ] Understand why this is confusing
- [ ] Prefer let/const over var in practice
- [ ] Know temporal dead zone implications

#### Closures
- [ ] Understand what closures are (not just callbacks)
- [ ] Know how closures capture variables
- [ ] Understand closure memory implications
- [ ] Know common closure patterns (factory, data privacy)
- [ ] Can identify unintended closures
- [ ] Know how to prevent closure memory leaks

#### Garbage Collection
- [ ] Understand Mark & Sweep algorithm basics
- [ ] Know when objects are collected (zero references)
- [ ] Understand reference counting concept
- [ ] Can identify memory leaks
- [ ] Know common leak causes (globals, listeners, closures)
- [ ] Can use DevTools to profile memory

#### Event Loop & Async
- [ ] Understand JavaScript is single-threaded
- [ ] Know event loop: call stack → microtasks → tasks
- [ ] Understand microtasks vs tasks (Promise vs setTimeout)
- [ ] Know how await affects execution context
- [ ] Can predict async execution order
- [ ] Understand how event loop prevents blocking

#### `this` Binding
- [ ] Know how `this` is determined (not what value it has)
- [ ] Understand method call (object), function call (undefined), arrow function (parent)
- [ ] Know bind, call, apply methods
- [ ] Understand `this` in event handlers
- [ ] Can explain `this` surprises in interviews

### How to Practice

1. **Draw Execution Contexts**: For any code, draw the call stack at each point
2. **Predict Output**: Write code, predict output order (especially async)
3. **Memory Profiling**: Use DevTools to see memory growth/shrinkage
4. **Find Leaks**: Intentionally create leaks, use DevTools to find them
5. **Trace Execution**: Use `debugger` statement, step through code
6. **Write Closures**: Create closure patterns, understand memory implications
7. **Event Loop Visualization**: Understand execution order with mixed async code

### Timeline to Mastery

- **Week 1**: Learn basics (Parts 0, 1, 2) - execution context, call stack, memory
- **Week 2**: Learn hoisting, closures, garbage collection (Part 1)
- **Week 3**: Learn event loop and async (Part 1)
- **Week 4**: Practice with coding challenges (Part 6)
- **Week 5**: Interview questions and tricky scenarios (Part 3)
- **Week 6**: Advanced patterns and memory profiling

**Total: 4-6 weeks to solid mastery**

### Next Topics After Execution Context & Memory

After mastering execution context, move to:

1. **Scope & Closures (1.4.5)** — Deep dive into lexical scope and closures
   - Module pattern, IIFE, data privacy
   - Higher-order functions
   - Closure patterns and anti-patterns

2. **Functions (1.4.3)** — Now that you understand execution context
   - Function types and declarations
   - First-class functions, higher-order functions
   - Currying, partial application
   - Pure functions and functional programming

3. **Asynchronous JavaScript (1.4.6)** — Deep dive into async with new understanding
   - Event loop in detail
   - Callbacks, Promises, async/await
   - Race conditions and concurrency patterns
   - Error handling in async

### Real-World Application

In a real job (Google, Meta, Netflix):

- **Debugging**: 50% of debugging time goes to understanding execution context
- **Performance**: Understanding memory = writing fast code (avoiding GC pauses)
- **Architecture**: Designing systems around JavaScript's single-threaded nature
- **Memory Management**: Large-scale apps must manage memory carefully
- **Code Review**: Reviewers check for memory leaks and execution issues

### Interview Tips

1. **Draw it out**: Execution contexts, call stacks, memory - always draw
2. **Explain the why**: "Objects on heap because they're dynamic and large"
3. **Know the gotchas**: "Var hoisting causes undefined, use let/const"
4. **Use real examples**: Talk about event listeners, closures, memory leaks
5. **Mention debugging**: "I'd use DevTools to inspect this"
6. **Think about performance**: "This could cause memory pressure if called 1000s of times"
7. **Be precise with terminology**: Don't confuse scope with execution context

### Resources

- **MDN Execution Context**: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this
- **You Don't Know JS**: https://github.com/getify/You-Dont-Know-JS (Scope & Closures)
- **JavaScript.info**: https://javascript.info/
- **Chrome DevTools**: https://developer.chrome.com/docs/devtools/
- **Event Loop Visualizer**: http://latentflip.com/loupe/

---

## 🎓 Final Thoughts

Execution Context & Memory is **foundational to advanced JavaScript**. It explains:

- Why `this` behaves unexpectedly
- Why memory leaks happen
- Why the event loop is necessary
- Why closures capture variables
- Why garbage collection is needed
- Why hoisting is confusing

**In interviews**, demonstrate:
1. Deep understanding (not just "what" but "why")
2. Mental models (draw stacks, heaps, contexts)
3. Practical awareness (memory leaks, performance)
4. Debugging ability (use DevTools, trace execution)
5. Production thinking (scale, optimization, monitoring)

Most developers skip this because it's "hard" or "theoretical." They're wrong. **This is the foundation of everything else.** Master it, and you'll solve bugs in minutes instead of hours. You'll write code that scales to millions of users. You'll understand JavaScript deeply.

**This is worth the effort.** 🚀

---

**You've completed the comprehensive Execution Context & Memory guide. You're ready to master Scope & Closures (1.4.5) next.** 🎯

