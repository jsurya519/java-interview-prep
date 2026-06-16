# Spring & Spring Boot Interview Questions

## 1. Spring Core & IoC (1–10)
1. What is Dependency Injection?
2. How does Spring IoC Container work?
3. Difference between BeanFactory and ApplicationContext?
4. What are singleton, prototype, request, and session scopes?
5. How does @Autowired work internally?
6. Difference between @Component, @Service, @Repository, and @Controller?
7. How does @Primary work?
8. What is @Qualifier and when do you use it?
9. How does Spring resolve circular dependencies?
10. What are BeanPostProcessors?

## 2. Spring Boot Fundamentals (11–20)
11. Why Spring Boot over Spring Framework?
12. What is auto-configuration?
13. How does @SpringBootApplication work internally?
14. Explain component scanning.
15. How can you disable auto-configuration?
16. What is Spring Boot Starter?
17. How do starters reduce dependency management?
18. What happens during Spring Boot startup?
19. How does embedded Tomcat work?
20. Difference between Spring MVC and Spring Boot?

## 3. Configuration (21–30)
21. Difference between application.properties and application.yml?
22. How does @Value work?
23. How does @ConfigurationProperties work?
24. Difference between @Configuration and @Component?
25. What is profile-specific configuration?
26. How do Spring Profiles work?
27. How do you activate profiles?
28. How do you externalize configurations?
29. How do environment variables override properties?
30. What is Spring Cloud Config?

## 4. Bean Lifecycle (31–40)
31. Explain Spring Bean lifecycle.
32. What is @PostConstruct?
33. What is @PreDestroy?
34. Difference between InitializingBean and @PostConstruct?
35. What are BeanFactoryPostProcessors?
36. How are beans instantiated?
37. What happens if two beans have same name?
38. How does lazy initialization work?
39. What is eager initialization?
40. When should prototype scope be avoided?

## 5. Spring MVC (41–50)
41. Explain DispatcherServlet flow.
42. Difference between @Controller and @RestController?
43. What is HandlerMapping?
44. What is ViewResolver?
45. How does request mapping work?
46. Difference between @PathVariable and @RequestParam?
47. What is @RequestBody?
48. How does Jackson serialization work?
49. How do you implement pagination?
50. How do you handle file uploads?

## 6. Exception Handling (51–55)
51. Difference between checked and runtime exceptions?
52. How does @ExceptionHandler work?
53. What is @ControllerAdvice?
54. How do you create global exception handling?
55. How do you return standard error responses?

## 7. Spring Data JPA (56–70)
56. Difference between JPA and Hibernate?
57. What is EntityManager?
58. What are entity states?
59. Difference between save() and saveAndFlush()?
60. What is dirty checking?
61. What is persistence context?
62. What causes LazyInitializationException?
63. Difference between FetchType.LAZY and EAGER?
64. What is N+1 query problem?
65. How do you solve N+1 issue?
66. What is first-level cache?
67. What is second-level cache?
68. Difference between getById() and findById()?
69. What is optimistic locking?
70. What is pessimistic locking?

## 8. Transactions (71–80)
71. How does @Transactional work internally?
72. Why does @Transactional fail in self-invocation?
73. What is transaction propagation?
74. Explain REQUIRED propagation.
75. Explain REQUIRES_NEW propagation.
76. Explain NESTED propagation.
77. What causes transaction rollback?
78. Difference between checked and unchecked rollback?
79. What is transaction isolation?
80. Explain all isolation levels.

## 9. Microservices (81–90)
81. How do services communicate?
82. Difference between RestTemplate and WebClient?
83. Why is RestTemplate deprecated?
84. What is service discovery?
85. What is Eureka?
86. What is API Gateway?
87. What is Circuit Breaker?
88. What is distributed tracing?
89. How do you propagate correlation IDs?
90. How do you handle distributed transactions?

## 10. Security (91–95)
91. How does Spring Security work?
92. Difference between authentication and authorization?
93. How does JWT authentication work?
94. How do filters work in Spring Security?
95. How would you secure REST APIs?

## 11. Production & Performance (96–100)
96. How do you monitor Spring Boot applications?
97. What is Spring Boot Actuator?
98. How do you troubleshoot memory leaks?
99. How would you improve startup time?
100. Your service latency suddenly increased from 100ms to 3s. How would you debug it?

---

## Top 20 Must-Master Questions

For your experience level, prioritize these:

1. @SpringBootApplication internals
2. Auto Configuration
3. Bean Lifecycle
4. @Autowired internals
5. DispatcherServlet flow
6. @Transactional internals
7. Propagation levels
8. Isolation levels
9. Self-invocation problem
10. Persistence Context
11. Dirty Checking
12. Lazy vs Eager
13. N+1 problem
14. Optimistic vs Pessimistic Locking
15. RestTemplate vs WebClient
16. Circuit Breaker
17. JWT flow
18. Spring Security filter chain
19. Actuator
20. Performance troubleshooting