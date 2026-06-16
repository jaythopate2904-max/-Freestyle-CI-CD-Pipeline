# JUnit Reports

## Overview

JUnit reports are generated automatically when running Maven tests. These reports provide detailed information about test execution, results, and coverage.

## Report Location

Test reports are generated in the following directory after running `mvn test`:

```
target/surefire-reports/
```

## Report File Types

### Text Reports (*.txt)

Plain text summary reports for each test class:

```
target/surefire-reports/
├── com.example.CalculatorTest.txt
├── com.example.StringUtilTest.txt
└── ...
```

**Example Content:**
```
Running com.example.CalculatorTest
Tests run: 5, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.042 s
```

### XML Reports (*.xml)

Detailed XML format reports compatible with CI/CD systems and reporting tools:

```
target/surefire-reports/
├── TEST-com.example.CalculatorTest.xml
├── TEST-com.example.StringUtilTest.xml
└── ...
```

**Example XML Structure:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuite name="com.example.CalculatorTest" tests="5" failures="0" errors="0" time="0.042">
  <testcase name="testAddition" classname="com.example.CalculatorTest" time="0.001"/>
  <testcase name="testSubtraction" classname="com.example.CalculatorTest" time="0.001"/>
  <testcase name="testMultiplication" classname="com.example.CalculatorTest" time="0.001">
    <failure message="AssertionError: expected 6 but was 5" type="java.lang.AssertionError">
      <!-- Stack trace -->
    </failure>
  </testcase>
</testsuite>
```

## Viewing Reports

### Console Output

View immediate test results in the Maven console:

```bash
mvn test
```

### Report Directory

Navigate to `target/surefire-reports/` to view detailed reports:

```bash
cd target/surefire-reports/
ls -la
```

### HTML Reports

Generate HTML reports using Maven Site plugin:

```bash
mvn surefire-report:report
mvn site:site
```

HTML reports are generated in `target/site/surefire-report.html`

## Interpreting Reports

### Key Metrics

- **Tests Run**: Total number of test methods executed
- **Failures**: Tests that failed assertions
- **Errors**: Tests that threw unexpected exceptions
- **Skipped**: Tests that were skipped (not executed)
- **Time**: Total execution time in seconds

### Example Report Summary

```
[INFO] -------------------------------------------------------
[INFO]  T E S T S
[INFO] -------------------------------------------------------
[INFO] Running com.example.CalculatorTest
[INFO] Tests run: 5, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.042 s
[INFO] Running com.example.StringUtilTest
[INFO] Tests run: 3, Failures: 1, Errors: 0, Skipped: 0, Time elapsed: 0.015 s
[INFO]
[INFO] Results:
[INFO] Tests run: 8, Failures: 1, Errors: 0, Skipped: 0
[INFO]
[INFO] BUILD FAILURE
```

## CI/CD Integration

### Jenkins Integration

Jenkins can parse XML reports directly:

```groovy
pipeline {
    stage('Test') {
        steps {
            sh 'mvn clean test'
        }
        post {
            always {
                junit 'target/surefire-reports/*.xml'
            }
        }
    }
}
```

### GitHub Actions Integration

GitHub Actions can display test results:

```yaml
- name: Run tests
  run: mvn clean test

- name: Publish test results
  uses: EnricoMi/publish-unit-test-result-action@v2
  if: always()
  with:
    files: target/surefire-reports/*.xml
```

### GitLab CI Integration

```yaml
test:
  script:
    - mvn clean test
  artifacts:
    reports:
      junit: target/surefire-reports/*.xml
```

## Report Analysis

### Identifying Failures

1. Check console output for failed test names
2. Review XML report for detailed failure messages
3. Examine stack traces in XML `<failure>` elements
4. Check test class for assertion logic

### Test Coverage

Generate code coverage reports with JaCoCo:

```bash
mvn clean test jacoco:report
```

Coverage reports are generated in `target/site/jacoco/index.html`

## Best Practices

1. **Run Tests Before Committing**: Verify all tests pass locally
2. **Monitor Test Metrics**: Track failures and errors over time
3. **Automate Report Generation**: Integrate with CI/CD pipeline
4. **Archive Reports**: Keep historical test reports for analysis
5. **Review Failed Tests**: Investigate and fix failing tests immediately
6. **Set Coverage Goals**: Maintain minimum code coverage threshold (70%+)

## Troubleshooting

### Reports Not Generated

Ensure Surefire plugin is configured in `pom.xml`:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.0.0-M7</version>
</plugin>
```

### No Tests Found

Check test file naming:
- Test files should end with `Test.java` or `Tests.java`
- Test methods should be annotated with `@Test`

### Report Parsing Errors in CI/CD

Verify XML report format:

```bash
xmllint target/surefire-reports/TEST-*.xml
```

## Related Commands

- `mvn test` - Run tests and generate reports
- `mvn clean test` - Clean and run tests
- `mvn surefire-report:report` - Generate HTML report
- `mvn site:site` - Generate complete site documentation
