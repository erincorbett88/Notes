# Spring Boot Basics
This course is a beginner-friendly introduction to Spring Boot. It covers the basics of Spring Boot, including setting up a new project, dependency injection, and connecting to databases.

This course is a Code With Mosh - [Spring Boot: Mastering the Fundamentals](https://members.codewithmosh.com/courses/enrolled/2741443). 

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
- .mvn: this is part of the maven wrapper. It's a way to run maven without requiring local installation. In intellij, Maven is built-in, so we don't have to install it locally.
- help.md: markdown that includes instructions for getting started (ok to ignore)
- pom.xml: ***this is the heart of a maven project***. It's an XML file that contains all the dependencies and configurations for the project. It's where you define the project's structure and dependencies.
  - stands for "project object model"