# minecraft-bedrock
Docker setup for private Bedrock Minecraft server.


## Docker Compose
The following file uses many of the default values, but does store the persistent world data in a local folder on the host under `/data-survival-aaron`.

```docker
name: minecraft-survival-aaron
services:
  bedrock:
    image: itzg/minecraft-bedrock-server
    container_name: minecraft-survival-aaron
    environment:
      EULA: "TRUE"
      GAMEMODE: survival
      DIFFICULTY: normal
      DUMP_SERVER_PROPERTIES: "true"
      DEFAULT_PLAYER_PERMISSION_LEVEL: member
      ALLOW_CHEATS: "false"
      SERVER_NAME: "Survival-Aaron"
      LEVEL_NAME: "SurvivalAaron"
      TICK_DISTANCE: 4
      VIEW_DISTANCE: 32
      PLAYER_IDLE_TIMEOUT: 30
      MAX_THREADS: 8
      ALLOW_LIST: "true"
      ALLOW_LIST_USERS: "aaronmorgan"
    ports:
      - "19132:19132/udp"
    volumes:
      - ./data-survival-aaron:/data
    restart: unless-stopped
    stdin_open: true
    tty: true
```

## Optional Settings Changes

When the Docker container is up and running we might want to customize some of the server settings using administrator/cheat commands.

Use the following command to attach to the running container.

```bash
docker attach CONTAINER_NAME_OR_ID
```

This will open a prompt where admin commands can be entered, e.g.

* time set day|noon|...
* gamerule doDaylightCycle false
* gamerule showcoordinates true

## Updating the Minecraft Server

Simple bash script to update and restart the server.

```bash
#!/bin/bash

DOCKER_COMPOSE_FILE="docker-compose-survival-aaron.yml "

docker compose -f $DOCKER_COMPOSE_FILE down

docker compose -f $DOCKER_COMPOSE_FILE pull

docker compose -f $DOCKER_COMPOSE_FILE up -d

```