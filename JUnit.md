# JUnit 5

*Unit 5 is most widely used testing framework for java applications. For very long time, JUnit has been doing its job perfectly. JUnit 5 aims to adapt java 8 style of coding and several other features as well, that’s why java 8 is required to create and execute tests in JUnit 5.*

## JUnit 5 Architecture

As compared to JUnit 4, JUnit 5 is composed of several different modules from three different sub-projects:

> **JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage**

1. JUnit Platform
    * To be able to launch junit tests, IDEs, build tools or plugins need to include and extend platform APIs. 
    * It defines the TestEngine API for developing new testing frameworks that runs on the platform.
    * It also provides a Console Launcher to launch the platform from the command line and build plugins for Gradle and Maven.

2. JUnit Jupiter
    * It includes new programming and extension models for writing tests.
    * It has all new junit annotations and TestEngine implementation to run tests written with these annotations.

3. JUnit Vintage
    * It primary purpose is to support running JUnit 3 and JUnit 4 written tests on the JUnit 5 platform.
    * It’s there are backward compatibility.    

## Instalation

You can use JUnit 5 in your maven or gradle project by including minimum two dependencies i.e. Jupiter Engine Dependency and Platform Runner Dependency.

```java

//pom.xml
 
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>${maven.compiler.source}</maven.compiler.target>
    <junit.jupiter.version>5.5.2</junit.jupiter.version>
    <junit.platform.version>1.5.2</junit.platform.version>
</properties>
<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-engine</artifactId>
        <version>${junit.jupiter.version}</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.junit.platform</groupId>
        <artifactId>junit-platform-runner</artifactId>
        <version>${junit.platform.version}</version>
        <scope>test</scope>
    </dependency>
</dependencies>
 
//build.gradle
 
testRuntime("org.junit.jupiter:junit-jupiter-engine:5.5.2")
testRuntime("org.junit.platform:junit-platform-runner:1.5.2")

```

## JUnit 5 Annotations

ANNOTATION | DESCRIPTION
---------- | -----------
@BeforeEach | The annotated method will be run before each test method in the test class.
@AfterEach | The annotated method will be run after each test method in the test class.
@BeforeAll | The annotated method will be run before all test methods in the test class. This method must be static.
@AfterAll | The annotated method will be run after all test methods in the test class. This method must be static.
@Test | It is used to mark a method as junit test.
@DisplayName | Used to provide any custom display name for a test class or test method.
@Disable | It is used to disable or ignore a test class or method from test suite.
@Nested | Used to create nested test classes.
@Tag | Mark test methods or test classes with tags for test discovering and filtering.
@TestFactory | Mark a method is a test factory for dynamic tests.


## Writing Tests in JUnit 5

*There is not much changed between JUnit 4 and JUnit 5 in test writing styles. Here is sample tests with their life cycle methods.*

```java
import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Tag;
import org.junit.jupiter.api.Test;
import com.howtodoinjava.junit5.examples.Calculator;
 
public class AppTest {
     
    @BeforeAll
    static void setup(){
        System.out.println("@BeforeAll executed");
    }
     
    @BeforeEach
    void setupThis(){
        System.out.println("@BeforeEach executed");
    }
     
    @Tag("DEV")
    @Test
    void testCalcOne() 
    {
        System.out.println("======TEST ONE EXECUTED=======");
        Assertions.assertEquals( 4 , Calculator.add(2, 2));
    }
     
    @Tag("PROD")
    @Disabled
    @Test
    void testCalcTwo() 
    {
        System.out.println("======TEST TWO EXECUTED=======");
        Assertions.assertEquals( 6 , Calculator.add(2, 4));
    }
     
    @AfterEach
    void tearThis(){
        System.out.println("@AfterEach executed");
    }
     
    @AfterAll
    static void tear(){
        System.out.println("@AfterAll executed");
    }
}

```

## Test Suites

Using JUnit 5 test suites, you can run tests spread into multiple test classes and different packages.
* JUnit 5 provides two annotations to create test suites: 
    * @SelectPackages 
    * @SelectClasses 

