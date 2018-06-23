# Library

It is just a ***collection of routines*** (functional programming) or ***class definitions***(object oriented programming). 
The reason behind is simply code reuse, i.e. get the code that has already been written by other developers. 
The classes or routines normally define ***specific*** operations in a ****domain specific area****. For example, there are some
libraries of mathematics which can let developer just call the function without redo the implementation of how an algorithm works.

# Framework

In framework, all the ***control flow*** is already there, and there are a bunch of predefined white spots that we should fill out with our code. A framework is normally more complex. It defines a skeleton where the application defines its own features to fill out the skeleton. In this way, your code will be called by the framework when appropriately. The benefit is that developers do not need to worry about if a design is good or not, but just about implementing ***domain specific functions***.
> Frameworks are designed to solve recurring problems in application development. Whatever the task in an application, by using a framework, you ensure consistency and efficiency in tackling that task. This is what frameworks are all about: consistently and effectively providing solutions to problems in a structured manner. 

# Library, Framework and your Code image representation
![framework_vs_library](../assets/framework_vs_library.png)


# Key difference

The key difference between a library and a framework is "***Inversion of Control***" . When you call a method from a library, you are in control. But with a framework, the control is inverted: the framework calls you.

> A **library** is essentially a set of functions that you can call, these days usually organized into classes. Each call does 
some work and returns control to the client.
>
> A **framework** embodies some abstract design, with more behavior built in. In order to use it you need to insert your behavior 
into various places in the framework either by subclassing or by plugging in your own classes. The framework's code then calls 
your code at these points.

# References
- [What is the difference between a framework and a library?](https://stackoverflow.com/questions/148747/what-is-the-difference-between-a-framework-and-a-library)
- [Inversion Of Control](https://martinfowler.com/bliki/InversionOfControl.html)
