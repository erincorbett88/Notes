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
- you won't have to rewrite OrderService if you switch from Stripe to Paypal, and you don't have to write new tests, either

**Dependency Injection** is where you inject an _object_ (PaymentService implementation) into a _class_ (OrderService) that needs it. This is done through the constructor, or through a setter method.
An interface decouples the two classes, "Stripe" and "Paypal," from the OrderService. This is the essence of Dependency Injection. The interface just says "you have to do this bare minimum", etc.

For example, _before_ decoupling:
```    
public class OrderService {
    public void placeOrder() {
        var paymentService = new StripePaymentService();
        paymentService.processPayment(10);

    }
}
```
You can see that we can't test OrderService without testing StripePaymentService. This is a problem. We want to test OrderService in isolation. 
We also can't switch to Paypal without changing OrderService. So we need an interface to decouple the OrderService from the PaymentService.

### Constructor Injection
Constructor is recommended way to inject a dependency into a class. Dependency is passed as an argument to the constructor:
```
public class OrderService {
  private PaymentService paymentService;
    
  public OrderService(PaymentService paymentService) {
      this.paymentService = paymentService;
  }
    
  public void placeOrder() {
      paymentService.processPayment(10);
  }
}
```
Then, when you create a new OrderService, you provide which payment service you want to use:
```
    var orderService = new OrderService(new StripePaymentService());
```

This is called the **Open Closed Principle**. This principle says that _a class should be open for extension, but closed for modification_. This means that we should be able to add new functionality to a class without changing the existing code. This is what we're doing here. We can add new payment services without changing the OrderService.
Avoiding changes to existing code helps prevent bugs and makes the code more maintainable. This is a guideline, not a rule - meant to help you build and maintain flexible software, but must be used with common sense.

### Setter Injection
this is another way to inject dependencies. It's not as recommended as constructor injection, but it's still useful. It's used when you have a lot of dependencies, and you don't want to have a constructor with a lot of parameters. 

Remember the difference between a constructor and a setter: when a class is instantiated, the constructor is called. A constructor is used to set _initial values_. The setter is called _after_ the object is created. Any values set in the constructor can only be changed through the setter. So if we set a dependency through a setter, it can be changed later for that object.
```
public class OrderService {

    private PaymentService paymentService; //instance variable

//    public OrderService(PaymentService paymentService) {
//        this.paymentService = paymentService;
//    }

    public void placeOrder() {
        paymentService.processPayment(10);
    }

    public void setPaymentService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```
We've commented out the constructor, and added a setter method. This is how you would use it:
```
    var orderService = new OrderService();
    orderService.setPaymentService(new StripePaymentService());
```
However, it's not always recommended because what if, in the above example, you forget to set the payment service? Then you'll get a null pointer exception. This is why constructor injection is recommended. Like so:
```
    var orderService = new OrderService();
    orderService.placeOrder();
```
Therefore, this is only recommended for optional dependencies.

### The Spring IoC container
Spring can create objects and inject them into our classes automatically. Spring has an **IoC** container, that can manage the lifecycle of objects and their dependencies. This is called **Inversion of Control**. In Spring, our objects are called "beans." Spring creates beans, injects dependencies, and manages their lifecycle.

_**IoC**_ stands for **Inversion of Control.** It inverts the control of creating objects and managing dependencies. With IoC, we hand over control of creating objects and injecting depencies. The "application context" is our IoC container. It'll look kind of like this:
```
public class Main {
    public static void main(String[] args) {
        var context = new AnnotationConfigApplicationContext(AppConfig.class);
        //run method returns application context, which is our spring container
        var orderService = context.getBean(OrderService.class);
        orderService.placeOrder();
    }
}
```
So instead of us manually creating objects and injecting dependencies, we let Spring take care of it for us.

### Configuring Beans using annotations
There are two ways to configure a bean - either using class or using annotations.

Adding @component to our OrderService class tells Spring that we want it to manage objects of type OrderService - Spring manages this class as a bean. This is called a "component scan." Spring will scan the package for classes with @Component, @Service, @Repository, or @Controller annotations. It will then create beans for these classes and manage their lifecycle.
We can also mark it as a @Service:
```
@Service
public class OrderService {
```

