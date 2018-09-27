# docker-php72

## What is this?

This is a custom build based on PHP 7.2's docker image, with changes to make Laravel back-end testing easily possible.

## Where can I find it?

You can find the image on Docker Hub here: https://hub.docker.com/r/nicoverbruggen/php72/.

## GitLab CI

For example, if you are running GitLab, you can use `.gitlab-ci` on your custom GitLab instance:

```
image: nicoverbruggen/php72:latest

cache:
  paths:
  - vendor/
  - node_modules/

tests:
  script:
  - curl -sS https://getcomposer.org/installer | php
  - php composer.phar install
  - yarn install
  - yarn run dev
  - vendor/bin/phpunit -v --configuration phpunit.ci.xml --coverage-text --colors=never
  after_script:
  - cat storage/logs/laravel.log 2>/dev/null
```

This will allow automatic tests of your application to occur.

A few notes:

- Front-end testing w/ Laravel Dusk is not supported in this version.
- This container ships with `npm` and `yarn`.

## How can I build this myself?

Use the Dockerfile, customize it as desired and build it!

Of course, you must replace `nicoverbruggen/php72` with something else if you want to publish your customized version yourself.

    docker build -t nicoverbruggen/php72 .
    docker push nicoverbruggen/php72

If you want to tag the current version (let's say... `1.0`) based on the latest version you just pushed:

    docker image tag nicoverbruggen/php72:latest nicoverbruggen/php72:1.0
    docker push nicoverbruggen/php72:1.0

Anyone can run it afterwards:

    docker run nicoverbruggen/php72

You can also attach to the container w/ bash:

    docker run -i -t nicoverbruggen/php72 /bin/bash
