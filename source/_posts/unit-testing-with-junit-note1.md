title: Unit Testing Note 1 with JUnit 
date: 2017-06-05 20:00:00
categories: unit-test
tags:
---
This note contains basic understandings of unit test compared to other tests, explains unit testing with JUnit 4 and JUnit 5, mostly on junit 5. Also covers the usage of annotation with example.
### Basic understandings
#### Unit tests
A unit test targets a small unit of code, e.g., a method or a class. External dependencies should be removed from unit tests, e.g., by replacing the dependency with a test implementation or a **mock** object created by a test framework which will detail.
**Test coverage** is the percentage of code which is tested by unit tests.
<!-- more -->
#### Integration tests
An integration test aims to test the behavior of a component or the integration between a set of components. Also called **functional test**.
Integration tests check that the whole system works as intended, therefore they are reducing the need for intensive manual tests.<br>
#### Performance tests
Performance tests are used to benchmark software components repeatedly. Their purpose is to ensure that the code under test runs fast enough even if it’s under high load.<br>
#### Behavior vs. state testing
State testing is about validating the result. 
Behavior/Interaction testing is about testing the behavior of the application under test. It does not validate the result of a method call.
If you are testing algorithms or system functionality, in most cases you may want to test state and not interactions. A typical test setup uses mocks or stubs of related classes to abstract the interactions with these other classes away Afterwards you test the state or the behavior depending on your need.
### Junit
#### Maven/Gradle install and [IDEA support](https://blog.jetbrains.com/idea/2016/08/using-junit-5-in-intellij-idea)

