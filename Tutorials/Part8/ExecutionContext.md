#Execution Context

>Execution context is the environment in which the code is executed. The execution context of a variable or function defines what 
>other data it has access to, as well as how it should behave. Execution context are of three types:

#### Global Execution Context: 
>This is the default exection context in the which JavaScript code first executes. 
>In web browsers, the global context is said to be that of the window object, so all global variables
>and functions are created as properties and methods on the window object.
>There can be only one global execution context. 

#### Functional Execution Context:
>Each function has its own executed context. Functional execution context are created on each function call. 
>There can be any number of functional execution context.

>When an execution context has executed all of its code, it is destroyed, taking with it all of the variables and functions
>defined within it. The global execution context isn’t destroyed until the application exits, such as when a web
>page is closed or a web browser is shut down.

###Execution context stack
>JavaScript creates a stack of execution context in which it stores all the execution context (both global and functional). 
>When the browser loads the JavaScript file, it pushes Global Execution Context in the Execution Context stack.
>While executing the code in the Global context, when the execution flow gets a function call, it creates a new functional executional 
>context and pushes it to the top in the Execution Context stack.The browser will always execute the current 
>execution context that sits on top of the stack, and once the function completes executing the current execution context, 
>it will be popped off the top of the stack, returning control to the context below in the current stack. 
>Lets understand this with the help of an example:

```javascript
var a = 10;

functionA();

function functionA(){

	console.log("Start function A");

	function functionB(){
		console.log("In function B");
	}

	functionB();

}

console.log("GlobalContext");
```
>Here, when the script loads, by default the global execution context is pushed in the Execution Context stack and
>and the code execution continues with the global execution context. 
>When the execution flow reaches functionA() i.e. call to the functionA, the execution flow enters the body of functionA
>and before executing any code it creates functional execution context and pushes it to the top the stack. 
>As functional execution context of functionA is on the top of execution context stack, JavaScript engines start executing the 
>functionA.
>When inside functionA, execution flow reaches functionB() i.e. call to the functionB, the above process repeats and 
>execution context of functionB is pushed to the top of execution context stack. 
>When functionB is completely executed, JavaScript engines pops off the functionB executional context and returns the 
>control to the functionA execution context.
>Similarly, once functionA is completely executed. functionA execution context will be popped of the execution context stack.
>and the control returns to the global execution context. 

### Execution context in detail
>An execution context can be divided into a creation and execution phase

#### Creation Stage [when the function is called, but before it executes any code inside]:
>In the creation stage, the syntax parser will walk through each line of code, and when it comes to a new variable or function definition, it will commit these variables and functions to memory. Basically it performs the following three actions:
1. Creates the scope chain
2. Create variables, functions and arguments
3. Determine the value of **this**

#### Activation or code execution stage
> Assign values, references to functions and interpret / execute code

#### Execution object can be represented with the following three object:

```javascript
executionContextObj = {
    'scopeChain': { /* variableObject + all parent execution context's variableObject */ },
    'variableObject': { /* function arguments / parameters, inner variable and function declarations */ },
    'this': {}
}
```
>When code is executed in a context, a scope chain of variable objects is created. The purpose of the
>scope chain is to provide ordered access to all variables and functions that an execution context has
>access to. The front of the scope chain is always the variable object of the context whose code is
>executing. If the context is a function, then the activation object is used as the variable object. An
>activation object starts with a single defi ned variable called arguments. (This doesn’t exist for the
>global context.) The next variable object in the chain is from the containing context, and the next
>after that is from the next containing context. This pattern continues until the global context is
>reached; the global context’s variable object is always the last of the scope chain.
>Identifi ers are resolved by navigating the scope chain in search of the identifi er name. The search
>always begins at the front of the chain and proceeds to the back until the identifi er is found. (If the
>identifi er isn’t found, typically an error occurs.)

1. Find some code to invoke a function.
2. Before executing the function code, create the  execution context.
3. Enter the creation stage:
	1. Initialize the Scope Chain.
	2. Create the variable object:
		1. Create the arguments object, check the context for parameters, initialize the name and value and create a reference copy.
		2. Scan the context for function declarations:
			1. For each function found, create a property in the  variable object that is the exact function name, which has a reference pointer to the function in memory.
			2. If the function name exists already, the reference pointer value will be overwritten.
		3. Scan the context for variable declarations:
			1. For each variable declaration found, create a property in the variable object that is the variable name, and initialize the value as undefined.
			2. If the variable name already exists in the  variable object, do nothing and continue scanning.
	3. Determine the value of "this" inside the context.
	
Activation / Code Execution Stage:
1. Run / interpret the function code in the context and assign variable values as the code is executed line by line.

```javascript
function foo(i) {
    var a = 'hello';
    var b = function privateB() {

    };
    function c() {

    }
}

foo(22);
```
On calling foo(22), the creation stage looks as follows:

```javascript
fooExecutionContext = {
    scopeChain: { ... },
    variableObject: {
        arguments: {
            0: 22,
            length: 1
        },
        i: 22,
        c: pointer to function c()
        a: undefined,
        b: undefined
    },
    this: { ... }
}
```

As you can see, the creation stage handles defining the names of the properties, not assigning a value to them, with the exception of formal arguments / parameters. Once the creation stage has finished, the flow of execution enters the function and the activation / code execution stage looks like this after the function has finished execution:

```javascript
fooExecutionContext = {
    scopeChain: { ... },
    variableObject: {
        arguments: {
            0: 22,
            length: 1
        },
        i: 22,
        c: pointer to function c()
        a: 'hello',
        b: pointer to function privateB()
    },
    this: { ... }
}
```







