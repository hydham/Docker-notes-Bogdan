# Chapter 1 — Welcome to Docker (Crash Course) + Setup

Welcome. In this crash course, we’ll get you productive with Docker fast. We’ll cover the core things you must know to use Docker during development:

Images, containers, building custom images with Dockerfiles, and running multiple services using Docker Compose.

My assumption (prerequisite): you’re comfortable with basic terminal usage, and you understand the idea of a “program running on a computer.” No Docker knowledge required.

## What you need to install

Docker runs differently depending on your operating system.

### If you are on macOS or Windows
Install **Docker Desktop**.

Go to the Docker Desktop download page and install it. After installation, you will see Docker Desktop running.

Important idea: Docker Desktop is not “just an app.” Internally, Docker Desktop runs a **tiny Linux virtual machine**, and containers run inside that Linux VM.

### If you are on Linux
Install **Docker Engine** (not Docker Desktop). On Linux, your Linux machine itself becomes the Docker host (no extra VM needed).

## Docker Desktop settings you should know (macOS/Windows)

Open Docker Desktop → Settings (Preferences).

Under **Resources**, you can configure:
- CPUs
- Memory (RAM)
- Swap
- Disk image size

Very important: these resource limits are shared by all containers you run.  
Example: if you allocate 2GB RAM to Docker Desktop, then *all containers together* must fit inside that 2GB.

You can also open the Docker Desktop dashboard to see which containers are running.

That’s enough setup. Next, we’ll compare Docker containers vs virtual machines so you understand what Docker actually is.
