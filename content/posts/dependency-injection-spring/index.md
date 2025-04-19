---
  author: Marco Pegoraro
  title: What is the best way to do dependency injection in Spring?
  date: 2025-04-19 11:00:00
  description: "@Autowired VS Constructor - which one is the best?"
  tags: ["Java", "Spring", "Autowired", "Lombok", "Constructor", "Dependency Injection"] 
  header_image: /posts/dependency-injection-spring/banner.png
  show_summary: false 
---

As you probably know, there are two ways to perform dependency injection in a Spring component, by using constructor: 

```java
@Component
public class GreetingController {

    private GreetingService greetingService;

    public GreetingController(GreetingService greetingService) {
        this.greetingService = greetingService;
    }

} 
```
Or by using the ``@Autowired`` annotation:

```java
@Component
public class GreetingController {

    @Autowired
    private GreetingService greetingService;

} 
```

When I was a junior developer, I really liked how the @Autowired annotation worked. I thought it was easier to use compared to the constructor method — just one annotation on each dependency and you’re good to go. Not to mention, you don’t need to create a constructor for the class.

But as time went on, I realized that this is the worse method for doing dependency injection, for a few reasons:

1. It can't guarantee that the dependencies aren't ``null`` at compile time.
2. It uses reflections in Java to instantiate the dependencies instead of constructing the object in the "normal way"
3. It makes unit testing more difficult — you're forced to use the @Mock and @InjectMocks annotations, and you can't simply mock each dependency and instantiate the class manually.
3. 1. And if you need to mock a @Value variable inside the class, you have to use ReflectionTestUtils. The code becomes much uglier than it needs to be. With constructor injection, you could just pass the string as a parameter.
4. When you have a very large class with lots of dependencies, it starts to look redundant, with many lines using just the @Autowired annotation.

These were some of the reasons why I started using constructor-based dependency injection in Java. But there’s still one thing that bothers me: having to create the constructor and manually assign the dependencies.

```java
@Component
public class TestUseCase {

    private final UserService userService;
    private final ProductService productService;
    private final StockService stockService;
    private final String valueFromProperties;

    public TestUseCase(
      final UserService userService;
      final ProductService productService;
      final StockService stockService;
      @Value("${valueFromProperties}") final String valueFromProperties;
    ) {
        this.userService = userService;
        this.productService = productService;
        this.stockService = stockService;
        this.valueFromProperties = valueFromProperties;
    }

} 
```

Look at how big the constructor gets — it's filled with boilerplate code. I get tired just looking at it. The solution? Good old <a href="https://projectlombok.org" target="_blank">Lombok</a>.

If you're a Java developer, you most certainly know Lombok, so I won’t go into too much detail. But there are a few different annotations used to generate constructors:

<a href="https://projectlombok.org/features/constructor" target="_blank">
@NoArgsConstructor <br/>
@AllArgsConstructor <br/>
@RequiredArgsConstructor <br/><br/>
</a>

The annotation we're interested in is the last one: @RequiredArgsConstructor. It automatically generates a constructor for all the required dependencies marked with the final keyword, like this:

```java
@Component
@RequiredArgsConstructor
public class TestUseCase {

    private final UserService userService;
    private final ProductService productService;
    private final StockService stockService;
    private final String valueFromProperties;

} 
```

Look at how all the boilerplate code is now gone with just this one annotation. But there's still a problem with this code — can you guess what it is?

It's the fact that we're not specifying the value that valueFromProperties should be filled with using the @Value annotation. To fix this, we still need to use the @Value annotation above the variable, like this:

```java
@Component
@RequiredArgsConstructor
public class TestUseCase {

    private final UserService userService;
    private final ProductService productService;
    private final StockService stockService;
    @Value("${valueFromProperties}")
    private final String valueFromProperties;

} 
```

And that’s everything you need! But still, if this code doesn’t work and you’re getting an error like this one:

![Application failed to start](./app-failed-to-start.png)

You're probably using an older version of Java or Lombok. If that’s the case, Lombok might not automatically apply the ``@Value`` annotation in the generated constructor. To fix this, you’ll need to create a file called ``lombok.config`` in the same folder as your ``pom.xml`` or ``build.gradle``. Inside that file, add the following lines:

```
lombok.copyableAnnotations += org.springframework.beans.factory.annotation.Autowired
lombok.copyableAnnotations += org.springframework.beans.factory.annotation.Value
```

Of course, you could just use the ``@Value`` annotation on a non-final variable, and it would work. But the problem is that you'd need to use ReflectionTestUtils in your unit tests.

By using the final keyword, you can create the component instance by passing the value directly into the constructor — which makes your code cleaner and your tests much simpler.

And that’s why, in my opinion, constructor-based dependency injection is the best approach in Spring.

You can check out the full code in the following repository:

https://github.com/marcoagpegoraro/spring-dependency-injection-the-best-way

I'm using Java 8 here just to demonstrate how the ``lombok.config`` file works. Depending on your Java and Lombok versions, you may or may not need it.

To check whether you need the config file, open the target folder and inspect the generated .class file. Look at the constructor — if the ``@Value`` annotation appears in the constructor parameters (as shown in the example below), then you're good to go.

![Generated class](./generated-class.png)

See ya.