To execute the suite, you will use @RunWith(JUnitPlatform.class):
```java
@RunWith(JUnitPlatform.class)
@SelectPackages("com.howtodoinjava.junit5.examples")
public class JUnit5TestSuiteExample 
{
}
```

Additionally, you can use following annotations for filtering test packages, classes or even test methods.

1. @IncludePackages and @ExcludePackages to filter packages
2. @IncludeClassNamePatterns and @ExcludeClassNamePatterns to filter test classes
3. @IncludeTags and @ExcludeTags to filter test methods

```java
@RunWith(JUnitPlatform.class)
@SelectPackages("com.howtodoinjava.junit5.examples")
@IncludePackages("com.howtodoinjava.junit5.examples.packageC")
@ExcludeTags("PROD")
public class JUnit5TestSuiteExample 
{
}
```

## Assertions

Assertions help in validating the expected output with actual output of a testcase. 
To keep things simple, all JUnit Jupiter assertions are static methods in the org.junit.jupiter.Assertions class e.g. assertEquals(), assertNotEquals().

```java
void testCase() 
{
    //Test will pass
    Assertions.assertNotEquals(3, Calculator.add(2, 2));
      
    //Test will fail 
    Assertions.assertNotEquals(4, Calculator.add(2, 2), "Calculator.add(2, 2) test failed");
      
    //Test will fail 
    Supplier<String> messageSupplier  = ()-> "Calculator.add(2, 2) test failed";
    Assertions.assertNotEquals(4, Calculator.add(2, 2), messageSupplier);
}
```

### JUnit 5 Assertions Examples

JUnit 5 assertions help in validating the expected output with actual output of a testcase. 
To keep things simple, all JUnit Jupiter assertions are static methods in the org.junit.jupiter.Assertions class.

#### Assertions.assertEquals() and Assertions.assertNotEquals() Example

Use Assertions.assertEquals() to assert that expected value and actual value are equal. assertEquals() has many overloaded methods for different data types e.g. int, short, float, char etc. 
It also support passing error message to be printed in case test fails. e.g.

```java
public static void assertEquals(int expected, int actual)
public static void assertEquals(int expected, int actual, String message)
public static void assertEquals(int expected, int actual, Supplier<String< messageSupplier)
```

```java
void testCase() 
{
    //Test will pass
    Assertions.assertEquals(4, Calculator.add(2, 2));
     
    //Test will fail 
    Assertions.assertEquals(3, Calculator.add(2, 2), "Calculator.add(2, 2) test failed");
     
    //Test will fail 
    Supplier&lt;String&gt; messageSupplier  = ()-> "Calculator.add(2, 2) test failed";
    Assertions.assertEquals(3, Calculator.add(2, 2), messageSupplier);
}
```

Assertions.assertNotEquals() method is used to assert that expected value and actual value are NOT equal. 
In contrast to assertEquals(), assertNotEquals() does **not** overloaded methods for different data types but only Object is accepted.

```java
public static void assertNotEquals(Object expected, Object actual)
public static void assertNotEquals(Object expected, Object actual, String message)
public static void assertNotEquals(Object expected, Object actual, Supplier<String> messageSuppl
```

```java
void testCase() 
{
    //Test will pass
    Assertions.assertNotEquals(3, Calculator.add(2, 2));
     
    //Test will fail 
    Assertions.assertNotEquals(4, Calculator.add(2, 2), "Calculator.add(2, 2) test failed");
     
    //Test will fail 
    Supplier&lt;String&gt; messageSupplier  = ()-> "Calculator.add(2, 2) test failed";
    Assertions.assertNotEquals(4, Calculator.add(2, 2), messageSupplier);
}
```

#### Assertions.assertArrayEquals() Example
Similar to assertEquals(), assertArrayEquals() does the same for arrays i.e. asserts that expected and actual arrays are equal. 
It also has overloaded methods for different data types e.g. boolean[], char[], int[] etc. 
It also support passing error message to be printed in case test fails. e.g.

