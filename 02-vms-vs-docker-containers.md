# Chapter 2 — Virtual Machines vs Docker Containers (Simple, Practical Comparison)

Before learning Docker, you must understand how it differs from virtual machines (VMs). Both are useful. You choose based on the problem.

## Virtual machine architecture (how VMs work)

A typical VM stack looks like this:

1) Physical or virtual server (your computer or a cloud server)
2) Host Operating System (Windows/macOS/Linux)
3) Hypervisor (VirtualBox, VMware Fusion, ESXi, Hyper‑V, etc.)
4) Virtual machines on top of the hypervisor

The key idea: **each VM has its own guest operating system**.

So you can run:
- Linux VM on Windows host
- Windows VM on macOS host
- etc.

Because each VM has a full OS, each VM behaves like a separate computer:
- its own filesystem
- its own processes
- its own “world”

### Resources in VMs
Typically, you allocate dedicated resources to a VM:
- CPU cores
- RAM
- disk size

Those resources are reserved while the VM is running.

Example (real-world feel): if your laptop has 16GB RAM and you allocate:
- 8GB to a macOS VM
- 4GB to a Windows VM
- 2GB to an Ubuntu VM  
…then you’ve reserved ~14GB RAM just for those VMs.

VMs are great when you need full isolation and a full OS per machine.

## Docker container architecture (how containers work)

A Docker stack looks like this:

1) Physical or virtual server
2) Host OS
3) Docker Engine
4) Containers running on Docker Engine

Main difference: **containers do not have their own guest OS**.

A container is basically:
- a set of files + libraries (its filesystem)
- one or more running processes

It relies on the host OS kernel to run.

### Resources in Docker
Containers share the resources allocated to the Docker host.

In Docker Desktop (macOS/Windows), you configure CPUs/RAM in settings. All containers share those limits.

So if Docker Desktop has 2GB RAM allocated:
- 10 containers together still only have 2GB total RAM available.

## Can Docker run Windows/macOS inside containers?
No. Containers do not contain a full OS. They depend on the host kernel.

So how do containers run on macOS/Windows then?

Answer: Docker Desktop runs a tiny Linux VM, and *containers actually run inside Linux*.

That’s the core reason containers are lightweight compared to VMs.

Next, we’ll go deeper: how Docker runs on Linux, and why Docker Desktop needs a VM.
