---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: CLI in Docker
description: The data is passed through the application constantly. Multiple application parts are responsible for this data processing. Learn the main actors, data-pipes and conditions in this guide!

# Micro navigation
micro_nav:
  enabled: true
  url: '/docs/docker'
  title: Docker

# Page navigation
page_nav:
    prev:
        content: Service overview
        url: '/docs/docker/services'
    next:
        content: Environment configuration
        url: '/docs/docker/configuration'

---

## Watch video

<div class="video">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/-RWQB4US4tg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

## Pro Tip: use aliases

```bash
# use `dc` to start without `frontend` container
alias dc="docker-compose -f docker-compose.yml -f docker-compose.local.yml -f docker-compose.ssl.yml"

# use `dcf` to start with `frontend` container
alias dcf="docker-compose -f docker-compose.yml -f docker-compose.local.yml -f docker-compose.ssl.yml -f docker-compose.frontend.yml"

# use `inapp` to quickly get inside of the app container
alias inapp="docker-compose -f docker-compose.yml -f docker-compose.local.yml -f docker-compose.ssl.yml -f docker-compose.frontend.yml exec -u user app"

# use `applogs` to quickly see the last 100 lines of app container logs
alias applogs="docker-compose logs -f --tail=100 app"

# use `frontlogs` to quickly see the last 100 lines of frontend container logs
alias frontlogs="docker-compose -f docker-compose.yml -f docker-compose.local.yml -f docker-compose.ssl.yml -f docker-compose.frontend.yml logs -f --tail=100 frontend"
```

## Stack operations

Start and create containers `docker-compose up -d`

Start - `docker-compose start`

Restart - `docker-compose restart`

Stop - `docker-compose stop`

Stop and remove - `docker-compose down`

Pull latest containers from repos - `docker-compose pull`

Check status - `docker-compose ps`

List open ports - `docker-compose ports`

## App container debug mode

During container build you might need to execute indide it, but due to errors it bootloops.

Here is how to override it:

-   Stop app container
    `docker-compose stop app`
-   Execute the app container `docker run -it -e COMPOSER_AUTH --entrypoint /bin/bash APP_CONTAINER_NAME`

## Logging

Logs can be accessed with `docker-compose logs -f` to fetch all logs from all stack.
To watch specific container logs - `docker-compose logs -f app` can use multiple container names, separated by space

## Shell access

To execute into application `docker-compose exec -u user app bash -l`, it runs default Ubuntu 16.04.
Other services in stack are using default ports and open to host, you can use any tools to interact with them directly
Service containers run on Alpine local, use `docker-compose exec mysql /bin/sh -l`

Note the flag `-l`, passing it you will make "normal" login into shell, with proper expose of updated $PATH and other variables in the system

## Full rebuild with log attach

`docker-compose -f docker-compose.yml -f docker-compose.local.yml up -d --force-recreate --remove-orphans --build && docker-compose logs -f`

## Rebuild app container without cache

`docker-compose -f docker-compose.yml -f docker-compose.local.yml build --no-cache app`

## Reference

- <https://docs.docker.com/compose/reference/overview/> - docker-compose cli overview and reference
- <https://docs.docker.com/engine/reference/commandline/cli/> - docker cli overview and reference
