
# Functional Programming
A way of writing code that focuses on using pure functions describing the relationship between their arguments and return values without creating side effects or depending on the state of an application.

##  Program Paradigm
 How to write /  organize your code, how to reason about programming.

  - **Procedural** :
	- Sequencial series of excecution steps/tasks. 
	- Do not organize the code in big artifacts. 
	- Top to bottom code excecution.

  - **Object Oriented** : 
	- Your data and logic are organised in objects, data as props, methods as behavior. 
	- You think your code as a real life entity, defined in classes, Objects.

  - **Functional**  : 
	- Functions are the way to organize the code. 
	- Functions are pure , clearly defined tasks 
	- Pass data around via parameters.
	- Functions hold behaviour.


## Goals?

  - **Modular** : In FP, the goal is to write separate independent functions that are joined together to produce the final results.
  - **Undestandable**: Programs that are written in a functional style usually tend to be cleaner, shorter, and easier to understand.
  - **Testable** : Functions can be tested on their own, and FP code has advantages in achieving this
  - **Reusable**: Functional code is free from side effects.
- **Extensible** : Composition and Pipeline of functions easy to integrate


## Key features of JavaScript?
JavaScript isn't a purely functional language, but it has all the features that we need for it to work as if it were. The main features of the language that we will be using are as follows:

-   **Functions** as first-class objects
-   **Recursion**: insted of loop controls
-   **Arrow functions**: Short syntaxis 
- **Closures** are a way to implement data hiding
```javascript
const newCounter = ()=> {  
 let count = 0;  
 return ()=> count++;   
}  
  
const nc = newCounter();  
console.log(nc()); // _1_
```
-   **Spread operator** : Handle N number of arguments
```javascript
const x = [1, 2, 3];  
  
function sum3(a, b, c) {  
  return a + b + c;  
}  
  
const y = sum3(...x); 
// equivalent to sum3(1,2,3)  
console.log(y); // 6
```

### Memoization
Since the output of a pure function for a given input is always the same, you can cache the function results and avoid a possibly costly recalculation. This process, which implies evaluating an expression only the first time and caching the result for later calls, is called **memoization**.

### Higher-order functions  (**HOF**)
Functions that receive or return a funciton:
 
-   `reduce()` and reduceRight() to apply an operation to a whole array, reducing it to a single result
-   `map(`) to transform one array into another by applying a function to each of its elements
-   `flat() `to make a single array out of an array of arrays
-   `flatMap()` to mix together mapping and flattening
-   `forEach()` to simplify writing loops by abstracting the necessary looping code


## Stop talking show me code

Problem: To bill a user after a button is clicked.

### Non pure function:
1- Global variable  
5- Global variable  modified
6- Reaching out of scope artifacts (window)

```javascript
1 let clicked = false;  
2 .  
3 function billTheUser(some, sales, data) {  
4   if (!clicked) {  
5     clicked = true;  
6     window.alert("Billing the user...");  
 // _actually bill the user_
7   }  
8 }
```
###  Example using  **Immediately Invoked Function Expression** (IIFE)
```javascript
const billTheUser = (clicked => {  
  return (some, sales, data) => {  
    if (!clicked) {  
      clicked = true;  
      window.alert("Billing the user..."); 
       // _actually bill the user_ 
    }  
  };  
})(false)
```

###  Extract behavior   higher-order solution
```javascript
const once = fn => {  
  let done = false;  
  return (...args) => {  
    if (!done) {  
      done = true;  
      fn(...args);  
    }  
  };  
};

const billTheUser = (some, sales, data, alert) => {  
      alert("Billing the user..."); 
       // _actually bill the user_ 
  };  
```

```html
<button id="billButton" onclick="once(billTheUser)(some, sales, data)">  
  Bill me  
</button>;
```
## FP  - Currying

Techniche to provide functions with only one parameter, heavily used for connecting functions via Pipelining and Composition. 

```javascript 
const sum3 = x => y => z => x + y + z;
sum3(1)(2)(3); // 6
```
For example in linux it is a common practice to pipeline commands.
```sh
command_1 | command_2 | command_3 | .... |

ls -l | more
```
## FP  - Partial Application
**Partial application** (or **partial** function **application**) refers to the process of fixing a number of arguments to a function


```javascript 
const students = [
  { name: "A", grade: 3, height: 50, weight: 50 },
  { name: "B", grade: 2, height: 40 ,weight: 45 },
  { name: "C", grade: 3, height: 60 ,weight: 80 },
  { name: "D", grade: 4, height: 55, weight: 76 },
  { name: "E", grade: 5, height: 55 ,weight: 45 },
  { name: "F", grade: 6, height: 45 ,weight: 30 },
  { name: "G", grade: 2, height: 66, weight: 12 },
];
```
```javascript 
// imperative
var sum = 0, numberOfGrade3Students = 0;

for (var i = 0; i < students.length; i++) {
	var student = students[i];
    if (student.grade === 3) {
    	sum += student.height;
        numberOfGrade3Students++;
    }
}

var averageHeight = sum / numberOfGrade3Students;
```

```javascript
// functional

const sum = (a,b) => a + b;

const isStudenInGrade = grade =>student=> student.grade === grade;

const isThirdGrade = isStudenInGrade(3);

const sum = (a,b) => a + b;

const getHeight = student=> student.height;

const average = (students, criteria, property) => {
const propertyValues = students.filter(criteria).map(property);
const averageMesure = propertyValues.reduce(sum,0) / propertyValues.length;
return averageMesure;
}
    
const curryAverage = criteria=> property=>students=>average(students, criteria, property);

const averageThirdGrade = curryAverage(isThirdGrade)
const calculateAverageThirdGradeHeight = averageThirdGrade(getHeight);

const averageThirdGradeHeight = calculateAverageThirdGradeHeight(students);

```

### Compose
The concept of `compose` is simple — it combines `n` functions. It chains a  flow  right-to-left, calling each function with the output of the last one.

```javascript
var averageHeight = R.compose(R.mean, R.map(getHeight), R.filter(isThirdGrade));

averageHeight(students); 
```

### Pipe
The concept of `pipe` is simple — it combines `n` functions. It’s a pipe flowing left-to-right, calling each function with the output of the last one.
```javascript
var averageHeight = R.pipe(R.filter(isThirdGrade), R.map(getHeight), R.mean);

averageHeight(students); 
```


##  Arity changing

The following code will not behave as expected :
`["123.45", "-67.8", "90"].map(parseInt)`

Why `parseInt(_string, radix_)` : The radix is a number (from 2 to 36) that represents the numeral system to be used 

Fix 
```
const unary = fn => (...args) => fn(args[0]);

["123.45", "-67.8", "90"].map(unary(parseInt)); 
 // _[123, -67, 90]_
```

## Demethodizing – turning methods into functions

```
const demethodize = fn => (arg0, ...args) => fn.apply(arg0, args);

const name = "FUNCTIONAL";  
const toUpperCase = demethodize(String.prototype.toUpperCase);

const result = name.split("").map(toUpperCase);  
/*  
 _["F", "U", "N", "C", "T", "I", "O", "N", "A", "L"]  
_*/

```

## Where are my Gang of Four OOP Patterns ?
