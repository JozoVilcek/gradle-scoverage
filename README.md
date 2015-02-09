[![Build Status](https://travis-ci.org/scoverage/gradle-scoverage.png?branch=master)](https://travis-ci.org/scoverage/gradle-scoverage)

gradle-scoverage
================
A plugin to enable the use of Scoverage in a gradle Scala project.

This has now been deployed to maven central.

Using gradle-scoverage
======================

Getting started
---------------
```groovy
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath 'org.scoverage:gradle-scoverage:1.0.8'
    }
}

apply plugin: 'scoverage'

dependencies {
    scoverage 'org.scoverage:scalac-scoverage-plugin_2.11:1.0.4', 'org.scoverage:scalac-scoverage-runtime_2.11:1.0.4'
}
```

This creates an additional task testCoverage which will run tests against instrumented code

- [x] instrumenting main scala code
- [x] running JUnit tests against instrumented scala code
- [x] failing the build on lack of coverage

Then launch command :
`gradle testScoverage` or `gradle checkScoverage`

Available tasks
---------------

* testScoverage - Executes all tests and creates Scoverage XML report with information about code coverage
* reportScoverage - Generates reports (see below).
* aggregateScoverage - Aggregates reports from multiple sub-projects (see below).
* checkScoverage - See below.
* compileScoverageScala - Instruments code without running tests.

ReportScoverage
---------------

You can configure output generated by `gradle reportScoverage` using flags:

| Flag name               | Default value | Description                                     |
| ------------------------|---------------|-------------------------------------------------|
| coverageOutputCobertura | true          | Enables/disables cobertura.xml file generation. |
| coverageOutputXML       | true          | Enables/disables scoverage XML output.          |
| coverageOutputHTML      | true          | Enables/disables scoverage HTML output.         |
| coverageDebug           | false         | Enables/disables scoverage debug output.        |

Aggregating Reports
-------------------

There is now experimental support for aggregating coverage statistics across sub-projects.

The project hosting the aggregation task **must** be configured as the sub-projects are;
i.e. with the scoverage plugin applied and the scoverage dependencies configured.

You also have to declare this task:

```groovy
task aggregateScoverage(type: org.scoverage.ScoverageAggregate)
```

This will produce a report into `build/scoverage-aggregate` directory.

Aggregation uses same flags as reporting for enabling/disabling different output types.

CheckScoverage
--------------

By default, when you launch `gradle checkScoverage` build fail if only 75% of project is covered by tests.

To configure it as you want, add this configuration :
```
checkScoverage {
    minimumLineRate = 0.5
}
```
