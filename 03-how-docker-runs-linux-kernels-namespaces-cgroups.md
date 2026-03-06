# Chapter 3 — How Docker Runs: Linux Kernel, Namespaces, and cgroups (Beginner-Friendly)

Docker containers are fundamentally a Linux concept. Docker works best on Linux because Linux provides features to isolate processes.

## Docker on Linux (native case)

A Linux machine has:
- CPU
- RAM
- disk
- network
- Linux kernel

When you install Docker Engine on Linux:
- Docker creates containers as isolated process groups.
- Each container gets its own view of filesystem, processes, network, etc.

This isolation comes from **namespaces**.

### Namespaces (simple meaning)
Namespaces let Linux isolate things like:
- processes
- users
- filesystem mounts
- network interfaces

So container A and container B can “feel” like separate machines even though they share the same kernel.

## Resource limits (default: unlimited) and cgroups

By default, a container can try to use up to 100% of host resources.

If you want to limit resources per container, Linux provides **cgroups** (control groups).

With cgroups you can restrict:
- memory usage
- CPU usage
- etc.

## Single responsibility principle in containers (important best practice)

A container should ideally run **one main process**.

Bad approach:
- one container running Python + Redis + mail server all together

Better approach:
- one container runs Python app
- one container runs Redis
- one container runs mail server

Why? Easier to manage, scale, upgrade, and debug.

## Docker on macOS/Windows (why a Linux VM exists)

macOS and Windows don’t provide the same Linux-native container features in the same way.

So Docker Desktop creates a **small Linux VM** and runs containers inside it.  
That Linux VM is the **Docker host** in Docker Desktop setups.

Next, we will define the main Docker components: client, server, host, images, containers, repositories, registries.
