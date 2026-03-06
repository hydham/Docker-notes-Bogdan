# Chapter 12 — Docker Networking: Connect Containers by Name (Custom Bridge Network)

We will run two containers:
1) MongoDB container named `mongo`
2) Python container that connects to `mongodb://mongo:27017`

## Start MongoDB container
Run MongoDB with a fixed name:
```bash
docker run -d --name mongo mongo
```

Docker will pull the `mongo:latest` image if needed (it’s large).

Check:
```bash
docker ps
```

You will see the Mongo container running.

Inspect to see its IP:
```bash
docker inspect mongo
```

## Why it still fails on default bridge network
If you run the Python container now, it may still fail to resolve `mongo` name on the default bridge network.

You can test inside a container:
```bash
docker run -it python-custom sh
ping mongo
# likely fails
```

But pinging the IP might work.

Reason: on default bridge network, name resolution is not automatically handled the way we want for container-to-container by name in this demo.

## Solution: create a custom bridge network
Create network:
```bash
docker network create python-mongo
```

List networks:
```bash
docker network ls
```

You will see `python-mongo` of type bridge.

## Attach Mongo container to the custom network
If Mongo is already running, connect it:
```bash
docker network connect python-mongo mongo
```

(Alternative approach: stop/remove and recreate Mongo with `--network python-mongo`.)

## Run Python container on the same network
Now run Python container attached to the same network:
```bash
docker run -it --network=python-mongo python-custom
```

Now it should work and print database list:
- admin
- config
- local

Because on a custom bridge network, Docker provides DNS so containers can reach each other by name.

You can prove it:
```bash
docker run -it --network=python-mongo python-custom sh
ping mongo
```

This time it should succeed.

## Why names matter
Container IP addresses change. Names are stable (if you set them).  
So we connect by name, not by IP.

Next, we stop doing everything manually and move to Docker Compose to start multiple services with one command.
