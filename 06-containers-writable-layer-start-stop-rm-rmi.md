# Chapter 6 — Containers: Writable Layer, Processes, Start/Stop, Remove

Now I want you to understand containers as: files + processes.

## Image vs container (key mental model)

An image has read-only layers.

When you run a container, Docker adds a small **writable layer** on top.

That’s why:
- image is read-only
- container filesystem is writable

## A container runs while a process runs
A container is considered “running” as long as at least one main process is running inside it.

When that process ends, the container stops.

That’s why hello-world stops instantly.

## Run multiple containers from the same image
You can create multiple containers from one image.

Example:
```bash
docker run -it busybox
# (container A)

docker run -it busybox
# (container B)
```

Both use the same image layers. Each gets its own writable layer.

## Modify a container (create a file)
Inside a running busybox container:
```bash
touch file.txt
ls
```

That file exists in the container’s writable layer.

## List and remove containers
List running:
```bash
docker ps
```

List all:
```bash
docker ps -a
```

Stop a running container:
```bash
docker stop <ID_OR_NAME>
```

Remove a stopped container:
```bash
docker rm <ID_OR_NAME>
```

Important detail: Docker stop sends SIGTERM first.  
If the process does not exit, Docker will fall back to SIGKILL after ~10 seconds.

## Remove images
Remove an image from local cache:
```bash
docker rmi <IMAGE_NAME_OR_ID>
```

If containers exist that use that image, you must remove those containers first.

Next, we learn port mapping (publish ports) so we can access container apps from our browser.
