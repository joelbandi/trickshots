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

__Question:__ What is a generator? what is a Co-Routine and what are they trying to solve?

In order to understand all the above, Lets take a steop back and understand the javascript model of execution. Javascript as we all know has a single threaded and synchronous execution model.
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
Therefore, generator functions are used to handle these asynchronous code in a clean,efficient and easy-to-read manner.

So, let's get to it.
```javascript
function* funcky_generator(){
	var one = yield isTrue(true);
}
```