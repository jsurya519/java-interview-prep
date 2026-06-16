1. Spring Core & IoC (1-10)
What is Dependency Injection?
How does Spring IoC Container work?
Difference between BeanFactory and ApplicationContext?
What are singleton, prototype, request, and session scopes?
How does @Autowired work internally?
Difference between @Component, @Service, @Repository, and @Controller?
How does @Primary work?
What is @Qualifier and when do you use it?
How does Spring resolve circular dependencies?
What are BeanPostProcessors?

2. Spring Boot Fundamentals (11-20)
Why Spring Boot over Spring Framework?
What is auto-configuration?
How does @SpringBootApplication work internally?
Explain component scanning.
How can you disable auto-configuration?
What is Spring Boot Starter?
How do starters reduce dependency management?
What happens during Spring Boot startup?
How does embedded Tomcat work?
Difference between Spring MVC and Spring Boot?

3. Configuration (21-30)
Difference between application.properties and application.yml?
How does @Value work?
How does @ConfigurationProperties work?
Difference between @Configuration and @Component?
What is profile-specific configuration?
How do Spring Profiles work?
How do you activate profiles?
How do you externalize configurations?
How do environment variables override properties?
What is Spring Cloud Config?


4. Bean Lifecycle (31-40)
Explain Spring Bean lifecycle.
What is @PostConstruct?
What is @PreDestroy?
Difference between InitializingBean and @PostConstruct?
What are BeanFactoryPostProcessors?
How are beans instantiated?
What happens if two beans have same name?
How does lazy initialization work?
What is eager initialization?
When should prototype scope be avoided?


5. Spring MVC (41-50)
Explain DispatcherServlet flow.
Difference between @Controller and @RestController?
What is HandlerMapping?
What is ViewResolver?
How does request mapping work?
Difference between @PathVariable and @RequestParam?
What is @RequestBody?
How does Jackson serialization work?
How do you implement pagination?
How do you handle file uploads?


6. Exception Handling (51-55)
Difference between checked and runtime exceptions?
How does @ExceptionHandler work?
What is @ControllerAdvice?
How do you create global exception handling?
How do you return standard error responses?


7. Spring Data JPA (56-70)
Difference between JPA and Hibernate?
What is EntityManager?
What are entity states?
Difference between save() and saveAndFlush()?
What is dirty checking?
What is persistence context?
What causes LazyInitializationException?
Difference between FetchType.LAZY and EAGER?
What is N+1 query problem?
How do you solve N+1 issue?
What is first-level cache?
What is second-level cache?
Difference between getById() and findById()?
What is optimistic locking?
What is pessimistic locking?


8. Transactions (71-80)
How does @Transactional work internally?
Why does @Transactional fail in self-invocation?
What is transaction propagation?
Explain REQUIRED propagation.
Explain REQUIRES_NEW propagation.
Explain NESTED propagation.
What causes transaction rollback?
Difference between checked and unchecked rollback?
What is transaction isolation?
Explain all isolation levels.


9. Microservices (81-90)
How do services communicate?
Difference between RestTemplate and WebClient?
Why is RestTemplate deprecated?
What is service discovery?
What is Eureka?
What is API Gateway?
What is Circuit Breaker?
What is distributed tracing?
How do you propagate correlation IDs?
How do you handle distributed transactions?


10. Security (91-95)
How does Spring Security work?
Difference between authentication and authorization?
How does JWT authentication work?
How do filters work in Spring Security?
How would you secure REST APIs?
11. Production & Performance (96-100)
How do you monitor Spring Boot applications?
What is Spring Boot Actuator?
How do you troubleshoot memory leaks?
How would you improve startup time?
Your service latency suddenly increased from 100ms to 3s. How would you debug it?

--------------------------

For your experience level, I would prioritize these Top 20 must-master questions:

@SpringBootApplication internals
Auto Configuration
Bean Lifecycle
@Autowired internals
DispatcherServlet flow
@Transactional internals
Propagation levels
Isolation levels
Self-invocation problem
Persistence Context
Dirty Checking
Lazy vs Eager
N+1 problem
Optimistic vs Pessimistic Locking
RestTemplate vs WebClient
Circuit Breaker
JWT flow
Spring Security filter chain
Actuator
Performance troubleshooting