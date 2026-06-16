# Code Analysis

## Overview

Code analysis is the process of examining source code to identify potential bugs, code smells, security vulnerabilities, and quality issues without executing the program.

## Purpose

- Detect bugs and potential issues early
- Improve code quality and maintainability
- Enforce coding standards and best practices
- Identify security vulnerabilities
- Reduce technical debt
- Measure code metrics (complexity, duplication, coverage)

## Code Analysis Tools

### SonarQube

Industry-leading platform for continuous code quality and security analysis.

**Features:**
- Bug detection and code smell identification
- Security vulnerability scanning
- Code coverage integration
- Duplicate code detection
- Code complexity metrics
- Custom quality gates

### CheckStyle

Enforces coding standards and style guidelines.

**Features:**
- Java coding standards enforcement
- Naming conventions validation
- Code formatting checks
- Documentation verification

### SpotBugs (formerly FindBugs)

Identifies common bug patterns in Java code.

**Features:**
- Potential null pointer dereferences
- Infinite loops
- Resource leaks
- Type mismatches

### PMD

Static analysis tool for source code problems.

**Features:**
- Code smell detection
- Dead code identification
- Duplicate code detection
- Performance optimization suggestions

### JaCoCo

Code coverage measurement tool.

**Features:**
- Line and branch coverage calculation
- Coverage reports (HTML, XML, CSV)
- Coverage verification
- Integration with CI/CD systems

## Analysis in Build Lifecycle

### Maven Integration

Add plugins to `pom.xml` for automated analysis:

```xml
<plugin>
    <groupId>org.sonarsource.scanner.maven</groupId>
    <artifactId>sonar-maven-plugin</artifactId>
    <version>3.9.1.2184</version>
</plugin>

<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-checkstyle-plugin</artifactId>
    <version>3.1.2</version>
</plugin>

<plugin>
    <groupId>com.github.spotbugs</groupId>
    <artifactId>spotbugs-maven-plugin</artifactId>
    <version>4.7.0.0</version>
</plugin>

<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-pmd-plugin</artifactId>
    <version>3.16.0</version>
</plugin>
```

## Analysis Workflow

1. **Local Analysis**: Run analysis on development machine before committing
2. **Pre-commit Checks**: Validate code meets standards
3. **CI/CD Pipeline Analysis**: Run comprehensive analysis on every build
4. **Quality Gates**: Define pass/fail criteria for code quality
5. **Reports Review**: Examine analysis results and address issues
6. **Metrics Tracking**: Monitor quality trends over time

## Common Commands

### Run All Code Analysis

```bash
mvn clean verify
```

### Run SonarQube Analysis

```bash
mvn sonar:sonar
```

### Run CheckStyle Validation

```bash
mvn checkstyle:check
```

### Run SpotBugs Analysis

```bash
mvn spotbugs:check
```

### Run PMD Analysis

```bash
mvn pmd:check
```

### Generate Code Coverage

```bash
mvn clean test jacoco:report
```

## Analysis Reports

Reports are generated in `target/` directory:

```
target/
├── sonar/                          # SonarQube analysis
├── checkstyle-result.xml           # CheckStyle violations
├── spotbugsXml.xml                 # SpotBugs findings
├── pmd.xml                         # PMD violations
├── site/jacoco/index.html          # Code coverage report
└── surefire-reports/               # Test results
```

## Quality Gates

### SonarQube Quality Gate Example

```
✓ Coverage >= 80%
✓ Duplicated Lines Density <= 3%
✓ Maintainability Rating >= A
✓ Reliability Rating >= A
✓ Security Rating >= A
✗ Critical Issues = 0
```

## Best Practices

1. **Run Analysis Frequently**: Execute before committing code
2. **Fix Critical Issues First**: Address bugs and vulnerabilities immediately
3. **Maintain Coverage Target**: Keep code coverage above 80%
4. **Enforce Standards**: Use quality gates to prevent poor code merging
5. **Monitor Trends**: Track metrics over time to identify patterns
6. **Address Technical Debt**: Dedicate time to fixing code issues
7. **Review Reports**: Examine analysis results and learn from issues

## CI/CD Integration

### Jenkins Pipeline

```groovy
stage('Code Analysis') {
    steps {
        sh 'mvn clean verify sonar:sonar'
    }
    post {
        always {
            publishHTML([
                reportDir: 'target/site',
                reportFiles: 'index.html',
                reportName: 'SonarQube Report'
            ])
        }
    }
}
```

### GitHub Actions

```yaml
- name: Code Analysis
  run: mvn clean verify sonar:sonar
```

## Troubleshooting

### SonarQube Server Not Reachable

Verify server URL in properties:

```bash
mvn sonar:sonar -Dsonar.host.url=http://localhost:9000
```

### Analysis Takes Too Long

Optimize by skipping certain checks:

```bash
mvn sonar:sonar -Dsonar.skip=true
```

### Quality Gate Failures

Review SonarQube dashboard for specific violations and fix code accordingly.
