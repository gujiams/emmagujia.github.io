---
title: Spring Annotation note
date: 2018-0202-20 20:00:00
categories: spring
tags:
---
Mark down some important annotations while reading Spring in action.
<!-- more -->
- `@Component` identifies this class as a component class and serves as a clue to Spring that a bean should be created for the class.
- `@PostConstruct` before ,because in a constructor, the injection of the dependencies has not yet occurred.
- `@PreDestroy` 
- `@Resource`,`@Inject`,`@Autowired`
- `@Bean` tells Spring that this method will return an object that should be registered as a bean in the Spring application context.
- `@Import` `@ImportResource`
- `@Controller` `@RestController`
- `@FrameworkEndpoint`