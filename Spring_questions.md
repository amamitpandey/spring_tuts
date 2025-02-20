InterView questions :
### IOC(Inversion Of Control):
Giving control to the container to create and inject instances of objects that your application depend upon, means instead of you are creating an object using the new operator, let the container do that for you. Inversion of control relies on dependency injection because a mechanism is needed in order to activate the components providing the specific functionality

The two concepts work together in this way to allow for much more flexible, reusable, and encapsulated code to be written. As such, they are important concepts in designing object-oriented solutions.

Example for Dependency injection

Previously we are writing code like this
```
Public MyClass{
 DependentClass dependentObject
 /*
  At somewhere in our code we need to instantiate 
  the object with new operator  inorder to use it or perform some method.
  */ 
  dependentObject= new DependentClass();
  dependentObject.someMethod();
}

//With Dependency injection, the dependency injector will take off the instantiation for us

Public MyClass{
 /* Dependency injector will instantiate object*/
 DependentClass dependentObject

 /*
  At somewhere in our code we perform some method. 
  The process of  instantiation will be handled by the dependency injector
 */ 

  dependentObject.someMethod();
}
```

The above process of giving the control to some other (for example the container) for the instantiation and injection can be termed as Inversion of Control

You can read more on dependency injection and IOC in my answer :- You can find advantages and applications of the concepts here.

# Can you give few examples of Dependency Injection?
dependentObject= new DependentClass();
  dependentObject.someMethod();

# put vs post vs patch method
put use for update info fully
Patch use for update only one field
Post use for create new info

# What is Auto Wiring? 
Kind fo DI to use another class element


# What are the important roles of an IOC Container?
Initiate dependency obj for use using new key words

# Context
Is just environment, scope where var method exit, can access by this keyword. 

# What are Bean Factory and Application Context?
Can you compare Bean Factory with Application Context?

BeanFactory and ApplicationContext both are Java interfaces and ApplicationContext extends BeanFactory. Both of them are configuration using XML configuration files. In short BeanFactory provides basic Inversion of control(IoC) and Dependency Injection (DI) features while ApplicationContext provides advanced features.

## What’s the difference Between @Controller, @Component, @Repository, @SpringBootApplication and @Service Annotations in Spring?
@component : use in bean, model for easy to use, using @Autowire.
@controller : behave as a logic controller build for normal html, js whether @RestController by default return xml/json response.
@Repository : use with JPA interface for gather info form db, a persistence layer of connection 
@service :  a persistence layer of service 
@SpringBootApplication : 
@Configuration to enable Java-based configuration
@ComponentScan to enable component scanning.
@EnableAutoConfiguration to enable Spring Boot's auto-configuration feature.

## What is @Primary and @Qualifier?
if you have two beans with same interface, then these anotation decide which will get more prefrence.
@qualifier will get more value. 
@Primary indicates that a bean should be given preference when multiple candidates are qualified to autowire a single-valued dependency.

@Qualifier indicates specific bean should be autowired when there are multiple candidates.
```
public interface BeanInterface {

    String getName();
}


public class Bean1 implements BeanInterface {
    @Override
    public String getName() {
        return "bean 1";
    }
}


public class Bean2 implements BeanInterface {
    @Override
    public String getName() {
        return "bean2";
    }
}

// Here is our service.

@Service
public class BeanService {

    @Autowired
    private BeanInterface bean; // it'll get error by say you havemore then one bean
}

```

```
// solution

@Bean("bean1")
@Primary
public BeanInterface bean1() {
    return new Bean1();
}
```



How do you create an application context with Spring?
How do you define a component scan in XML and Java Configurations?

What is the default scope of a bean?
Are Spring beans thread safe?
What are the other scopes available?
How is Spring’s singleton bean different from Gang of Four Singleton Pattern?
What are the different types of dependency injections?
What is setter injection?
What is constructor injection?
How do you choose between setter and constructor injections?
What are the different options available to create Application Contexts for Spring?
What is the difference between XML and Java Configurations for Spring?
What are the different kinds of matching used by Spring for Autowiring?
How do you debug problems with Spring Framework?
How do you solve NoUniqueBeanDefinitionException?
How do you solve NoSuchBeanDefinitionException?
What is CDI (Contexts and Dependency Injection)?
Does Spring Support CDI?
Would you recommed to use CDI or Spring Annotations?
What are the major features in different versions of Spring?
What are new features in Spring Framework 4.0?
What are new features in Spring Framework 5.0?
What are important Spring Modules?
What are important Spring Projects?
What is the simplest way of ensuring that we are using single version of all Spring related dependencies?
Name some of the design patterns used in Spring Framework?
What do you think about Spring Framework?
Why is Spring Popular?
Can you give a big picture of the Spring Framework?
Spring MVC

What is Model 1 architecture?
What is Model 2 architecture?
What is Model 2 Front Controller architecture?
Can you show an example controller method in Spring MVC?
Can you explain a simple flow in Spring MVC?
What is a ViewResolver?
What is Model?
What is ModelAndView?
What is a RequestMapping?
What is Dispatcher Servlet?
How do you set up Dispatcher Servlet?
What is a form backing object?
How is validation done using Spring MVC?
What is BindingResult?
How do you map validation results to your view?
What are Spring Form Tags?
What is a Path Variable?
What is a Model Attribute?
What is a Session Attribute?
What is a init binder?
How do you set default date format with Spring?
Why is Spring MVC so popular?
Spring Boot

