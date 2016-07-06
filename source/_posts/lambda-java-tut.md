---
title: Your guide to Lambda expressions in java
date: 2016-07-06 06:39:56
tags: ["Java","Funtional programming","Tutorials"]
categories: ["Tutorials"]
draft: false
---

You may have previously encountered code that like this:

```java
import static spark.Spark.*;

public class HelloWorld {
    public static void main(String[] args) {
        get("/hello", (req, res) -> "Hello World");
    }
}
```
_taken from [sparkjava](http://www.sparkjava.com)_

And if youre expression looked something like this,
![messi](https://i.imgflip.com/ppzib.jpg "Awkward!")

__Then fear not__, because we will (by the end of this post) understand everything about lambda expression, how to write methods that takes in lambda expressions and how to effectively use lambda expressions in our code.


First of all, We need to understand what a lambda expression is. Sometimes some libraries use programming patterns where the methods take in an inline declaration of a method or a class as an argument using the ```new``` keyword. These are called anonymous classes or methods because they dont have name. They are directly used as soon as they are created and hence, we dont need to store their reference in some variable. Android framework typically uses this pattern to usually implement callbacks and listeners.


Now, what has this got to do with a lambda Expression? well, lambda expressions is a shorter way of writing these anonymous constructs. One can almost say that lambda expressions are like syntactic sugar designed by Java developers to mimic some of the functional programming languages like javascript or haskell.

So without further ado, let's get to the fun part

A lambda expression looks like the following syntax 

	(parameters) -> do something;

or when you have mutliple statements and parameters,
	
	(parameters1, parameter2) -> {
		statement 1;
		statement 2; 
	}

Upon closer investigation you'll notice that a lambda expression is like a method except it does not require a return type and even a name (It's anonymous) you don't even have to  use 'new' keyword or even have block braces if only one statement.




 



