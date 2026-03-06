# Chapter 4 — Docker Components (Client, Server, Host, Images, Containers, Registry)

Now we name the main moving parts.

## Docker Client
When you type `docker ...` in your terminal, you are using the **Docker client**.

Example:
```bash
docker
```

That prints help. You launched the client, it printed output, then exited.

### Example: two equivalent commands
These do the same thing:
```bash
docker ps
docker container ls
```

The second is the “management command style.”

### Docker client can exist without server
If Docker Desktop is stopped, you can still run `docker version` and you will see:
- client info
- server connection error (“cannot connect to docker daemon”)

That proves client is a separate binary on your machine.

## Docker Server (Docker Engine processes)
Docker “server” is not one single process. It’s a set of processes.

Important ones you will see in `docker version`:
- docker daemon
- containerd
- runc
- docker-init

The client talks mainly to the **docker daemon**, which coordinates everything.

## Docker Host
Docker host is the machine where containers actually run.

- On Linux with Docker Engine installed: your Linux machine is the Docker host.
- On macOS/Windows with Docker Desktop: the tiny Linux VM is the Docker host.

### Quick proof of Docker host on Linux
On Linux, you can see a `docker0` network interface:
```bash
ifconfig
# look for docker0
```

Containers get IPs in that docker0 network.

On macOS, you won’t see docker0 in your macOS `ifconfig`, because the host is the Linux VM. If you enter the Docker VM and run `ifconfig`, you’ll see docker0 there.

## Docker Image
An image is a **read-only** set of files (filesystem layers).

List local images:
```bash
docker images
```

Images are used to create containers.

## Docker Container
A container is a running instance created from an image.

- Image: read-only layers
- Container: image layers + one thin writable layer + running process(es)

List running containers:
```bash
docker ps
```

List all (including stopped):
```bash
docker ps -a
```

## Docker repository and tags
A repository is a collection of image versions (tags).

Example: `wordpress` repo can have tags `latest`, `5.4`, etc.

List local images and tags:
```bash
docker images
```

## Docker registry (Docker Hub)
A registry is where repositories live.

Docker Hub is the most popular public registry. You can pull official images without logging in.

Next, we start practicing: pulling images, running containers, and understanding image layers.
