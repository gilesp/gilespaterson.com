---
date: 2024-10-14T15:19:21.572Z
publishDate: 2024-10-14T15:19:21.572Z
title: Docker/Colima setup on a Mac
summary: A reminder to myself mostly, on how I setup a colima on my mac to avoid having to use Docker Desktop.
category:
  - macos
  - docker
  - colima
---

I dislike Docker Desktop because I prefer to work on the command line and it's always been more of an inconvenience to me. Plus with the licensing changes it's expensive to use for a lot of companies. Either way, I prefer just using the command line tools so it doesn't give me much benefit.

Instead, I've setup [Colima](https://github.com/abiosoft/colima) and use the docker command line tools around that.

First step was to install docker and docker-compose using brew. This gives me the docker command line tools I need.

    brew install docker docker-compose

Then I installed colima with:

    brew install colima

To make sure colima starts up automatically:

    brew services start colima

As docker compose is a plugin, I need to add some config to `.docker/config.json` so it can find the plugins installed by homebrew. This is done after installing colima, as that will create the config.json file for us.

    "cliPluginsExtraDirs": [
      "/opt/homebrew/lib/docker/cli-plugins"
    ]

Finally, to support other, older docker tools, it's helpful to set the `DOCKER_HOME` environment variable. I added the following to my `.bash_profile`:

    export COLIMA_HOME="$HOME/.colima/"
    export DOCKER_HOST="unix://${COLIMA_HOME}/default/docker.sock"

