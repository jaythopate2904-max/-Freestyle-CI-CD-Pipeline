# Maven Test Command

## Command: mvn test

This Maven command runs all unit tests in the project.

### What It Does

1. **Validates** the project structure
2. **Compiles** source code to `target/classes/`
3. **Compiles** test code to `target/test-classes/`
4. **Executes** all tests in `src/test/java/`
5. **Generates** test reports in `target/surefire-reports/`

### Basic Usage

```bash
mvn test
```

### Run Specific Test Class

```bash
mvn test -Dtest=CalculatorTest
```

### Run Specific Test Method

```bash
mvn test -Dtest=CalculatorTest#testAddition
```

### Run Multiple Test Classes

```bash
mvn test -Dtest=Calculator*,String*
```

### Skip Tests During Build

```bash
mvn clean package -DskipTests
```

## Test Output

### Success Example

```
[INFO] 
[INFO] -------------------------------------------------------
[INFO]  T E S T S
[INFO] -------------------------------------------------------
[INFO] Running com.example.CalculatorTest
[INFO] Tests run: 5, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.042 s
[INFO] 
[INFO] Results:
[INFO] Tests run: 5, Failures: 0, Errors: 0, Skipped: 0
[INFO] 
[INFO] BUILD SUCCESS
```

### Failure Example

```
[INFO] Running com.example.CalculatorTest
[INFO] Tests run: 5, Failures: 1, Errors: 0, Skipped: 0
[ERROR] FAILURE!
[INFO] BUILD FAILURE
```

## Test Reports Location

After running tests, reports are available at:

```
target/surefire-reports/
├── com.example.CalculatorTest.txt
└── TEST-com.example.CalculatorTest.xml
```

## Maven Surefire Plugin Configuration

The Surefire plugin is responsible for running tests. Configure it in `pom.xml`:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.0.0-M7</version>
    <configuration>
        <includes>
            <include>**/*Test.java</include>
            <include>**/*Tests.java</include>
        </includes>
    </configuration>
</plugin>
```

## Test Lifecycle

Tests are part of the standard Maven build lifecycle:

- **test-compile**: Compile test sources
- **test**: Run tests using Surefire plugin
- Runs before: package, install, deploy phases

## Best Practices

1. **Run Tests Regularly**: Execute `mvn test` before committing
2. **Use Descriptive Names**: Test class and method names should indicate what they test
3. **Maintain Test Coverage**: Aim for high code coverage (70%+)
4. **Keep Tests Fast**: Unit tests should run in seconds
5. **Isolate Tests**: Tests should not depend on execution order
6. **Mock External Services**: Use Mockito for external dependencies

## Troubleshooting

### Tests Not Found

```bash
mvn test -e
```

Use `-e` flag for detailed error output.

### Build Cache Issues

```bash
mvn clean test
```

Always use `clean` to remove stale test artifacts.

### Specific Test Fails

```bash
mvn test -Dtest=FailingTest -X
```

Use `-X` for debug output to troubleshoot failing tests.
