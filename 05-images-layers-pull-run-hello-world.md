# Chapter 5 — Images, Layers, Pulling, and the Hello-World Container

We start from scratch and learn images practically.

## Cleanup (optional)
If your Docker setup is not clean, you can remove stopped containers:
```bash
docker container prune
```

Remove unused images:
```bash
docker image prune -a
```

After that:
```bash
docker ps -a
docker images
```
should show empty output.

## Pull the first image: hello-world
Docker Hub has an official `hello-world` image.

Pull it:
```bash
docker pull hello-world
```

Check local cache:
```bash
docker images
```

You should see:
- repository: hello-world
- tag: latest
- very small size (~13KB)

## Pull another small image: busybox
Using management-command style:
```bash
docker image pull busybox
docker images
```

## Why pulling shows “layers”
When Docker pulls an image, you may see:
- “pulling fs layer”
- “pull complete”

That’s because images are made of multiple filesystem layers. Docker downloads layers (usually as compressed archives) and extracts them.

Layers are reused across images. If two images share the same base layers, Docker doesn’t download duplicates.

## Run hello-world
Now create a container from the image:
```bash
docker run hello-world
```

You will see a block of text and then you return to the terminal.

Important: the container **stopped** because it finished its only job.

Verify:
```bash
docker ps
# empty
docker ps -a
# shows exited container
```

## Why it printed text, and why it stopped
The image includes a default command to run when a container starts. In `hello-world`, it runs an executable that prints that message and exits.

You can inspect the container to see what command ran:
```bash
docker ps -a
docker inspect <CONTAINER_ID>
```

Look for a field showing the command that was executed.

Next, we go deeper: what a container is internally, and the writable layer.
