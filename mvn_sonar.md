# Maven SonarQube Command

## Command: mvn sonar:sonar

This Maven command runs static code analysis using SonarQube.

### What It Does

1. **Analyzes** source code for bugs and code smells
2. **Scans** for security vulnerabilities
3. **Measures** code complexity and maintainability
4. **Detects** duplicate code
5. **Calculates** code coverage metrics
6. **Uploads** results to SonarQube server

### Basic Usage

```bash
mvn sonar:sonar
```

### With Custom SonarQube Server

```bash
mvn sonar:sonar -Dsonar.host.url=http://sonarqube-server:9000
```

### With Authentication

```bash
mvn sonar:sonar \
  -Dsonar.host.url=http://sonarqube-server:9000 \
  -Dsonar.login=your_token
```

### With Project Key

```bash
mvn sonar:sonar -Dsonar.projectKey=my-project
```

## SonarQube Analysis Output

### Console Output Example

```
[INFO] SonarQube Scanner for Maven 3.9.1.2184
[INFO] Java 11.0.13 AdoptOpenJDK (64-bit)
[INFO] Scanning for projects...
[INFO] 
[INFO] --------< com.example:myapp >--------
[INFO] Building myapp 1.0-SNAPSHOT
[INFO] --------[ jar ]--------
[INFO] 
[INFO] --- sonar-maven-plugin:3.9.1.2184:sonar (default-cli) @ myapp ---
[INFO] User cache: /home/user/.sonar/cache
[INFO] SonarQube Scanner 4.6.1.2450
[INFO] Analysis of project MyApp
[INFO] Base dir: /path/to/project
[INFO] Source paths: src/main/java
[INFO] Test paths: src/test/java
[INFO] Binary dirs: target/classes
[INFO] 
[INFO] 1 file indexed
[INFO] 0 tests included
[INFO] 
[INFO] Sensor JavaXmlSensor [java]
[INFO] Parsing XML files
[INFO] 1/1 source files have been analyzed
[INFO] ANALYSIS SUCCESSFUL, you can browse http://sonarqube-server:9000/dashboard?id=myapp
[INFO] BUILD SUCCESS
```

## Prerequisites

### SonarQube Server

Ensure SonarQube server is running and accessible:

```bash
# Default SonarQube URL
http://localhost:9000
```

### Maven Configuration

Add SonarQube plugin to `pom.xml`:

```xml
<plugin>
    <groupId>org.sonarsource.scanner.maven</groupId>
    <artifactId>sonar-maven-plugin</artifactId>
    <version>3.9.1.2184</version>
</plugin>
```

### Code Coverage Integration

Include JaCoCo for coverage metrics:

```xml
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.7</version>
    <executions>
        <execution>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <phase>test</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

## Analysis Metrics

SonarQube measures:

- **Bugs**: Code defects that could cause failures
- **Code Smells**: Design and maintainability issues
- **Security Issues**: Potential security vulnerabilities
- **Coverage**: Percentage of code covered by tests
- **Duplications**: Duplicated code blocks
- **Complexity**: Cyclomatic complexity of code
- **Technical Debt**: Effort to fix all issues

## Quality Gates

Quality gates define pass/fail criteria:

```
✓ Coverage >= 80%
✓ Duplicated Lines Density <= 3%
✓ Maintainability Rating >= A
✓ Reliability Rating >= A
✓ Security Rating >= A
```

## CI/CD Integration

### Jenkins Pipeline

```groovy
stage('Code Analysis') {
    steps {
        sh 'mvn clean test sonar:sonar'
    }
}
```

### GitHub Actions

```yaml
- name: SonarQube Analysis
  run: mvn clean verify sonar:sonar
  env:
    SONAR_HOST_URL: http://sonarqube-server:9000
    SONAR_LOGIN: ${{ secrets.SONAR_TOKEN }}
```

### GitLab CI

```yaml
sonarqube:
  script:
    - mvn clean verify sonar:sonar
  variables:
    SONAR_HOST_URL: http://sonarqube-server:9000
    SONAR_LOGIN: $CI_JOB_TOKEN
```

## Common Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `sonar.host.url` | SonarQube server URL | `http://localhost:9000` |
| `sonar.login` | Authentication token | `squ_xxxxxxxxxxxxx` |
| `sonar.projectKey` | Project key | `my-project` |
| `sonar.projectName` | Project name | `My Project` |
| `sonar.sources` | Source directories | `src/main/java` |
| `sonar.tests` | Test directories | `src/test/java` |
| `sonar.coverage.jacoco.xmlReportPaths` | JaCoCo report path | `target/site/jacoco/jacoco.xml` |

## Troubleshooting

### SonarQube Server Connection Error

```
ERROR: Unable to execute SonarQube scanner
```

**Solution**: Verify server URL is correct and accessible

```bash
curl http://localhost:9000/api/system/status
```

### Authentication Failed

```
ERROR: Authentication failed
```

**Solution**: Verify SonarQube token is valid

```bash
mvn sonar:sonar -Dsonar.login=correct_token
```

### Analysis Takes Too Long

Optimize by excluding test files:

```bash
mvn sonar:sonar -Dsonar.exclusions=**/*Test.java
```

### No Coverage Data

Ensure JaCoCo is configured and `mvn test` runs before analysis:

```bash
mvn clean test sonar:sonar
```

## Best Practices

1. **Run Analysis Regularly**: Include in CI/CD pipeline
2. **Fix Critical Issues**: Address bugs and vulnerabilities immediately
3. **Monitor Quality Gates**: Prevent low-quality code from merging
4. **Maintain Coverage**: Keep coverage above 80%
5. **Review Reports**: Regularly check SonarQube dashboard
6. **Track Trends**: Monitor code quality metrics over time
7. **Automate Analysis**: Run on every commit in CI/CD

## Related Commands

- `mvn clean verify sonar:sonar` - Clean, test, and analyze
- `mvn sonar:sonar -Dsonar.skip=false` - Force analysis
- `mvn sonar:help` - Get SonarQube plugin help
