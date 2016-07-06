---
title: Your guide to Lambda expressions in java
date: 2016-07-06 06:39:56
tags: ["Java","Funtional programming","Tutorials"]
categories: ["Tutorials"]
draft: false
---

You may have previously encountered code that looked like this:

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


First of all, We need to understand what a lambda expression is. Sometimes some libraries use programming patterns where the methods take in an inline declaration of a method or a class as an argument using the `new` keyword. These are called anonymous classes or methods because they dont have name. They are directly used as soon as they are created and hence, we dont need to store their reference in some variable. Android framework typically uses this pattern to usually implement callbacks and listeners.


Now, what has this got to do with a lambda Expression? well, lambda expressions is a shorter way of writing these anonymous constructs. One can almost say that lambda expressions are like syntactic sugar designed by Java developers to mimic some of the functional programming languages like javascript or haskell.

So without further ado, let's get to the fun part

A lambda expression looks like the following syntax 

	(parameters) -> do something;

or when you have mutliple statements and parameters,
	
	(parameters1, parameter2) -> {
		statement 1;
		statement 2; 
	}

Upon closer investigation you'll notice that a lambda expression is like a method except 
 
1. It does not require a return type. It's automatically inferred!!
2. It doesnt  need a name (It's anonymous).
3. you don't have to  use 'new' keyword.
4. No need of block braces if only one statement.
5. No need of mentioning the data type if the parameters. It's automatically inferred!!

OK, Now to write one of our own , let us define a new java List of Integer's:

```java
List<Integer> = Arrays.asList(1,2,3,4,5,6,7,8,9);
```
In order to print the numbers in this list, we use a lambda expression

```java
list.forEach((Integer value)) -> System.out.println("Lambda expression: The number is " + value));
```
Now compile and run this code using a _java8_ JDK
This is a lambda expression in all its glory. In fact, The `Integer` was unnecessary

```java
list.forEach((value)) -> System.out.println("Lambda expression: The number is " + value));
```


 Now experienced java programmers immediately think about two things:

1. How in the heck is the right parameter being passed into the lambda expression??!?!?!
2. The `forEach` method in the `List<T>` interface must be specially set-up so that it takes in a lambda expression

To answer these questions, Let us closely look at the __original__ and more verbose way of writing the above code and what the `forEach`method really is looking for in terms of arguments.

```java
list.forEach(new Consumer<Integer>() {
            
        @Override
        public void accept(Integer integer) {
            System.out.println("Object reference: the number in the list is  :" + integer);
        }
});
```

As you can see, We pass it an inline declaration class that is the `Consumer<Integer>` by using the `new` keyword. This is the anonymous construct we have talked about. We then override the accept method in the 





 



