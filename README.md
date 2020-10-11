<!--
  This file was automatically generated by Gradle. Do not modify.
  To update the content of this README, please apply modifications
  to `README.md.template` instead, and run the `generateReadme` task from Gradle.
-->
# android-junit5 [![CircleCI](https://circleci.com/gh/mannodermaus/android-junit5/tree/master.svg?style=svg)][circleci]

![Logo](.images/logo.png)

A Gradle plugin that allows for the execution of [JUnit 5][junit5gh] tests in Android environments using **Android Gradle Plugin 3.5.0 or later.**

## How?

This plugin configures the unit test tasks for each build variant of a project to run on the JUnit Platform. Furthermore, it provides additional configuration options for these tests [through a DSL][wiki-dsl] attached to `android.testOptions`.

Instructions on how to write JUnit 5 tests can be found [in their User Guide][junit5ug].
Furthermore, this repository provides a small showcase of the functionality provided by JUnit 5 [here][sampletests].

## Download

<details open>
  <summary>Kotlin</summary>

  ```kotlin
  buildscript {
    dependencies {
      classpath("de.mannodermaus.gradle.plugins:android-junit5:1.6.2.0")
    }
  }
  ```
</details>

<details>
  <summary>Groovy</summary>
  
  ```groovy
  buildscript {
    dependencies {
      classpath "de.mannodermaus.gradle.plugins:android-junit5:1.6.2.0"
    }
  }
  ```
</details>

<br/>

