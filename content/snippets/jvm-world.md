---
title: "Jvm World"
date: 2020-01-28T08:28:46+01:00
draft: true
---


## Init new java application with gradle

```bash
gradle init --type java-application --test-framework junit-jupiter
```

java-application build type has the following features:

- Uses the **“application” plugin** to produce a command-line application implemented in Java
- Uses the **“jcenter” dependency repository**
- Uses **JUnit 4 for testing by default**. By supplying `--test-framework junit-jupiter` argument, I tell gradle to **use JUnit Jupiter (aka Junit 5) instead**
- Has directories in the conventional locations for source code
- Contains a sample class and unit test, if there are no existing source or test files

This `gradle init` command provides a starting point. Now you can open the project with your favorite ide.

For jvm world coding, my favorite IDE is IntelliJ. So, at this point all I have to do is:

```bash
idea .
```

If you are missing the `idea` command, checkout how to [enable a command-line launcher](https://www.jetbrains.com/help/idea/working-with-the-ide-features-from-command-line.html).

## Init new Kotlin application with gradle

```bash
gradle init --type kotlin-application
```

If you like kotlin dsl instead of groovy

```bash
gradle init --type kotlin-application --dsl kotlin
```

## Quarkus

<https://quarkus.io/vision/continuum>
<https://www.bookstack.cn/read/quarkus-v1.0-en/37be67e6bac6f800.md>
<https://quarkus.io/guides/kotlin>
<https://code.quarkus.io/>

```bash
mvn io.quarkus:quarkus-maven-plugin:1.7.0.Final:create \
    -DprojectGroupId=my-groupId \
    -DprojectArtifactId=my-artifactId \
    -DprojectVersion=my-version \
    -DclassName="org.my.group.MyResource" \
    -Dpath="/greeting" \
    -Dextensions="kotlin,resteasy-jsonb" \
    -DbuildTool=gradle
```
