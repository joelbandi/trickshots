---
title: How to be cool like generators in javascript
subtitle: Pausable functions? really?
date: 2016-09-09 07:52:59
tags: ["Javascript","Functional programming","Tutorials","Asynchronous"]
categories: ["Tutorials"]
draft: false
author: Joel Bandi
---

This tutorial assumes you have knowledge of [npm](www.npmjs.com "Node package manager") and [promises](https://www.youtube.com/watch?v=2d7s3spWAzo "FunFunFunction").

__Question:__ What is a generator? what is a Co-Routine and what are they trying to solve? Do aliens exist?

In order to understand all the above, let's take a step back and understand the javascript model of execution. Javascript as we all know has a single threaded and synchronous execution model.
What that means is that there is only one thread of execution at any given point of time and every thing gets executed sequentially.

Because all developers have trust issues, lets confirm this fact and test out some code here in either the browser or using node.

```javascript
console.log('bork');
setTimeout(function(){console.log('meow')},500);
console.log('woof');
```
We expect the output
```bash
bork #wait-half-second
meow
woof```

But to our dismay we see
```bash
bork
woof  #wait-almost-half-second
meow```

This is because in addition to javascript being a synchronous language it is complimented by the V8 compiler's event loop/queue system. Everytime the system encounters a time-taking function it sticks the routine in the event queue, ready to be executed once the system is free again. This is a feature of the V8 compiler in chrome and node and not javascript itself. If we were to replace `console.log('woof')` with a `for` loop that runs for 5 secs. Do you expect the same output? __ABSOLUTELY__. You dont believe me? Try it yourself.

Your output will be 
```bash
bork
RandomForLoopStuff #after-five-friggin-seconds
meow```

But wait!! 'meow' is supposed to log out after 500 milliseconds, instead takes five seconds. Well the reason behind it is the fact that functions cannot be interrupted in javascript.
The system logs out 'bork' and pushes `console.log('meow')` to the event queue and starts rolling the `for` loop. Within half a second of the start of for loop, the 'meow' is ready to be logged but, the system is busy with the `for` loop and it cannot be paused and it runs to completion after which the 'meow' is logged out.

Well thats a bummer. 

Thats exactly what generator functions try to solve. They are pausable functions. Lets take a look at a generator function.
```javascript
function* funky_generator(){
	for(var i = 1 ; i <=30 ; i++){
		if(i === 15){
			yield "----function paused at 15----";
			continue;
		}
		console.log(i);
	}
}
```
Running a generator function will give you a javascript iterator which can be run by calling its next method. Everytime it encounters a `yield` keyword, the function pauses and returns the iterator object which again contains the next function and a value field which is passed to yield keyword.

```javascript
var iter = generator();
console.log(iter.next().value); //----function paused at 15----
iter.next();
```

Lets us define simple example a simple promise function API to demonstrate a generator function

```javascript
function isTrue(name,value){
	return new Promise(function(resolve,reject){
		if(value === true){
			resolve(`The promise named ${name} has been resolved`);
		}else{
			reject(`The promise named ${name} has been rejected`);
		}
	});
}
```
A promise is a function which is guaranteed to run to completion *_eventually_* but, we just don't know when.
Therefore, generator functions are also used to handle these asynchronous code in a clean, efficient and easy-to-read manner.

So, let's get to it.

```javascript
function* funky_generator(){
	var one = yield isTrue('one',true);
	var two = yield isTrue('two',true);
	console.log(one);
	console.log(two);
}
```
There is a special npm module called `co` which excels in handling generator functions. We can install it using `npm install co -g`.

```javascript
var co = require('co');
co(funky_generator);
```
The above piece of code will log out both our promise messages once they're resolved/rejected. The `co` library first runs the generator function we pass as an argument to it. If the running generator function encounters the `yield` keyword, it realizes that the expression to the right of the keyword evaluates asynchronously and "yields" the execution to the the function wrapping it - The `co` library. The `co` library then receives the iterator object, evaluates the expression value be it a promise,thenable or a thunk and then continues the execution by calling the next method appropriately.

So now Let's try and construct our own generator function handler library. We shall call it `harambe` in honor of our dear departed ape.

```javascript
var harambe = function(generator) {
    if (generator && generator.constructor.name === 'GeneratorFunction') { //A rudimentary type check
        const iterator = generator(); //get iterator from the generator.
		//define a function called iterate that runs a single iteration until it encounter yield keyword again
        const iterate = function(iteration) { 
            if (iteration.done) return iteration.value; //if no more iterations, return the current iteration value
            const promise = iteration.value; //else,get the promise 
			//resolve or reject it acoordingly and pass the value to the next iteration to be iterated recursively
            return promise.then(x => iterate(iterator.next(x)),x => iterate(iterator.next(x))); 
        }
        return iterate(iterator.next()); //start by running the iterator
    } else {
        throw new Error('Argument should be a generator function'); //type check fail safe
    }
}
```
So now let's call it by using our very own function
```javascript
harambe(funky_generator);
```

So there you have it, generators in javascript and how to use them to avoid the hassles of asynchronous programming.
Leave comments down below or send me an email, if you want to talk to me! Keep calm and carry on :3 