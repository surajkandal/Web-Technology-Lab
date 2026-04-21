# Assignment 11: Build Automation with Maven and Gradle

This project demonstrates how the same Spring Boot application can be managed using two different build tools: Maven and Gradle.

## 1. Maven Build (mvnw)
Maven uses the `pom.xml` file for project configuration.

### Run the Application
```bash
./mvnw.cmd spring-boot:run
```

### Build the Executable JAR
```bash
./mvnw.cmd clean package
```

---

## 2. Gradle Build (build.gradle)
Gradle uses the `build.gradle` file for project configuration.

### Run the Application
```bash
./gradlew bootRun
```

### Build the Executable JAR
```bash
./gradlew build
```

---

## Key Benefits of Build Automation
- **Consistency**: Ensures the project builds the same way on every machine.
- **Dependency Management**: Automatically downloads and manages library dependencies.
- **Lifecycle Management**: Handles clean, compile, test, and package phases automatically.