What is Spring Boot?
What are the important Goals of Spring Boot?
What are the important Features of Spring Boot?
Compare Spring Boot vs Spring?
Compare Spring Boot vs Spring MVC?
What is the importance of @SpringBootApplication?
What is Auto Configuration?
How can we find more information about Auto Configuration?
What is an embedded server? Why is it important?
What is the default embedded server with Spring Boot?
What are the other embedded servers supported by Spring Boot?
What are Starter Projects?
Can you give examples of important starter projects?
What is Starter Parent?
What are the different things that are defined in Starter Parent?
How does Spring Boot enforce common dependency management for all its Starter projects?
What is Spring Initializr?
What is application.properties?
What are some of the important things that can customized in application.properties?
How do you externalize configuration using Spring Boot?
How can you add custom application properties using Spring Boot?
What is @ConfigurationProperties?
What is a profile?
How do you define beans for a specific profile?
How do you create application configuration for a specific profile?
How do you have different configuration for different environments?
What is Spring Boot Actuator?
How do you monitor web services using Spring Boot Actuator?
How do you find more information about your application envrionment using Spring Boot?
What is a CommandLineRunner?
Database Connectivity - JDBC, Spring JDBC & JPA

What is Spring JDBC? How is different from JDBC?
What is a JdbcTemplate?
What is a RowMapper?
What is JPA?
What is Hibernate?
How do you define an entity in JPA?
What is an Entity Manager?
What is a Persistence Context?
How do you map relationships in JPA?
What are the different types of relationships in JPA?
How do you define One to One Mapping in JPA?
How do you define One to Many Mapping in JPA?
How do you define Many to Many Mapping in JPA?
How do you define a datasource in a Spring Context?
What is the use of persistence.xml
How do you configure Entity Manager Factory and Transaction Manager?
How do you define transaction management for Spring – Hibernate integration?
Spring Data

What is Spring Data?
What is the need for Spring Data?
What is Spring Data JPA?
What is a CrudRepository?
What is a PagingAndSortingRepository?
Unit Testing

How does Spring Framework Make Unit Testing Easy?
What is Mockito?
What is your favorite mocking framework?
How do you do mock data with Mockito?
What are the different mocking annotations that you worked with?
What is MockMvc?
What is @WebMvcTest?
What is @MockBean?
How do you write a unit test with MockMVC?
What is JSONAssert?
How do you write an integration test with Spring Boot?
What is @SpringBootTest?
What is @LocalServerPort?
What is TestRestTemplate?
AOP

What are cross cutting concerns?
How do you implement cross cutting concerns in a web application?
If you would want to log every request to a web application, what are the options you can think of?
If you would want to track performance of every request, what options can you think of?
What is an Aspect and Pointcut in AOP?
What are the different types of AOP advices?
What is weaving?
Compare Spring AOP vs AspectJ?
SOAP Web Services

What is a Web Service?
What is SOAP Web Service?
What is SOAP?
Waht is a SOAP Envelope?
What is SOAP Header and SOAP Body?
Can you give an example of SOAP Request and SOAP Response?
What is a SOAP Header? What kind of information is sent in a SOAP Header?
Can you give an example of a SOAP Header with Authentication information?
What is WSDL (Web Service Definition Language)?
What are the different parts of a WSDL?
What is Contract First Approach?
What is an XSD?
Can you give an example of an XSD?
What is JAXB?
How do you configure a JAXB Plugin?
What is an Endpoint?
Can you show an example endpoint written with Spring Web Services?
What is a MessageDispatcherServlet?
How do you configure a MessageDispatcherServlet?
How do you generate a WSDL using Spring Web Services?
How do you implement error handling for SOAP Web Services?
What is a SOAP Fault?
RESTful Web Services

What is REST?
What are the key concepts in designing RESTful API?
What are the Best Practices of RESTful Services?
Can you show the code for an example Get Resource method with Spring REST?
What happens when we return a bean from a Request Mapping Method?
What is GetMapping and what are the related methods available in Spring MVC?
Can you show the code for an example Post Resource method with Spring REST?
What is the appropriate HTTP Response Status for successful execution of a Resource Creation?
Why do we use ResponseEntity in a RESTful Service?
What is HATEOAS?
Can you give an Example Response for HATEOAS?
How do we implement it using Spring?
How do you document RESTful web services?
Can you give a brief idea about Swagger Documentation?
How do you automate generation of Swagger Documentation from RESTful Web Services?
How do you add custom information to Swagger Documentation generated from RESTful Web Services?
What is Swagger-UI?
What is "Representation" of a Resource?
What is Content Negotiation?
Which HTTP Header is used for Content Negotiation?
How do we implement it using Spring Boot?
How do you add XML support to your RESTful Services built with Spring Boot?
How do you implement Exception Handling for RESTFul Web Services?
What are the best practices related to Exception Handling with respect to RESTful Web Services?
What are the different error status that you would return in RESTful Web Services?
How would you implement them using Spring Boot?
What HTTP Response Status do you return for validation errors?
How do you handle Validation Errors with RESTful Web Services?
Why do we need Versioning for RESTful Web Services?
What are the versioning options that are available?
How do you implement Versioning for RESTful Web Services?


