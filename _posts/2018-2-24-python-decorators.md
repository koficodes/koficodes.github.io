---
title: 'Python Decorators'
excerpt: "Writing clean code with python decorators"
comments: true
categories:
  - Software-Engineering
tags:
  - Python
---

Functions are used to organize statements that does a unit of task. This makes them resuable when we need the same task performed at different parts of our application.  
Example
```python
def greet():
    print("Good morning")
```
Now if we want the function above to do more, we can add more statements.
 ```python
 def greet():
     print("Good morning")
     print("What is your name")
```
You can clearly see that ```greet``` is doing more than what it's name says.  
First it greets and then asks a question which is not ideal.  
It is a good practice for functions to be focused on a unit of task.  

We can solve that by creating a function for each task, a function that greets and a function thats asks a question
```python
def greet():
    print("Good morning")

def ask_user_name():
    print("What is you name")
```
Now if we want to greet and ask the user's name.  
we do this.
```python 
greet() 
# Good morning
ask_user_name() 
# What is your name
```
This means these two statements will be called at any part of our appliction when we need to greet and ask for user's name. But what if there is a better way?  
what if any time we call greet we want to ask the user's name too?  
let's give it a try.  
```python
def greet():
    print("Good morning")
    ask_user_name()

greet()
# Good morning
# What is your name
```
From the above we still have a call to ```ask_user_name``` in the body of ```greet``` which is what we want to avoid.  
let's try again

```python
def greet():
    print("Good morning")


def bind_question(func):
    def greet_with_question():
        func()
        print("what is your name")
    return greet_with_question

greet_and_ask_name = bind_question(greet)
greet_and_ask_name()
# Good morning
# what is your name
```

We've been able to extend the functionality of ```greet``` without modifying its body. Nice!  
We can do better by rebinding the ```greet``` name to the result of ```bind_question```.

```python
greet = bind_question(greet)
greet()
# Good morning
# what is your name
```

The function named ```bind_question``` is called a <b>decorator</b>. It is a nice way of extending a function without modifying the body of the function.
We can see that ```bind_question``` takes a function and then returns a funtion.  
That is the two main features of a decorator.  
so we can say in this context that a decorator is a function that takes a function as an argument and returns a function.

## What is a Decorator

We defined a <b>Decorator</b> as a function that takes a function as an argument and returns a function.  
But that is not entirely true. A class can be a decorator too as long as it implements the ```__call__```
method, this makes the class callable.
We can formally define a <b>decorator</b> as:

 >A decorator is a callable that takes a callable as an argument and returns a callable as a return value.


In python a callable is a function, a method on a class, or even a class that implements the ```__call__``` special method.

This way of extending a function is a common pattern and there is a special syntax for it in python  
```@decorator_name``` .  

So instead rebinding the function name to the decorated one,  we can do :  

```python
def bind_question(func):
    def greet_with_question():
        func()
        print("what is your name")
    return greet_with_question

@bind_question
def greet():
    print("Good morning")


greet()
# Good morning
# what is your name
```

Let's say we now want a function to create a log when ever it is called.  
We will user a class for this.

```python
class Logger:
    """Creates logs for the decorated function"""
    def __init__(self,func):
      self.func = func

    def __call__(self):
      self.func()
      print(self.func.__name__+ " called")

@Logger
def greet():
    print("Good morning")

@Logger
def ask_user_name():
    print("what is your name")


greet()
#Good morning
#greet called

ask_user_name()
#what is your name
#ask_user_name called
```
## Nested Decorators

We can use more than one decorator at once.

```python

@Logger # a callable class 
@bind_question # a function 
def greet():
    print("Good morning")

# Good morning
# what is your name
# greet_with_question called

```

Python has several built-in decorators that are part of the language. eg.    
```@property```  
```@classmethod```  
```@staticmethod```  

## Summary
In this post you learnt that:  
* A __Decorator__ let you extend a function without modifying its body.  
* It is a callable that takes a callable as arguments and returns a callable.
* It makes your code clean by preventing duplications








  