#### Common annotations
All core annotations for JUint are located in the [org.junit.jupiter.api](http://junit.org/junit5/docs/current/api/org/junit/jupiter/api/package-summary.html) package in the `junit-jupiter-api` module. Let's go through each annotation by example.
##### Part 1
- `@Test` denotes that a method is a test method.
- `@Disabled` used to disable a test class or test method; analogous to JUnit 4’s @Ignore.
- `@BeforeEach` denotes that the annotated method should be executed before **each** `@Test`, `@RepeatedTest`, `@ParameterizedTest`, or `@TestFactory` method in the current class; analogous to JUnit 4’s `@Before`. 
- `@AfterEach` is same as `@BeforeEach`, except annotated method executed after.
- `@BeforeAll` denotes that the annotated method should be executed before **all** `@Test`, `@RepeatedTest`, `@ParameterizedTest`, or `@TestFactory` method in the current class; analogous to JUnit 4’s `@BeforeClass`. 
- `@AfterAll` is same as `@BeforeAll`, except annotated method executed after.
```java
import static org.junit.jupiter.api.Assertions.assertEquals;

import org.junit.jupiter.api.Test;

class FirstTests {
    
    @BeforeAll
    static void willCallOnceeBeforeAllTest(){
            System.out.println("BeforeAll");//will be print only once
        }
        
    @BeforeEach
    void willCallBeforeEachTest(){
        System.out.println("BeforeEach");//will executed 2 times
    }
    
    @Test
    void myFirstTest() {        
        System.out.println("test1");
    }
    
    @Test
    void mySecondTest() {        
        System.out.println("test2");
    }
    
    @Test 
    @Disabled("Oh maven test fail? So we should ignore it")
    void failingTest() {
        assertTrue(false);
    }
    
    @AfterEach
    void willCallAfterEachTest(){
        System.out.println("AfterEach");
    }
    
    @AfterAll
    static void willCallOnceeAfterAllTest(){
        System.out.println("AfterAll");
    }
}


```
##### Part 2
- `@RepeatedTest` will let test automatically repeated.
- `@ParameterizedTest` run a test multiple times with different arguments.
- `@DisplayName` declares a custom display name for the test class or test method. Such annotations are not inherited.
```java
import static org.junit.jupiter.api.Assertions.assertEquals;

import java.util.logging.Logger;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.RepeatedTest;
import org.junit.jupiter.api.RepetitionInfo;
import org.junit.jupiter.api.TestInfo;

class RepeatedTestsDemo {
    
    @BeforeEach
    void beforeEach(TestInfo testInfo, RepetitionInfo repetitionInfo) {
        int currentRepetition = repetitionInfo.getCurrentRepetition();
        int totalRepetitions = repetitionInfo.getTotalRepetitions();
        String methodName = testInfo.getTestMethod().get().getName();
        logger.info(String.format("About to execute repetition %d of %d for %s", 
            currentRepetition, totalRepetitions, methodName));
    }
    
    @ParameterizedTest
    @ValueSource(strings = { "racecar", "radar", "able was I ere I saw elba" })
    void palindromes(String candidate) {
        assertTrue(isPalindrome(candidate)); //isPalindrome(racecar),isPalindrome(radar)...
    }

    @RepeatedTest(5)
    void repeatedTestWithRepetitionInfo(RepetitionInfo repetitionInfo) {
        assertEquals(5, repetitionInfo.getTotalRepetitions());// repeat 5 times
    }

    @RepeatedTest(value = 1, name = "{displayName} {currentRepetition}/{totalRepetitions}")
    @DisplayName("Repeat!")
    void customDisplayName(TestInfo testInfo) {
        assertEquals(testInfo.getDisplayName(), "Repeat! 1/1");
    }

    @RepeatedTest(value = 1, name = RepeatedTest.LONG_DISPLAY_NAME)
    @DisplayName("Details...")
    void customDisplayNameWithLongPattern(TestInfo testInfo) {
        assertEquals(testInfo.getDisplayName(), "Details... :: repetition 1 of 1");
    }

    @RepeatedTest(value = 5, name = "Wiederholung {currentRepetition} von {totalRepetitions}")
    void repeatedTestInGerman() {
        // ...
    }

}
```
##### Part 3
- `@TestFactory`
```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertTrue;
import static org.junit.jupiter.api.DynamicContainer.dynamicContainer;
import static org.junit.jupiter.api.DynamicTest.dynamicTest;

import java.util.Arrays;
import java.util.Collection;
import java.util.Iterator;
import java.util.List;
import java.util.Random;
import java.util.function.Function;
import java.util.stream.IntStream;
import java.util.stream.Stream;

import org.junit.jupiter.api.DynamicNode;
import org.junit.jupiter.api.DynamicTest;
import org.junit.jupiter.api.Tag;
import org.junit.jupiter.api.TestFactory;
import org.junit.jupiter.api.function.ThrowingConsumer;

class DynamicTestsDemo {

    // This will result in a JUnitException!
    @TestFactory
    List<String> dynamicTestsWithInvalidReturnType() {
        return Arrays.asList("Hello");
    }

    @TestFactory
    Collection<DynamicTest> dynamicTestsFromCollection() {
        return Arrays.asList(
            dynamicTest("1st dynamic test", () -> assertTrue(true)),
            dynamicTest("2nd dynamic test", () -> assertEquals(4, 2 * 2))
        );
    }

    @TestFactory
    Iterable<DynamicTest> dynamicTestsFromIterable() {
        return Arrays.asList(
            dynamicTest("3rd dynamic test", () -> assertTrue(true)),
            dynamicTest("4th dynamic test", () -> assertEquals(4, 2 * 2))
        );
    }

    @TestFactory
    Iterator<DynamicTest> dynamicTestsFromIterator() {
        return Arrays.asList(
            dynamicTest("5th dynamic test", () -> assertTrue(true)),
            dynamicTest("6th dynamic test", () -> assertEquals(4, 2 * 2))
        ).iterator();
    }

    @TestFactory
    Stream<DynamicTest> dynamicTestsFromStream() {
        return Stream.of("A", "B", "C")
            .map(str -> dynamicTest("test" + str, () -> { /* ... */ }));
    }

    @TestFactory
    Stream<DynamicTest> dynamicTestsFromIntStream() {
        // Generates tests for the first 10 even integers.
        return IntStream.iterate(0, n -> n + 2).limit(10)
            .mapToObj(n -> dynamicTest("test" + n, () -> assertTrue(n % 2 == 0)));
    }

    @TestFactory
    Stream<DynamicTest> generateRandomNumberOfTests() {

        // Generates random positive integers between 0 and 100 until
        // a number evenly divisible by 7 is encountered.
        Iterator<Integer> inputGenerator = new Iterator<Integer>() {

            Random random = new Random();
            int current;

            @Override
            public boolean hasNext() {
                current = random.nextInt(100);
                return current % 7 != 0;
            }

            @Override
            public Integer next() {
                return current;
            }
        };

        // Generates display names like: input:5, input:37, input:85, etc.
        Function<Integer, String> displayNameGenerator = (input) -> "input:" + input;

        // Executes tests based on the current input value.
        ThrowingConsumer<Integer> testExecutor = (input) -> assertTrue(input % 7 != 0);

        // Returns a stream of dynamic tests.
        return DynamicTest.stream(inputGenerator, displayNameGenerator, testExecutor);
    }

    @TestFactory
    Stream<DynamicNode> dynamicTestsWithContainers() {
        return Stream.of("A", "B", "C")
            .map(input -> dynamicContainer("Container " + input, Stream.of(
                dynamicTest("not null", () -> assertNotNull(input)),
                dynamicContainer("properties", Stream.of(
                    dynamicTest("length > 0", () -> assertTrue(input.length() > 0)),
                    dynamicTest("not empty", () -> assertFalse(input.isEmpty()))
                ))
            )));
    }

}
```
#### Test Instance Lifecycle
`@TestInstance(Lifecycle.PER_CLASS)`
tbc