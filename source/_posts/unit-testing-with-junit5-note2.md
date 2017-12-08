title: Unit Testing Note 2 with JUnit5
date: 2017-12-04 18:10:00
<!-- categories: hexo #unit test -->
tags:
---
This note highlight the new feature of JUnit5, cover some understandings of java 8. 
There are some experimental APIS, be cautious. See more on [Experimental APIs](http://junit.org/junit5/docs/current/user-guide/#api-evolution-experimental-apis)
<!-- more -->
why blank?
<table>
  <tr>
    <th>JUnit 5</th>
    <th>JUnit 4</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>import org.junit.jupiter.api.*</td>
    <td>import org.junit.*</td>
    <td>Import statement for using the following annotations.</td>
  </tr>
  <tr>
    <td>@BeforeEach</td>
    <td>@Before</td>
    <td>Executed before each test. It is used to prepare the test environment (e.g., read input data, initialize the class).</td>
  <tr>
    <td>@AfterEach</td>
    <td>@After</td>
    <td>Executed after each test. It is used to cleanup the test environment (e.g., delete temporary data, restore defaults). It can also save memory by cleaning up expensive memory structures.</td>
  </tr>
  <tr>
      <td>@BeforeAll</td>
      <td>@BeforeClass</td>
      <td>Executed once, before the start of all tests. It is used to perform time intensive activities, for example, to connect to a database. Methods marked with this annotation need to be defined as static to work with JUnit.</td>  
  </tr>
  <tr>
      <td>@AfterAll</td>
      <td>@AfterClass</td>
      <td>Executed once, after all tests have been finished. It is used to perform clean-up activities, for example, to disconnect from a database. Methods annotated with this annotation need to be defined as static to work with JUnit.</td>
  </tr>
  <tr>
      <td>@Disabled</td>
      <td>@Ignore</td>
      <td>Marks that the test should be disabled. This is useful when the underlying code has been changed and the test case has not yet been adapted. Or if the execution time of this test is too long to be included. It is best practice to provide the optional description, why the test is disabled.</td>
  </tr>
  <tr>
      <td>Not available, is replaced by AssertTimeout.assertTimeout() and assertTimeoutPreemptively()</td>
      <td>@Test(timeout=100)</td>
      <td>Fails if the method takes longer than 100 milliseconds.</td>
  </tr>
  <tr>
      <td>Not available, is replaced by Assertions.expectThrows()</td>
      <td>@Test (expected = Exception.class)</td>
      <td>Fails if the method does not throw the named exception.</td>
  </tr>
</table>