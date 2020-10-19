---
title: "Ci Travis Heroku"
date: 2020-02-26T09:42:55+01:00
draft: true
---


git subtree push --prefix path/to/subdirectory heroku master


https://medium.com/@shalandy/deploy-git-subdirectory-to-heroku-ea05e95fce1f

https://docs.travis-ci.com/user/deployment/heroku/

```bash
brew install travis
brew install heroku
```

https://docs.travis-ci.com/user/deployment/heroku/

```bash
travis encrypt $(heroku auth:token) --add deploy.api_key
```

builde.gradle

```groovy
task stage {
    dependsOn installDist
}
```

Procfile

.travis.yml

```yml
language: java
script: cd booking && ./gradlew clean build
deploy:
  provider: heroku
  api_key:
    secure: EncRypt3dKEYwilLbeGen3R4teDhEre
  app:
    master: hotel-modeling-booking
```