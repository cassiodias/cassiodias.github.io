---
layout: post
title:  "Principles of readable code"
date:   2021-03-29 15:45
categories: coding, test, stub
---
# Principles of readable code

<p align="center">
    <img style="width: 380px; height: 200px;" src="https://www.wyliecomm.com/wp-content/uploads/2019/11/readable.png">
</p>

* **Single responsibility** All building blocks - classes, methods, variables - follow the single responsibility principle: every building block does exactly one thing. This makes it easy for the person reading the code to understand what this responsibility is. It also makes it clear what part of the code needs to change.
  

* **Well-structured**. The codebase is easy to navigate around, as functions, classes, modules follow a logical structure. Formatting is consistent across classes and the codebase.
  

* **Thoughtful naming**. Class, function, and variable names all help understand what is happening, and making reading more seamless. Code that has good names often had developers spend multiple iterations coming to these clear names.
  

* **Simple and concise**. The code tries to be as humble and simple as possible. Developers don't use fancy tricks and also avoid over-complicating things. Functions are mostly short, making them easy to read. Classes are also not overly large.
  

* **Comments explain the "why," not the "how."** Most of the code can be understood by itself. Comments fill in the remaining gaps.
  

* **Continuously refactored to keep being readable**. Code-base grow. As a simple class gets more responsibility, it grows in size and complexity. Readable codebases stay readable due to continuous refactoring. The new, complex class might be broken into multiple parts or changed other ways to stay easy to read.
  

* **Well-tested** Well-tested code can be modified quickly and without fear of breaking things. Having the code tested via automated tests is important for the code to stay readable. Without tests, refactoring the code becomes risky, and developers eventually stop doing it. With tests, there is no excuse on why not to make even large and risky refactors, that keep the code easy to read.



