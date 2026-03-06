# Chapter 13 — Docker Compose: Start Multiple Services with One Command

Running many containers with `docker run` gets painful. Docker Compose solves this by describing services in one YAML file and starting everything together.

## What Docker Compose does for you
When you run `docker compose up`, Compose will:
- create a custom network automatically
- build images if you tell it to build
- create containers for each service
- attach them to the network
- enable name-based communication between services

When you run `docker compose down`, Compose removes:
- containers
- the custom network
(and optionally volumes if you request)

## docker-compose.yml example for Python + Mongo

Create a folder (project root) containing:
- `docker-compose.yml`
- `app/` folder with Dockerfile + main.py

Example `docker-compose.yml`:

```yaml
version: "3"
services:
  app:
    build: ./app
  mongo:
    image: mongo
```

Meaning:
- `app` service: build custom image using Dockerfile inside `./app`
- `mongo` service: use official `mongo` image

## Run it
From the folder containing docker-compose.yml:
```bash
docker compose up
```

What you will observe:
- it builds `app` image (if needed)
- it creates a network like `<folder>_default`
- it starts `mongo` container
- it starts `app` container
- `app` prints output then exits (because the script ends)

In another terminal, check running containers:
```bash
docker ps
docker ps -a
```

You will see:
- mongo running
- app exited

## Stop everything
Press Ctrl+C in the terminal running compose.

Then remove containers + network:
```bash
docker compose down
```

Now:
```bash
docker ps -a
docker network ls
```
will show they are gone (custom network removed).

Next, we add ports and volumes in Docker Compose, and we persist MongoDB data.
