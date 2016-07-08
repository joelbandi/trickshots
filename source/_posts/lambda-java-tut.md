---
title: Your guide to Lambda expressions in java
subtitle: Hit the ground running
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

And if your expression looked something like this,
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

As you can see, We pass it an inline declaration class that is the `Consumer<Integer>` by using the `new` keyword. This is the anonymous construct we have talked about. We then override the accept method in the artifact. The `Consumer` is an interface actually defined in the java.lang.function package. Let's take a good look at the official documentation [here](https://docs.oracle.com/javase/8/docs/api/java/util/function/Consumer.html) We understand that the Consumer is actually annotated by the word `@FunctionalInterface`. This is a special kind of interface and falls under the category of a SAM interface (Single abstract Method). 


To understand `@FunctionalInterface` we must also know about Single Abstract Method interfaces (SAM Interfaces). It means just interfaces with only one single abstract (has to be overriden)  method. In java, we already have many examples of such SAM interfaces. From java 8, they will also be referred as functional interfaces. Java 8, enforces the rule of single responsibility by marking these interfaces with a new annotation i.e. `@FunctionalInterface`. 

The consumer is a functional interface predefined and declared in java8 that executes the overwritten code with the parameter passed given to it in the 'accept' method.(all methods are abstract by default in the abstract class).

Now Let's a function similar to the `forEach` method using all that we have learnt. Our function dubbed as `my_forEach_consumer` takes in two arguments; a list and an anonymous method/Lambda expression to be to be executed. 

Let's start by declaring our method as follows :

```java
// the function definition  of the forEach_Consumer method

static void my_forEach_consumer(List<Integer> list, Consumer<Integer> consumer){
        
        for (Integer i : list){
            
            // the lambda expression gets the variable passed to the accept method
            // here we call consumer accept method which will be overriden when using this function.
            // implying the accept functionality is defined at runtime of this code

            consumer.accept(i);
        
        }
}
```
and now we can use in one of the two ways, the anonymous composition method 

```java
my_forEach_consumer(list, new Consumer<Integer>(){
	
	@Override
	public void accept(Integer integer) {
    	System.out.println("my_forEach_consumer: the number in the list is  :" + integer);
    }

})
```

or wield the power of a lambda expression like so 

```java
// we don't even have to specify the data type of the parameters passed in
// a lambda expression automatically infers a lot from its lexical position in the code

my_forEach_consumer(list, (data) -> System.out.println("my_forEach_consumer (lambda expression): the number in the list is  :" + data))
```

Notice how we reduced the amount of code we have to type, that is one of the main advantages of lambda expressions. Consumer is not the only functional interface we have. Another common interface we might have used without knowing it is one - Runnables.

```java
// a small example of runnables and run methods
new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("Runnable: Hello World from a runnable");
    }
}).start();

// Above code does the same as below
new Thread(  ()-> System.out.println("Runnable (Lambda) : Hellow world from a runnable")  ).start();

```

Now let's try to define our own `@FunctionalInterface` and another method that multiples the numbers by `10` and then prints them in order.
Lets declare our interface named as `Custom`

```java
@FunctionalInterface
public interface Custom<T>{
	
	// YAY! java generics

	// all methods in defined in an interface MUST be abstract so you dont have to explicitly mention the abstract modifier
    void absorb(T t);

}
```

Now, lets define our custom method called `my_forEach_Custom` which takes in a `List`, a object that implements a `Custom` interface and prints the numbers mulitplied by 10.

```java
public static  my_forEach_Custom(List<Integer> list, Custom<Integer> custom){
	
	for(Integer i : list){

		//auto-boxing FTW!!
		i *= 10;
		custom.accept(i);

	}

}
```
Now let's use it in our code:


```java
my_forEach_Custom(list, new Custom<Integer>(){
	
	//overriding the single abstract method (SAM)
	@Override
	void absorb(Integer integer){
		System.out.printn("My_forEach_Custom: the number in the list is  :" + integer);
	}

})```

or be awesome and use a lambda expression like this


```java
my_forEach_Custom(list, (value) -> System.out.printn("My_forEach_Custom (Lambda): the number in the list is  :" + integer));
```

Lambda expression are a very useful way to implement what is known as a callback, A callback is function whose code is supplied at run-time and called into execution programmatically at a predefined time. It is of extreme importance in asyncrhonous web programming models that languages like nodejs and web browsers use to function.

Now that you have everything you need to not only use lambda expressions but also write library code that supports a lambda expression, you can now hit the ground running. Thanks for reading this tutorial and I hope it helped you as much it helped me. Please follow my blog and me for regular trickshots.

Now take a [gift](http://imgur.com/topic/Aww/8NxOHTw) from me and tarry on forward fellow coder!

The source for this tutorial is available [here](https://github.com/joelbandi/Implementations/blob/master/Misc/Java8/Lambda_Complete.java)