title: JUnit Testing Comparation
date: 2017-06-07 20:00:00
categories: unit-test
tags:
---
This note highlight the new feature of JUnit5, cover some understandings of java 8. 
There are some experimental APIS, be cautious. See more on [Experimental APIs](http://junit.org/junit5/docs/current/user-guide/#api-evolution-experimental-apis)
<!-- more -->

Differences between JUnit5 and JUnit4 annotation.

| JUnit 5                                                      | JUnit 4                            | Description                                                  |
| ------------------------------------------------------------ | ---------------------------------- | ------------------------------------------------------------ |
| import org.junit.jupiter.api.*                               | import org.junit.*                 | Import statement for using the following annotations.        |
| @BeforeEach                                                  | @Before                            | Executed before each test. It is used to prepare the test environment (e.g., read input data, initialize the class). |
| @AfterEach                                                   | @After                             | Executed after each test. It is used to cleanup the test environment (e.g., delete temporary data, restore defaults). It can also save memory by cleaning up expensive memory structures. |
| @BeforeAll                                                   | @BeforeClass                       | Executed once, before the start of all tests. It is used to perform time intensive activities, for example, to connect to a database. Methods marked with this annotation need to be defined as static to work with JUnit. |
| @AfterAll                                                    | @AfterClass                        | Executed once, after all tests have been finished. It is used to perform clean-up activities, for example, to disconnect from a database. Methods annotated with this annotation need to be defined as static to work with JUnit. |
| @Disabled                                                    | @Ignore                            | Marks that the test should be disabled. This is useful when the underlying code has been changed and the test case has not yet been adapted. Or if the execution time of this test is too long to be included. It is best practice to provide the optional description, why the test is disabled. |
| Not available, is replaced by AssertTimeout.assertTimeout() and assertTimeoutPreemptively() | @Test(timeout=100)                 | Fails if the method takes longer than 100 milliseconds.      |
| Not available, is replaced by Assertions.expectThrows()      | @Test (expected = Exception.class) | Fails if the method does not throw the named exception.      |