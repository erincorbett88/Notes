# Spring Boot Basics
This course is a beginner-friendly introduction to Spring Boot. It covers the basics of Spring Boot, including setting up a new project, dependency injection, and database integration.

This course is a Code With Mosh - [Spring Boot: Mastering the Fundamentals](https://members.codewithmosh.com/courses/enrolled/2741443).

Related repositories:
- [SpringBootStore](https://github.com/erincorbett88/SpringBootStore)

## Getting Started with Spring Boot
### Spring Framework: Introduction
- Spring is a framework for building Java applications
- Involves a lot of modules to handle specific tasks. There's:
  - Spring Core: Dependency Injection
  - Spring AOP: Aspect Oriented Programming
  - Spring Web: Web Applications
  - Spring Data: Data Access
  - ...and more
- Spring is modular, so you can pick and choose which modules you need for your project. There's a whole ecosystem.
- Spring Boot is a project that simplifies the Spring Framework by providing a set of tools to make it easier to build Spring applications.
  - it's like a layer on top of the Spring framework, that provides defaults and ready-to-use configurations and features. This helps save time and reduces boilerplate.
### Project Structure
When you first create a new SpringBoot project (either using IntelliJ built in things, or using spring.initializr.io), you will be given a 
project that generates a project automatically. It features several automatic files. In this example, we
used Maven to set up the project and it yielded the following files:
- .idea: config files, only used by intellij
- scr/main/java: where we put our java code
- src/main/resources: stores configuration files (like application.properties or application.yml)
- .mvn: this is part of the maven wrapper. It's a way to run maven without requiring local installation. In intellij, Maven is built-in, so we don't have to install it locally.
- help.md: markdown that includes instructions for getting started (ok to ignore)
- pom.xml: ***this is the heart of a maven project***. It's an XML file that contains all the dependencies and configurations for the project. It's where you define the project's structure and dependencies.
  - stands for "project object model"
  - in a gradle project, the equivalent file is build.gradle

We're using Maven for this project. It is a "build tool." It's more widely used and has been around longer. Gradle is more modern and more optimized for performance, so it's also a good option.
### Dependency Management
- PRO TIP: If you remove the version line from a dependency, if it is nested in a <parent> tag, then maven will automatically download the latest version of that dependency.
- this can simplify our pom file
- if you upgrade springboot, this means all dependency versions will automatically be updated, as well
- therefore, removing version is best practice; let springboot manage the versions _for_ us

Spring has ***Spring Boot Starters***, which is a set of predetermined libraries that Spring has tested and _knows_ they work well together. It just bundles commonly used libraries. For this project, we used ***web starter.***
### Building a Controller
Spring MVC: the basics of how we handle web requests in springboot. We follow the Model-View-Controller pattern, which is a clean way to organize our code.
- Model: data and logic. It's usually connected to data source. In springboot, this can be a simple java class.
- View: what the user sees. Can be dynamically generated using tools like Thymeleaf.
- Controller: a mediator that handles incoming requests from the user, interacts with the model to get the data, and then tells the view what to display.

Some annotations we've learned:
- **@Controller**: tells spring that this class is a controller
- **@RequestMapping**: when we send a request to the root of our website, we want this method to be called. @RequestMapping says to call this method.
  - part of "web starter" dependency
  - have to give it a path, so it knows what to do
  - root of website is ("/")
  - the return statement of a requestmapping method is the name of the view we want to display
- **@Value** - injects a value from the application.properties file into a variable
  - allows us to store configuration values in a file, rather than hardcoding them
  - this is a good practice, because it allows us to change the configuration without changing the code

Running the project:
- first add maven wrapper: ./mvnw -v
- then run the project: ./mvnw spring-boot:run

### Debugging

This course contains a wonderful introduction to debugging. Didn't feel like taking notes.

## Dependency Injection
### What is Dependency Injection?
Dependency Injection is a design pattern used to create loosely coupled components in a system.

What does this mean? 

### What is Dependency Injection (DI)?

To understand this concept, we really have to understand what it means to be "dependent," and also what it means to be "tightly coupled." Think of two examples:
- Order service
  - Consider an online store that has an "OrderService." When a customer places an order, it uses this order service. However, we need another service to process payments. 
  - If we're using Stripe, then the service is "StripePaymentService."
  - However, there's a tight coupling here between OrderService and StripePaymentService. It's true that we're dependent on _some_ payment service, but it doesn't _have_ to be Stripe...
  - if we want to switch to Paypal, does that mean we have to rewrite the OrderService?
- Restaurant
  - Restaurants are dependent on Chefs, but we should be able to switch out Chefs.
  - If we replace "Chef" with "John", that means Restaurant is tightly coupled to John specifically, instead of any chef.
 
The issue isn't that dependency _exists_. Dependency is normal and inescapable. The problem is more so that the issues are _tightly coupled_. We want our OrderService to be dependent on PaymentService, not StripePaymentService.

How to avoid this? Using an interface:
- An interface is a contract that says "this PaymentService _must_ be able to do x, y, or z." That is, it must have a payment method, a credit card id number, etc.
- The OrderService doesn't need to KNOW which _class_ is being implemented; it just calls an _interface_

### Constructor Injection

### Setter Injection

### The Spring IoC container

### Configuring Beans using annotations

### Controlling bean selection

### Externalizing configurations

### configuring beans programatically using Lazy initialization

### Bean scopes and bean lifecycle methods
