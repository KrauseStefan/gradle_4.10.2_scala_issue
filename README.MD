Reproduction of Gradle 4.10.1+ is unable to build a sound war project within a ear project if scala plugin is used
https://github.com/gradle/gradle/issues/7014

Gradle 4.10.1 is unable to build a sound war project within a ear project if Scala plugin is used.

Projects structure:
```
build.gradle (parrent)
|- build.gralde (war)
\- build.gralde (ear)
```
We have 200+ Scala sub projects in the overall project but only 2 sub projects is needed to reproduce.

All projects are configured to apply the Scala plugin from the parrent project

<!--- Provide a brief summary of the issue in the title above -->

### Expected Behavior
I would expect the behavior to be simuler to the one in previous Gradle versions

### Current Behavior
The ear file contains a unexpected `compileScala.mapping` file, and the file has an entry in `applications.xml`

``` xml
  <module>
    <ejb>compileScala.mapping</ejb>
  </module>
```
This causes in our case Wildfly to try to deploy the `compileScala.mapping` and fails at the extraction step.

### Steps to Reproduce (for bugs)
* Clone this repository: https://github.com/KrauseStefan/gradle_4.10.2_scala_issue
* Build
* Deploy to Wildfly (probably also other applications servers)

### Your Environment
Windows 10 1709

I've tested with a few Gradle version but the latest fails also fails
```
$  .\gradlew.bat -version
------------------------------------------------------------
Gradle 4.10.2
------------------------------------------------------------

Build time:   2018-09-19 18:10:15 UTC
Revision:     b4d8d5d170bb4ba516e88d7fe5647e2323d791dd

Kotlin DSL:   1.0-rc-6
Kotlin:       1.2.61
Groovy:       2.4.15
Ant:          Apache Ant(TM) version 1.9.11 compiled on March 23 2018
JVM:          1.8.0_171 (Oracle Corporation 25.171-b11)
OS:           Windows 10 10.0 amd64
```

We are under strict rules not to leak source code.
If I cannot inspect what a build scan publishes I cannot share it, sorry.

