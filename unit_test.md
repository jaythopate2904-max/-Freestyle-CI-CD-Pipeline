# Unit Testing

## Overview

Unit testing is a software testing method where individual components or functions of an application are tested in isolation to verify they work as expected.

## Purpose

- Verify individual functions/methods work correctly
- Catch bugs early in development
- Ensure code reliability and quality
- Facilitate code refactoring with confidence
- Serve as documentation for code behavior

## Testing Frameworks

### Java (Maven Projects)

- **JUnit**: Most popular Java testing framework
- **TestNG**: Alternative testing framework with advanced features
- **Mockito**: Mocking framework for creating mock objects

### Example JUnit Test

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class CalculatorTest {
    
    @Test
    public void testAddition() {
        Calculator calc = new Calculator();
        assertEquals(4, calc.add(2, 2));
    }
    
    @Test
    public void testSubtraction() {
        Calculator calc = new Calculator();
        assertEquals(0, calc.subtract(2, 2));
    }
}
```

## Running Unit Tests with Maven

### Command

```bash
mvn test
```

### What It Does

- Compiles the source code
- Compiles test code
- Runs all tests in `src/test/java/`
- Generates test reports in `target/surefire-reports/`

### Example Output

```
[INFO] Running com.example.CalculatorTest
[INFO] Tests run: 2, Failures: 0, Errors: 0, Skipped: 0
[INFO] BUILD SUCCESS
```

## Test Structure

```
src/
├── main/
│   └── java/
│       └── com/example/
│           └── Calculator.java
└── test/
    └── java/
        └── com/example/
            └── CalculatorTest.java
```

## Best Practices

1. **Test One Thing**: Each test should verify a single behavior
2. **Arrange-Act-Assert**: Structure tests with setup, execution, and verification
3. **Meaningful Names**: Use descriptive test method names (e.g., `testAdditionWithPositiveNumbers`)
4. **Independence**: Tests should not depend on each other
5. **Keep Tests Simple**: Avoid complex logic in tests
6. **Use Assertions**: Verify expected outcomes with assertions
7. **Mock External Dependencies**: Isolate the unit being tested

## Related Maven Commands

- `mvn clean test` - Clean and run all tests
- `mvn test -Dtest=CalculatorTest` - Run a specific test class
- `mvn test -Dtest=CalculatorTest#testAddition` - Run a specific test method
- `mvn test-compile` - Compile tests without running them