```java
public static void assertArrayEquals(int[] expected, int[] actual)
public static void assertArrayEquals(int[] expected, int[] actual, String message)
public static void assertArrayEquals(int[] expected, int[] actual, Supplier<String> messageSupplier)
```

```java
void testCase() 
{
    //Test will pass
    Assertions.assertArrayEquals(new int[]{1,2,3}, new int[]{1,2,3}, "Array Equal Test");
     
    //Test will fail because element order is different
    Assertions.assertArrayEquals(new int[]{1,2,3}, new int[]{1,3,2}, "Array Equal Test");
     
    //Test will fail because number of elements are different
    Assertions.assertArrayEquals(new int[]{1,2,3}, new int[]{1,2,3,4}, "Array Equal Test");
}
```

#### Assertions.assertIterableEquals() Example

It asserts that expected and actual iterables are deeply equal. 
Deeply equal means that number and order of elements in collection must be same; as well as iterated elements must be equal.

It also has 3 overloaded methods:
```java
public static void assertIterableEquals(Iterable<?> expected, Iterable> actual)
public static void assertIterableEquals(Iterable<?> expected, Iterable> actual, String message)
public static void assertIterableEquals(Iterable<?> expected, Iterable> actual, Supplier<String> messageSupplier)
```

```java
@Test
void testCase() 
{
     Iterable<Integer> listOne = new ArrayList<>(Arrays.asList(1,2,3,4));
     Iterable<Integer> listTwo = new ArrayList<>(Arrays.asList(1,2,3,4));
     Iterable<Integer> listThree = new ArrayList<>(Arrays.asList(1,2,3));
     Iterable<Integer> listFour = new ArrayList<>(Arrays.asList(1,2,4,3));
      
    //Test will pass
    Assertions.assertIterableEquals(listOne, listTwo);
     
    //Test will fail
    Assertions.assertIterableEquals(listOne, listThree);
     
    //Test will fail
    Assertions.assertIterableEquals(listOne, listFour);
}
```


#### Assertions.assertLinesMatch() Example

It asserts that expected list of Strings matches actual list.
* The logic to match a string with another string is :
    1. check if expected.equals(actual) – if yes, continue with next pair
    2. otherwise treat expected as a regular expression and check via String.matches(String) – if yes, continue with next pair
    3. otherwise check if expected line is a fast-forward marker, if yes apply fast-forward actual lines accordingly and goto 1.

A valid fast-forward marker is string which start and end with >> and and contains at least 4 characters. 
Any character between the fast-forward literals are discarded.


#### Assertions.assertNotNull() and Assertions.assertNull() Example

assertNotNull() asserts that **actual is NOT null**. 
assertNull() method asserts that **actual is null**. 
Both has three overloaded methods.

```java
public static void assertNotNull(Object actual)
public static void assertNotNull(Object actual, String message)
public static void assertNotNull(Object actual, Supplier<String> messageSupplier)

public static void assertEquals(Object actual)
public static void assertEquals(Object actual, String message)
public static void assertEquals(Object actual, Supplier<String> messageSupplier)
```

```java
@Test
void testCase() 
{    
    String nullString = null;
    String notNullString = "howtodoinjava.com";
     
    //Test will pass
    Assertions.assertNotNull(notNullString);
     
    //Test will fail
    Assertions.assertNotNull(nullString);
     
    //Test will pass
    Assertions.assertNull(nullString);
 
    // Test will fail
    Assertions.assertNull(notNullString);
}
```


#### Assertions.assertNotSame() and Assertions.assertSame() Example

assertNotSame() asserts that **expected and actual DO NOT refer to the same object.**. 
Similarly, assertSame() method asserts that expected **and actual refer to exactly same object.**. 
Both has three overloaded methods.

