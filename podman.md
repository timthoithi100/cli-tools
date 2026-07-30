Podman (Pod Manager) is a free, open-source, and Linux-native container engine designed to develop, manage, and run Open Container Initiative (OCI) containers and pods. Created by Red Hat, it serves as a lightweight, secure, and drop-in alternative to Docker.

## Key Features

Daemonless Architecture: Podman does not rely on a background server process (like Docker's dockerd). It interacts directly with the Linux kernel via libpod.

Rootless by Default: Users can run containers without needing root privileges, significantly reducing host system security risks.

Kubernetes Integration: Unlike Docker, Podman natively understands "Pods" (groups of containers sharing resources). It can interpret and generate Kubernetes YAML manifests directly.

Docker Compatibility: The syntax is identical to Docker's command-line interface. You can easily transition by setting up a command alias: alias docker=podman.

---

## 1. System & Information

Commands for checking Podman's health, configuration, and disk usage.

* **`podman info`**: Displays detailed information about the Podman installation, storage drivers, and system environment.
* **`podman version`**: Shows the Podman client and engine version details.
* **`podman system df`**: Displays disk space used by containers, images, and volumes.
* **`podman system prune`**: Removes unused data (stopped containers, dangling images, build cache, and unused networks).
* **`podman system prune -a --volumes`**: Performs a deep cleanup, removing **all** unused images and volumes.

---

## 2. Managing Images

Commands for searching, pulling, building, and inspecting container images.

* **`podman search <term>`**: Searches container registries (like Docker Hub or Quay.io) for an image.
* **`podman pull <image>`**: Downloads an image from a registry.
* **`podman images`** (or `podman image ls`): Lists all locally stored images.
* **`podman rmi <image>`** (or `podman image rm <image>`): Deletes a local image.
* **`podman build -t <tag-name> .`**: Builds an image from a `Dockerfile` or `Containerfile` in the current directory.
* **`podman inspect <image>`**: Returns low-level JSON details about an image.
* **`podman history <image>`**: Shows the layers and build history of an image.
* **`podman save -o <file.tar> <image>`**: Saves an image to a tar archive.
* **`podman load -i <file.tar>`**: Loads an image from a tar archive.

---

## 3. Managing Containers

Commands for creating, running, and controlling container lifecycles.

### Running & Lifecycle

* **`podman run -d --name <name> -p <host_port>:<container_port> <image>`**: Runs a container in detached mode (background) with port mapping.
* **`podman run -it <image> /bin/bash`**: Runs a container interactively with a terminal attached.
* **`podman ps`**: Lists running containers.
* **`podman ps -a`**: Lists **all** containers (running and stopped).
* **`podman start <container>`**: Starts one or more stopped containers.
* **`podman stop <container>`**: Gracefully stops a running container.
* **`podman restart <container>`**: Restarts a container.
* **`podman rm <container>`**: Deletes a stopped container.
* **`podman rm -f <container>`**: Force-deletes a running container.

### Monitoring & Interacting

* **`podman logs -f <container>`**: Fetches and streams the live logs of a container.
* **`podman exec -it <container> <command>`**: Executes a command inside a running container (e.g., `podman exec -it my-app bash`).
* **`podman top <container>`**: Displays the running processes of a container.
* **`podman stats`**: Displays a live stream of container resource usage (CPU, memory, network).
* **`podman cp <container>:<path> <host_path>`**: Copies files/folders between a container and the local filesystem.

---

## 4. Pod Management (Podman's Killer Feature)

Unlike Docker, Podman natively supports **Pods** (similar to Kubernetes pods)—a group of one or more containers sharing network, IPC, and storage resources.

* **`podman pod create --name <pod_name> -p <host_port>:<container_port>`**: Creates a new, empty pod.
* **`podman pod ls`**: Lists all active pods.
* **`podman run -d --pod <pod_name> --name <container_name> <image>`**: Runs a container inside an existing pod.
* **`podman pod start <pod_name>`**: Starts all containers inside a pod.
* **`podman pod stop <pod_name>`**: Stops all containers inside a pod.
* **`podman pod rm <pod_name>`**: Removes a pod and its containers.
* **`podman pod top <pod_name>`**: Displays running processes for all containers in a pod.

---

## 5. Kubernetes Integration

Podman makes transitioning to and from Kubernetes seamless.

* **`podman generate kube <pod_or_container> > deployment.yaml`**: Generates Kubernetes YAML manifests from a running pod or container.
* **`podman play kube deployment.yaml`**: Reads a Kubernetes YAML file and runs the containers/pods described in it locally.
* **`podman play kube --down deployment.yaml`**: Tears down the resources created by `play kube`.

---

## 6. Volumes & Networking

Commands for persistent storage and container networking.

### Volumes

* **`podman volume create <vol_name>`**: Creates a new persistent volume.
* **`podman volume ls`**: Lists all volumes.
* **`podman volume inspect <vol_name>`**: Displays detailed info about a volume.
* **`podman volume rm <vol_name>`**: Removes an unused volume.

### Networks

* **`podman network create <net_name>`**: Creates a custom user-defined network.
* **`podman network ls`**: Lists all available networks.
* **`podman network inspect <net_name>`**: Displays network configuration details.
* **`podman network connect <net_name> <container>`**: Connects a container to a network.

---

## 7. Useful Tips & Cheatsheet Shortcuts

> **Pro Tip: Alias Docker to Podman**
> If you're switching from Docker, add this to your `~/.bashrc` or `~/.zshrc`:
> ```bash
> alias docker=podman
> 
> ```
> 
> 

> **Rootless Machine Setup (Podman Desktop / macOS / Windows)**
> If you are on macOS or Windows using Podman Machine:
> * `podman machine init`: Initializes a Linux VM for Podman.
> * `podman machine start`: Starts the Podman VM.
> * `podman machine stop`: Stops the VM.
