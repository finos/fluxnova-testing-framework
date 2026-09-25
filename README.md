# Fluxnova Testing Framework

## Description

A framework for testing BPMN models

For more usage documentation see:

- [Process Tests](./docs/testing/PROCESS-TESTS.md)
- [Script Tests](./docs/testing/SCRIPT-TESTS.md)

## Getting started

A maven project is required to create and run tests. A new project can be created using the testing framework archetype or by creating one manually.
It is recommended to use the archetype as it comes with all required configurations in place rather than having to configure everything in the project from scratch. It also comes with some sample ready-to-run tests.

### Technologies needed

Ensure Java 21 JDK is installed.
Ensure Maven 3.9.x is installed.
Ensure Nodejs ^18 is installed.

### Generate Project (Quickstart)

By using our archetype you can generate a project to quickly run and write tests

```shell
 mvn archetype:generate                             \
  -DarchetypeGroupId=org.finos.fluxnova.bpm.test  \
  -DarchetypeArtifactId=bpm-testing-archetype       \
  -DarchetypeVersion=<latest-version>               \
  -DgroupId=<your-group-id>                         \
  -DartifactId=<your-artifact-id>
```

### Generate Project (Manual)

To install the framework within your BPMN model testing project update your Maven POM as follows:

#### Import BPM Testing BOM

The `bpm-testing-bom` POM specifies the versions of all the direct and transitive dependencies required to use the framework to run a test suite. Add the `bpm-testing-bom` dependency in the `<dependencyManagement>` section of your POM. The scope `import` indicates that the entire dependency is to be replaced with the list of dependencies listed under the `<dependencyManagement>` tag of the imported POM.

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.finos.fluxnova.bpm.test</groupId>
            <artifactId>fluxnova-bpm-testing-bom</artifactId>
            <version>1.2.0-SNAPSHOT</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

#### Import BPM Testing JAR

The `bpm-testing` JAR contains helper classes and imports all the dependencies required to run the test suite. Add the `bpm-testing` dependency in the `<dependencies>` section of your POM.

```xml
<dependencies>
    <dependency>
        <groupId>org.finos.fluxnova.bpm.test</groupId>
        <artifactId>fluxnova-bpm-testing</artifactId>
        <version>1.2.0-SNAPSHOT</version>
    </dependency>
</dependencies>
```

#### Configure GMavenPlus Plugin

Configure the GMavenPlus plugin to compile your Groovy tests. Ensure that you update the configuration as necessary to include the location of the tests within your project.

```xml
<plugins>
    <plugin>
        <groupId>org.codehaus.gmavenplus</groupId>
        <artifactId>gmavenplus-plugin</artifactId>
        <version>3.0.2</version>
        <configuration>
            <testSources>
                <testSource>
                    <directory>${project.basedir}/src/test/groovy</directory>
                    <includes>
                        <include>**/*.groovy</include>
                    </includes>
                </testSource>
            </testSources>
        </configuration>
        <executions>
            <execution>
                <goals>
                    <goal>compileTests</goal>
                </goals>
            </execution>
        </executions>
    </plugin>
    ...
</plugins>
```

#### Configure Surefire Plugin

Configure the Surefire plugin to be able to run the compiled Groovy tests

```xml
<plugins>
    ...
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>3.5.4</version>
        <configuration>
            <includes>
                <include>**/*Test*.class</include>
            </includes>
        </configuration>
    </plugin>
</plugins>
```

#### Groovy && JS Code Coverage

The testing framework provides functionality to produce code coverage data for inline and external JS and Groovy scripts used within
BPMN models. For JS files, this requires the use of the acornjs library. To ensure that this is set up, use the command `npm install` at
the root of the testing framework.

#### Run Tests

Run `mvn clean test` to run your tests

## License

Copyright FINOS 2026

Distributed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).

SPDX-License-Identifier: [Apache-2.0](https://spdx.org/licenses/Apache-2.0)