Initially, we'll get this error:
"Could not autowire. No beans of 'PaymentService' type found."
This means we _also_ have to mark StripePaymentService and PaypalPaymentService as beans; we give them the @Service annotation.

We need to know what **_@Autowired_** means, as well. It tells Spring to autowire a bean with its dependent. It is no longer necessary if you only have one constructor, but you'll see it in older codebases. However, if you have multiple constructors, you'd need it:
```
    public OrderService() {}
    
    @Autowired
    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
```
Above, the class has a default constructor AND a constructor with a parameter. In this case, we'll need @Autowired: it tells Spring which constructor to use. However, if we remove the default constructor, we don't need @Autowired.

Side note on annotations - we've seen a few at this point:
- @Component: general purpose annotation
- @Service: for classes that contain business logic
- @Repository: for classes that interact w/a database
- @Controller: use for marking classes as controllers for handling web requests

### Controlling bean selection
We have two beans that implement the PaymentService interface. How does Spring know which one to use? We can use the @Primary annotation to tell Spring which bean to use by default. This is useful when you have multiple beans that implement the same interface.
There is also the @Qualifier annotation, which is used to specify which bean to use. This is useful when you have multiple beans that implement the same interface, and you want to specify which one to use. You do this by giving every @Service a name:
```
@Service("stripe")
@Service("paypal")
```
Then, in the OrderService class, you can specify which one to use:
```
    @Autowired
    public OrderService(@Qualifier("paypal") PaymentService paymentService) {
        this.paymentService = paymentService;
    }
```
However, this does _not_ create "coupling", because the OrderService code doesn't reference the Paypal class at all. We "don't know" what the name "paypal" means or what class it's referring to.

### Externalizing configurations
The **@Value** annotation injects values from our configuration file (application.properties) into the code.
We can store this in an application.properties file, or in a application.yml file. The latter is more readable, but the former is more common.

properties filed:
```
spring.application.name=store
stripe.apiUrl=https://api.stripe.com
stripe.enabled=true
stripe.timeout=1000
stripe.supported-currencies=USD,EUR,GBP
```

application.yml file:
```
spring:
  application:
    name: store
  stripe:
    apiUrl: https://api.stripe.com
    enabled: true
    timeout: 1000
    supported-currencies: USD, EUR, GBP
```

### Configuring Beans Using Code
We have seen how to configure beans using annotations. However, you can also doing this using Java code. This gives us more control.
You do this in an appConfig file, and give beans an @Bean annotation. That tells you that the method is a "bean producer:"
```
@Bean
//this names the bean "paypal"
public PaymentService paypal() {
        return new PaypalPaymentService();
    }
```

This allows us to use conditional statements in our code:
```
@Value("${payment-gateway}")
    private String paymentGateway;

@Bean
    public OrderService orderService() {
        if (paymentGateway.equals("stripe")) {
            return new OrderService(stripe());
        }
        return new OrderService(paypal());
    }
```

This is also good for when we're using third party libraries that need special configurations. Then, in our PaypalPaymentService class, we _don't_ have to use the @Service or @Component annotation. We can just use the @Bean annotation in the AppConfig file, and gives us full control over bean creation.

### Lazy Initialization
Normally, beans are created upon starting the program automatically. However, some beans are expensive or "heavy" resources, and we don't want them created until they're needed/called. In that case, you can add a @Lazy annotation to the class. This tells Spring to only create the bean when it's needed:
```
@Component
@Lazy
public class HeavyResource {
    public HeavyResource() {
        System.out.println("HeavyResource Created");
    }
}
```

Then, it isn't created until "resource" here is called:
```
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(StoreApplication.class, args);
        var resource = context.getBean(HeavyResource.class);

    }
```

Note: If you are creating the bean _programatically_ - that is, not using the @Service annotation on the class but using the @Bean annotation in the appConfig file - you have to also use the annotation @Lazy to one of the bean-producer methods in the config file. 

### Bean Scopes
Are beans created once and reused, or are they created every time they're requested? This is the bean scope. That is the "rule" that determines _when_ it is created and how long the bean lives in the Spring IoC container.

There are several scopes:
- Singleton: the default scope. The bean is created once and reused every time it's requested.
- Prototype: a new instance is created every time the bean is requested.
- Request: a new instance is created for every HTTP request.
- 


### Bean Lifecycle Hooks
