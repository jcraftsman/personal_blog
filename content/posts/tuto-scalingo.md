---
title: "Tuto Scalingo"
date: 2020-09-13T00:48:50+02:00
draft: true
---

```bash

curl -O https://cli-dl.scalingo.io/install && bash install
scalingo create secure-quarkus-webapp-toto
scalingo --app secure-quarkus-webapp-toto env-set JAVA_VERSION=11


```

Use PORT environment variable
application.properties

```
quarkus.http.port=${PORT}
%dev.quarkus.http.port=8181
%test.quarkus.http.port=8080
```

Procfile

```
web: java -jar build/secure-quarkus-webapp-my-version-runner.jar
```

build.gradle

```groovy
task stage(dependsOn: ['build', 'clean'])
build.mustRunAfter clean

```


```bash
scalingo --app secure-quarkus-webapp-toto integration-link-create https://github.com/jcraftsman/

secure-quarkus-webapp --auto-deploy --branch master
-----> Your app 'secure-quarkus-webapp-toto' is linked to the repository https://github.com/jcraftsman/secure-quarkus-webapp.

 scalingo --app secure-quarkus-webapp-toto integration-link-manual-deploy master
-----> Manual deployment triggered for app 'secure-quarkus-webapp-toto' on branch 'master'.

scalingo logs
```


https://doc.scalingo.com/platform/deployment/deploy-with-gitlab
https://doc.scalingo.com/platform/deployment/continuous-integration/deploy-scalingo-from-gitlab