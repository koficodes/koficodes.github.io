---
title: 'When Software Is Not Soft Anymore: The Nature of Software Complexity.'
excerpt: "Understanding complexity in software development."
comments: true
categories:
  - Software-Engineering
tags:
  - Software-design
  - Code-quality
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: https://cdn-images-1.medium.com/max/12000/0*SEur1ZRBnjNnCmHa
---

## Introduction
One morning you show up to work and your boss walked to you and asked you to implement a feature that is in high demand by clients of an application you’ve developed with your team sometime in the past. 
As usual, your boss asked your estimated time of delivery and with much enthusiasm, you gave a couple of weeks (assuming 3weeks) for the feature to be completed.
A week passed by and you’ve not been able to add any substantial contribution to the project, you spend more time than usual staring at your screen and alternating between multiple opened tabs.
Managers keep asking for updates on your progress but you can barely give meaningful feedback. All you could say is “I’m working on it”

![](https://cdn-images-1.medium.com/max/2000/1*kG-89irfL6FwQN9MlUzRlw.gif)

You realized it’s now more difficult to add new features or make changes than when the project was freshly being developed in the past. But wait! Isn’t Software supposed to be soft?
 As in, shouldn’t software be able to be reshaped into any form by making changes or adding features easily?

![](https://cdn-images-1.medium.com/max/2000/0*hkhljBUO2G7ziBI-)

Since the early days, Software has been hailed for the possible changes that can be made to it as compared to hardware which is impossible to change once manufactured and has to be replaced when changes are required. The possibility of making changes to software when requirement demands it has always been the selling point for software for as long as we know, yet there comes a time when making changes to software seems close to impossible and sometimes the software project has to be rewritten from scratch just like how hardware has to be replaced when changes are required.
This hindrance to adding features or making changes to software usually comes at a cost, which is unexpected considering the fact that it is well known that software should allow changes to be made when needed.

![[https://www.computerhistory.org/revolution/birth-of-the-computer/4/78](https://www.computerhistory.org/revolution/birth-of-the-computer/4/78)](https://cdn-images-1.medium.com/max/2000/0*WkZJk4p-yR3ZJk0t)

## What makes adding features or changes to software more difficult?

Now, what causes this hindrance? what makes adding features more difficult?
The **complexity** of software is what makes it difficult to make changes to it.
This comes in many forms and almost every developer experiences it at some point in their career.
The ability to recognize and reduce **complexity** in software is a very important skill for a software developer and that’s what distinguishes a great developer from the others.

## Understanding software complexity.

Complexity in software can be defined as **anything related to the structure of the software system that makes it difficult to understand and modify.**
The various forms of software complexity are:

* Difficulty in understanding what a piece of code does.

* Takes too much effort to make small improvement or it’s unclear which part of the system to modify in order to make a small improvement.

* When fixing one bug introduces or creates another bug.

When software is difficult to understand and modify it is considered as complicated or **complex** but when it’s easy to understand and modify, then it’s **simple**.

### Size Doesn’t Matter.

The word **Complex** is often used to describe large software systems with very sophisticated features but for the purpose of this article when a large system is easy to understand and modify it’s not complex.
In other words, when a small software system is difficult to understand and takes too much effort to modify then it’s considered as complex.
Complexity is what you face as a developer at a particular time when you are trying to achieve a goal. It doesn’t relate to the overall size or functionality of the software system.

### You Read More Code Than You Write.

If you’ve been in software development for a while I bet you’ve already come to the realization that developers read more code than they write.
The complexity of a piece of code is more obvious to readers of your code. If your own code seems simple to you but others find it difficult to understand then it’s complex. Your job as a developer is not just writing code that works but also to write code that others can understand and work with easily

## Attributes of Software Complexity
Generally, complexity manifests in three ways.

**Change amplification**: This happens when multiple parts of a software system have to be modified to satisfy one simple requirement.
eg. Consider a web application that has multiple frontend templates with colors defined as inline CSS on each page. When the theme or color palettes of the web application change, all other pages have to be updated.

**Cognitive Load**: This refers to the amount of information a developer has to know about the system in order to modify it. A system that requires a developer to spend more time learning a lot of information before accomplishing a task is said to have a high cognitive load. This can lead to unwanted bugs when a developer misses some vital information about the software system.
eg. Using a resource-intensive class that has no in-built mechanism to free the acquired resources but expects the developer to know when to free those resources.

**Unknown unknowns**: This is when a developer doesn’t know which part of the software system to modify or doesn’t know the information needed to accomplish a task.
eg. When a developer is tasked to make changes to a system that uses a library that is not well documented makes it very difficult for the developer to complete the task assigned.

## **Causes of Complexity**

Complexity is mostly caused by **dependencies** and **obscurity.**

**Dependencies**: 
A dependency exists when a piece of code can’t work in isolation but depends on other pieces of code or other parts of the software system to function properly.
Technically, Anything that a piece of software requires to do what it is intended to do can be classified as a dependency. 
Dependencies are inevitable and are used in almost any software system.
Whenever you call a function in your code, you create a dependency between your code and the implementation of that function. When a new parameter is added to the function or there are some changes in the implementation of the function, it affects your code directly or indirectly (remember *Change amplification ?).*

**Obscurity**:
This happens when an important piece of information is not obvious. eg Not using meaningful variable or function names or the documentation of a function is very lacking and does not state the information needed to use the function properly.

### Complexity is Incremental

Complexity doesn’t just happen. It accumulates over a period of time.
This happens because many small dependencies and obscurities build up over time. Eventually, this makes it difficult to understand and modify the software system. Tasks that suppose to take little time to accomplish will then take too much time to complete.

## It’s All about complexity.

There has been a wave of ideas, experienced engineers, and industry experts that have given talks, authored books, and other resources with one common goal, to help other developers build maintainable software systems.
Most of the ideas shared are techniques or methods to reduce complexity in software systems.
Ideas like [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself), [YAGNI](https://martinfowler.com/bliki/Yagni.html), [SOLID](https://en.wikipedia.org/wiki/SOLID), books like clean code and clean architecture by Robert C. Martin and Refactoring by Martin Fowler and many more have one thing in common, that is building software that is easy to understand and to modify.

## Conclusion

A combination of dependency and obscurity leads to complexity in software systems which manifests in the form of change amplification, cognitive load, and unknown unknowns.
Building software systems with the intensions of making them easy to understand and modify can yield long term benefits in the future which might not be obvious at the beginning of a software project but it’s worth putting in the extra effort of making it as simple as possible.

## Resources

* A Philosophy of Software Design by John Ousterhout

* Clean Code Robert C. Martin

* Clean Architecture: A Craftsman’s Guide to Software Structure and Design by Robert C. Martin

* Refactoring by Martin Fowler

* Code Simplicity: The Fundamentals of Software by Max Kanat-Alexander