```java
public static void assertNotSame(Object actual)
public static void assertNotSame(Object actual, String message)
public static void assertNotSame(Object actual, Supplier<> messageSupplier)

public static void assertSame(Object actual)
public static void assertSame(Object actual, String message)
public static void assertSame(Object actual, Supplier<String> messageSupplier)
```
```java
@Test
void testCase() 
{    
    String originalObject = "howtodoinjava.com";
    String cloneObject = originalObject;
    String otherObject = "example.com";
     
    //Test will pass
    Assertions.assertNotSame(originalObject, otherObject);
     
    //Test will fail
    Assertions.assertNotSame(originalObject, cloneObject);
     
    //Test will pass
    Assertions.assertSame(originalObject, cloneObject);
 
    // Test will fail
    Assertions.assertSame(originalObject, otherObject);
}
```

#### Assertions.assertTimeout() and Assertions.assertTimeoutPreemptively() Example

assertTimeout() and assertTimeoutPreemptively() both are used to test long running tasks. 
If given task inside testcase takes more than specified duration, then test will fail.

Only different between both methods is that in assertTimeoutPreemptively(), execution of the Executable or ThrowingSupplier will be preemptively aborted if the timeout is exceeded. 
In case of assertTimeout(), Executable or ThrowingSupplier will NOT be aborted.

```java
public static void assertTimeout(Duration timeout, Executable executable)
public static void assertTimeout(Duration timeout, Executable executable, String message)
public static void assertTimeout(Duration timeout, Executable executable, Supplier<String> messageSupplier)
public static void assertTimeout(Duration timeout, ThrowingSupplier<T> supplier, String message)
public static void assertTimeout(Duration timeout, ThrowingSupplier<T> supplier, Supplier<String> messageSupplier)
```

```java
@Test
void testCase() {
 
    //This will pass
    Assertions.assertTimeout(Duration.ofMinutes(1), () -> {
        return "result";
    });
     
    //This will fail
    Assertions.assertTimeout(Duration.ofMillis(100), () -> {
        Thread.sleep(200);
        return "result";
    });
     
    //This will fail
    Assertions.assertTimeoutPreemptively(Duration.ofMillis(100), () -> {
        Thread.sleep(200);
        return "result";
    });
}
```

#### Assertions.assertTrue() and Assertions.assertFalse() Example
assertTrue() asserts that the supplied condition is true or boolean condition supplied by BooleanSupplier is true. 
Similarly, assertFalse() asserts that **supplied condition is false**. 
It has following overloaded methods:
```java
public static void assertTrue(boolean condition)
public static void assertTrue(boolean condition, String message)
public static void assertTrue(boolean condition, Supplier<String> messageSupplier)
public static void assertTrue(BooleanSupplier booleanSupplier)
public static void assertTrue(BooleanSupplier booleanSupplier, String message)
public static void assertTrue(BooleanSupplier booleanSupplier, Supplier<String> messageSupplier)

public static void assertFalse(boolean condition)
public static void assertFalse(boolean condition, String message)
public static void assertFalse(boolean condition, Supplier<String> messageSupplier)
public static void assertFalse(BooleanSupplier booleanSupplier)
public static void assertFalse(BooleanSupplier booleanSupplier, String message)
public static void assertFalse(BooleanSupplier booleanSupplier, Supplier<String> messageSupplier)
```

```java
@Test
void testCase() {
 
    boolean trueBool = true;
    boolean falseBool = false;
 
    Assertions.assertTrue(trueBool);
    Assertions.assertTrue(falseBool, "test execution message");
    Assertions.assertTrue(falseBool, AppTest::message);
    Assertions.assertTrue(AppTest::getResult, AppTest::message);
     
    Assertions.assertFalse(falseBool);
    Assertions.assertFalse(trueBool, "test execution message");
    Assertions.assertFalse(trueBool, AppTest::message);
    Assertions.assertFalse(AppTest::getResult, AppTest::message);
}
 
private static String message () {
    return "Test execution result";
}
 
private static boolean getResult () {
    return true;
}
```


#### Assertions.assertThrows() Example

It asserts that execution of the supplied Executable throws an exception of the expectedType and returns the exception.
```java
public static <T extends Throwable> T assertThrows(Class<T> expectedType, Executable executable)
```

