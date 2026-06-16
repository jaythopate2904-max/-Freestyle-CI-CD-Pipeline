# Build Application

## Overview

Building an application is the process of compiling source code, running tests, and packaging the application into a deployable artifact.

## Build Lifecycle in Maven

Maven follows a standard build lifecycle with predefined phases:

### Main Build Phases

1. **validate** - Validate the project structure
2. **compile** - Compile source code
3. **test** - Run unit tests
4. **package** - Package compiled code (JAR, WAR, etc.)
5. **verify** - Run integration tests
6. **install** - Install package to local repository
7. **deploy** - Deploy to remote repository

## Common Build Commands

### Clean Build

```bash
mvn clean package
```

**What it does:**
- Removes previous build artifacts
- Compiles source code
- Runs unit tests
- Packages the application into a JAR/WAR file

### Build with All Tests

```bash
mvn clean verify
```

**What it does:**
- Runs all phases up to and including verify
- Includes unit tests and integration tests
- Generates test reports

### Build Without Tests

```bash
mvn clean package -DskipTests
```

**What it does:**
- Skips all test execution
- Faster build time for quick iterations

### Install to Local Repository

```bash
mvn clean install
```

**What it does:**
- Builds the application
- Installs it to your local Maven repository (~/.m2/repository/)
- Makes it available as a dependency for other local projects

## Output Artifacts

After a successful build, artifacts are generated in the `target/` directory:

- `target/myapp-1.0-SNAPSHOT.jar` - Compiled JAR file
- `target/myapp-1.0-SNAPSHOT.war` - Web application archive
- `target/surefire-reports/` - Test results and reports
- `target/site/` - Generated documentation

## Build Configuration

Build behavior is configured in `pom.xml`:

```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>myapp</artifactId>
    <version>1.0-SNAPSHOT</version>
    
    <dependencies>
        <!-- Project dependencies -->
    </dependencies>
    
    <build>
        <plugins>
            <!-- Build plugins -->
        </plugins>
    </build>
</project>
```

## CI/CD Integration

In a CI/CD pipeline, the build command typically:

1. Checkout code from repository
2. Run `mvn clean package`
3. Generate build artifacts
4. Deploy artifacts to a repository or server
5. Run integration/smoke tests
6. Notify team of build status

## Troubleshooting

### Build Fails with Compilation Errors

```bash
mvn clean compile
```

Review error messages and fix source code issues.

### Tests Fail

```bash
mvn test
```

Check test reports in `target/surefire-reports/` for details.

### Dependency Issues

```bash
mvn clean install -U
```

The `-U` flag updates dependencies from remote repositories.

## Best Practices

1. **Use Version Control**: Always build from checked-in code
2. **Consistent Environment**: Use same Java/Maven versions
3. **Run Locally First**: Test build locally before pushing
4. **Automated Builds**: Automate builds in CI/CD pipeline
5. **Monitor Build Times**: Keep builds fast for quick feedback
6. **Clean Builds**: Periodically run clean builds to catch issues
