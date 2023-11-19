---
title: "SOLID Principle"
date: "2022-06-29"
tags: ["心法"]
---

***

* SOLID principles complement each other, and work together in union.

## Single Responsibility Principle

* Every software component should have one and only one responsibility / **reason to change**
* Group the responsibilities in a sensible way
* Cohesion - 内聚
  * The degree to which the various parts of a software component are related
  * Example of garbage classification
  * Aim for high cohesion
  * ![entropy-and-cohesion](https://raw.githubusercontent.com/hadjShell/hadjshell.github.io/3ec2708a01e389a49fa6541aaa6d27e67cd940c3/imgs/entropy-and-cohesion.svg)
* Coupling - 耦合
  * The level of inter dependency between various software components
  * Aim for loose coupling
  * ![entropy-and-coupling](https://raw.githubusercontent.com/hadjShell/hadjshell.github.io/3ec2708a01e389a49fa6541aaa6d27e67cd940c3/imgs/entropy-and-coupling.svg)

* Changes are inevitable, so we need **higher cohesion and loose coupling**

## Open Closed Principle

* Software components should be closed for modification, but open for extension
* New features getting added to the software component should NOT have to modify existing code
* A software component should be extendable to add a new feature or to add a new behavior to it
* Example of PS5 remote control and accessories
* Key philosophy: one interface, different implementations
* Open closed principle often requires decoupling

## Liskov Substitution Principle

* Objects should be replacable with their subtypes without effecting the correctness of the program
* Derived classes must be usable through the base class interface, without the need for the user to know the difference
* Change the way of "IS-A" thinking in the real world
  * E.g. `Square` should **NOT** be a subclass of `Rectangle`
* Two way of applying LSP
  * Break the hierarchy if it fails the substitution test
  * Polymorphism
* Most of time using `instanceof` indicates an issue

## Interface Segregation Principle

* No client should be forced to depend on methods it does not use
* Minimize the interface
* Work with SRP and LSP

## Dependency Inversion Principle

* High-level modules should not depend on the low-level modules. Both should depend on abstractions
* Abstractions should not depend on details. Details should depend on abstractions

## Resource

* https://github.com/JackChan1999/DesignPattern
* https://github.com/nahidulhasan/solid-principles
* [Uncle Bob Talk](https://www.youtube.com/watch?v=zHiWqnTWsn4)