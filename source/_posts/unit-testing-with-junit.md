title: Unit testing with junit note 1
date: 2017-12-01 18:10:00
<!-- categories: hexo #unit test -->
tags:
---
### Basic understandings
#### Unit tests
- A unit test targets a small unit of code, e.g., a method or a class. External dependencies should be removed from unit tests, e.g., by replacing the dependency with a test implementation or a **mock** object created by a test framework which will detail
- **Test coverage** is the percentage of code which is tested by unit tests.<br>
#### Integration tests
-  An integration test aims to test the behavior of a component or the integration between a set of components. Also called **functional test**.
- Integration tests check that the whole system works as intended, therefore they are reducing the need for intensive manual tests.<br>
#### Performance tests
- Performance tests are used to benchmark software components repeatedly. Their purpose is to ensure that the code under test runs fast enough even if it’s under high load.<br>
#### Behavior vs. state testing
- State testing is about validating the result. 
- Behavior/Interaction testing is about testing the behavior of the application under test. It does not validate the result of a method call.
- If you are testing algorithms or system functionality, in most cases you may want to test state and not interactions. A typical test setup uses mocks or stubs of related classes to abstract the interactions with these other classes away Afterwards you test the state or the behavior depending on your need

## TBC 12/4/2017