# Chapter 7 — Port Mapping (Publish Ports) with Nginx

Many containers run services that must be reachable from outside (browser, API client, etc.). For that, we use **port mapping**.

## The problem
A service inside a container listens on an internal container port (example: nginx listens on port 80).

If you run nginx like this:
```bash
docker run -d nginx
```
you cannot access it from your computer, even though nginx is listening on port 80 inside the container.

## Inspect the container to see ports
Run nginx:
```bash
docker run -d nginx
docker ps
```

Inspect:
```bash
docker inspect <NGINX_CONTAINER_ID>
```

You will find that port 80/tcp is exposed inside the container.

But you still cannot reach it from your host because you did not publish it.

## Publish port with -p (hostPort:containerPort)

Stop + remove the old nginx container:
```bash
docker stop <ID>
docker rm <ID>
```

Now run nginx with port mapping:
```bash
docker run -d -p 8080:80 nginx
```

If you see “address already in use”, it means port 8080 is taken on your host. Choose another port:

```bash
docker run -d -p 8081:80 nginx
```

Now check:
```bash
docker ps
```

You will see something like:
`0.0.0.0:8081->80/tcp`

That means:
- host port 8081 is published
- requests are forwarded to container port 80

## Test in browser
Open:
- `http://localhost:8081`

You should see “Welcome to nginx”.

## Notes for Docker Desktop (macOS/Windows)
Even though containers run inside a Linux VM, Docker Desktop forwards the published port to your macOS/Windows host so you can use `localhost`.

Next, we learn environment variables (`-e`) and how apps read them.
