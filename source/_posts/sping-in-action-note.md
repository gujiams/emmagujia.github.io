---
title: Spring in Action - Reading Notes
date: 2018-02-10 18:00:00
categories: spring 
tags:
---
This notes will keep updating. It summarizes what I think is important in each chapter. You can pass this post since it barely has examples and it is easier understanding by directly reading the book.
<!-- more -->
### Chapter 1
#### Spring’s bean container
- With **DI**, objects are given their dependencies at creation time by some third party that coordinates each object in the system. Objects aren’t expected to create or obtain their dependencies. **Constructor injection** is a type of DI.
- **Loose coupling**. If an object only knows about its dependencies by their interface (not by their implementation or how they’re instantiated), then the dependency can be swapped out with a different implementation without the depending object knowing the difference.
- In a Spring application, an **application context** loads bean definitions and wires them together. The Spring application context is fully responsible for the creation of and wiring of the objects that make up the application. Spring comes with several implementations of its application context, each primarily differing only in how it loads its configuration.
- Difference between ClassPathXmlApplicationContext, PathXmlApplicationContext, AnnotationConfigApplicationContext.
- **Aspects** ensure that POJOs remain plain.

### Chapter 2

### Chapter 9