Snapshots of the development version are available through [Sonatype's `snapshots` repository][sonatyperepo].

## Setup
<details open>
  <summary>Kotlin</summary>

  ```kotlin
  plugins {
    id("de.mannodermaus.android-junit5")
  }

  dependencies {
    // (Required) Writing and executing Unit Tests on the JUnit Platform
    testImplementation("org.junit.jupiter:junit-jupiter-api:5.7.0")
    testRuntimeOnly("org.junit.jupiter:junit-jupiter-engine:5.7.0")

    // (Optional) If you need "Parameterized Tests"
    testImplementation("org.junit.jupiter:junit-jupiter-params:5.7.0")

    // (Optional) If you also have JUnit 4-based tests
    testImplementation("junit:junit:4.13")
    testRuntimeOnly("org.junit.vintage:junit-vintage-engine:5.7.0")
  }
  ```
</details>

<details>
  <summary>Groovy</summary>

  ```groovy
  apply plugin: "de.mannodermaus.android-junit5"

  dependencies {
    // (Required) Writing and executing Unit Tests on the JUnit Platform
    testImplementation "org.junit.jupiter:junit-jupiter-api:5.7.0"
    testRuntimeOnly "org.junit.jupiter:junit-jupiter-engine:5.7.0"

    // (Optional) If you need "Parameterized Tests"
    testImplementation "org.junit.jupiter:junit-jupiter-params:5.7.0"

    // (Optional) If you also have JUnit 4-based tests
    testImplementation "junit:junit:4.13"
    testRuntimeOnly "org.junit.vintage:junit-vintage-engine:5.7.0"
  }
  ```
</details>

<br/>

More information on Getting Started can be found [on the wiki][wiki-gettingstarted].

## Requirements

The latest version of this plugin requires:
* Android Gradle Plugin `3.5.0` or above
* Gradle `6.1.1` or above

## Instrumentation Test Support

There is experimental support for Android instrumentation tests, which requires some additional configuration & dependencies. Furthermore, because JUnit 5 is built on Java 8 from the ground up, its instrumentation tests will only run on devices running Android 8.0 (API 26) or newer. Older phones will skip the execution of these tests completely, marking them as "ignored".

To start writing instrumentation tests with JUnit Jupiter, make the following changes to your module's build script:

<details open>
  <summary>Kotlin</summary>
  
  ```groovy
  android {
    defaultConfig {
      // 1) Make sure to use the AndroidJUnitRunner, or a subclass of it. This requires a dependency on androidx.test:runner, too!
      testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
      // 2) Connect JUnit 5 to the runner
      testInstrumentationRunnerArgument("runnerBuilder", "de.mannodermaus.junit5.AndroidJUnit5Builder")
    }

    // 3) Java 8 is required
    compileOptions {
      setSourceCompatibility(JavaVersion.VERSION_1_8)
      setTargetCompatibility(JavaVersion.VERSION_1_8)
    }
    
    // 4) JUnit 5 will bundle in files with identical paths; exclude them
    packagingOptions {
      exclude("META-INF/LICENSE*")
    }
  }
  dependencies {
    // 5) Jupiter API & Test Runner, if you don't have it already
    androidTestImplementation("androidx.test:runner:1.2.0")
    androidTestImplementation("org.junit.jupiter:junit-jupiter-api:5.7.0")

    // 6) The instrumentation test companion libraries
    androidTestImplementation("de.mannodermaus.junit5:android-test-core:1.2.0")
    androidTestRuntimeOnly("de.mannodermaus.junit5:android-test-runner:1.2.0")
  }
  ```
</details>

<details>
  <summary>Groovy</summary>

  ```groovy
  android {
    defaultConfig {
      // 1) Make sure to use the AndroidJUnitRunner, or a subclass of it. This requires a dependency on androidx.test:runner, too!
      testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
      // 2) Connect JUnit 5 to the runner
      testInstrumentationRunnerArgument "runnerBuilder", "de.mannodermaus.junit5.AndroidJUnit5Builder"
    }

    // 3) Java 8 is required
    compileOptions {
      sourceCompatibility JavaVersion.VERSION_1_8
      targetCompatibility JavaVersion.VERSION_1_8
    }

    // 4) JUnit 5 will bundle in files with identical paths; exclude them
    packagingOptions {
      exclude "META-INF/LICENSE*"
    }
  }

  dependencies {
    // 5) Jupiter API & Test Runner, if you don't have it already
    androidTestImplementation "androidx.test:runner:1.2.0"
    androidTestImplementation "org.junit.jupiter:junit-jupiter-api:5.7.0"

    // 6) The instrumentation test companion libraries
    androidTestImplementation "de.mannodermaus.junit5:android-test-core:1.2.0"
    androidTestRuntimeOnly "de.mannodermaus.junit5:android-test-runner:1.2.0"
  }
  ```
</details>

The `android-test-core` artifact includes an extension point for the `ActivityScenario` API; more information on that can be found in the wiki.

# Official Support

At this time, Google hasn't shared any immediate plans to bring first-party support for JUnit 5 to Android. The following list is an aggregation of pending feature requests:

- [InstantTaskExecutorRule uses @RestrictTo(RestrictTo.Scope.LIBRARY_GROUP) -- why? (issuetracker.google.com)](https://issuetracker.google.com/u/0/issues/79189568)
- [Add support for JUnit 5 (issuetracker.google.com)](https://issuetracker.google.com/issues/127100532)
- [JUnit 5 support (github.com/android/android-test)](https://github.com/android/android-test/issues/224)

# Building Locally

This repository contains multiple modules, divided into two sub-projects. The repository's root directory contains build logic shared across the sub-projects, which in turn use symlinks to connect to the common build scripts in their parent folder.

- `instrumentation`: The root folder for Android-based modules, namely the instrumentation libraries & a sample application. After cloning, open this project in Android Studio.
- `plugin`: The root folder for Java-based modules, namely the Gradle plugin for JUnit 5 on Android, as well as its test module. After cloning, open this project in IntelliJ IDEA.

## License

```
Copyright 2017-2020 Marcel Schnelle

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

See also the [full License text](LICENSE).

 [junit5gh]: https://github.com/junit-team/junit5
 [junit5ug]: https://junit.org/junit5/docs/current/user-guide
 [circleci]: https://circleci.com/gh/mannodermaus/android-junit5
 [sonatyperepo]: https://oss.sonatype.org/content/repositories/snapshots
 [sampletests]: instrumentation/sample
 [wiki-dsl]: https://github.com/mannodermaus/android-junit5/wiki/Configuration-DSL
 [wiki-gettingstarted]: https://github.com/mannodermaus/android-junit5/wiki/Getting-Started
