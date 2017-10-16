## Eonian Parent POM
Parent POM providing child projects with pipelining capabilities.


</br>## Discrete Pipeline Steps
The plugins that make up these steps are bound to the Maven Lifecycle. So a command like `mvn install` will execute them by default. But when pipelining, we want to execute a series of smaller, more granular steps, and make judgments on whether or not to proceed based on their output. A lifecycle goal like `mvn install` does too much. If the CI job running the command breaks there are several things that could have gone wrong. Could it not compile? Could it not run tests? Did the tests fail? Which tests failed, unit tests or integration tests? So when setting up the child project’s CI pipeline you should create discrete pipeline steps which directly call their corresponding Maven plugins. For more details on building continuous integration pipelines using Maven, see our article <a href="https://medium.com/eonian-technologies/maven-for-pipelining-part-1-8b850d10a7ee" target="_blank">Maven For Pipelining</a>.

### Build
```
mvn -DskipTests clean verify
```

### Unit Test
```
mvn jacoco:prepare-agent surefire:test jacoco:report
```

### Integration Test (JAR projects)
```
mvn jacoco:prepare-agent-integration failsafe:integration-test jacoco:report-integration
```

### Integration Test (WAR projects)
```
mvn jacoco:prepare-agent-integration cargo:start failsafe:integration-test jacoco:dump cargo:stop jacoco:report-integration
```

### Code Analysis
```
mvn sonar:sonar -Dsonar.host.url=URL -Dsonar.login=TOKEN 
```

### Publish Artifacts
```
mvn deploy:deploy-file -DpomFile=pom.xml -Dfile=target/ARTIFACT -Durl=REPO_URL -DrepositoryId=REPO_ID
```