```java
@Test
void testCase() {
 
    Throwable exception = Assertions.assertThrows(IllegalArgumentException.class, () -> {
        throw new IllegalArgumentException("error message");
    });
}
```


#### Assertions.fail() Example

fail() method simply fails the test. 
It has following overloaded methods:

```java
public static void fail(String message)
public static void fail(Throwable cause)
public static void fail(String message, Throwable cause)
public static void fail(Supplier<String> messageSupplier)
```

```java
public class AppTest {
    @Test
    void testCase() {
 
        Assertions.fail("not found good reason to pass");
        Assertions.fail(AppTest::message);
    }
     
    private static String message () {
        return "not found good reason to pass";
    }
}
```


## JUnit Best Practices Guide

### Unit testing is not about finding regression bugs

Unit tests are not an effective way to find application wide bugs or detect regressions defects. 
Unit tests, by definition, examine each unit of your code separately. 
But when your application runs for real, all those units have to work together, and the whole is more complex and subtle than the sum of its independently-tested parts. 
Proving that components X and Y both work independently doesn’t prove that they’re compatible with one another or configured correctly.

If you’re trying to find regression bugs, it’s far more effective to actually run the whole application together as it will run in production, just like you naturally do when testing manually. 
If you automate this sort of testing in order to detect breakages when they happen in the future, it’s called integration testing and typically uses different techniques and technologies than unit testing.

> *“Essentially, Unit testing should be seen as part of design process, as it is in TDD (**Test Driven Development**)”. This should be used to support the design process such that designer can identify each smallest module in the system and test it separately.*

####  TDD (Test Driven Development)

Test-driven development (TDD) is a software development process relying on software requirements being converted to test cases before software is fully developed, and tracking all software development by repeatedly testing the software against all test cases. 
This is opposed to software being developed first and test cases created later.
Programmers also apply the concept to improving and debugging legacy code developed with older techniques.

##### Test-driven development cycle
1. Add a test
    - The adding of a new feature begins by writing a test that passes iff(*If and only if*) the feature's specifications are met. 
     -The developer can discover these specifications by asking about use cases and user stories. 
     - A key benefit of test-driven development is that it makes the developer focus on requirements *before* writing code. 
     - This is in contrast with the usual practice, where unit tests are only written *after* code.   

2. Run all tests. The new test *should fail* for expected reasons
    - This shows that new code is actually needed for the desired feature. 
    - It validates that the test harness is working correctly.
    - It rules out the possibility that the new test is flawed and will always pass.

3. Write the simplest code that passes the new test
    - Inelegant or hard code is acceptable, as long as it passes the test. 
    - The code will be honed anyway in Step 5. 
    - No code should be added beyond the tested functionality.

4. All tests should now pass
    - If any fail, the new code must be revised until they pass. 
    - This ensures the new code meets the test requirements and does not break existing features.

5. Refactor as needed, using tests after each refactor to ensure that functionality is preserved
    - Code is refactored for readability and maintainability. 
    - In particular, hard-coded test data should be removed. 
    - Running the test suite after each refactor helps ensure that no existing functionality is broken.

    * Examples of refactoring:
        * moving code to where it most logically belongs
        * removing duplicate code
        * making names self-documenting
        * splitting methods into smaller pieces
        * re-arranging inheritance hierarchies


**Repeat**
The cycle above is repeated for each new piece of functionality. 
Tests should be small and incremental, and commits made often. 
That way, if new code fails some tests, the programmer can simply undo or revert rather than debug excessively. 
When using external libraries, it is important not to write tests that are so small as to effectively test merely the library itself, unless there is some reason to believe that the library is buggy or not feature-rich enough to serve all the needs of the software under development.

##### Keep the unit small
For TDD, a unit is most commonly defined as a class, or a group of related functions often called a module. 
Keeping units relatively small is claimed to provide critical benefits, including:
* Reduced debugging effort – When test failures are detected, having smaller units aids in tracking down errors.
* Self-documenting tests – Small test cases are easier to read and to understand.

https://en.wikipedia.org/wiki/Test-driven_development