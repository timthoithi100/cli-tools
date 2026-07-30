Distrobox is a lightweight command-line tool that allows you to run any Linux distribution inside your terminal by leveraging container engines like Podman or Docker.

While Podman/Docker manage isolated containers, Distrobox acts as a bridge—allowing you to run any Linux distribution inside a container that is **tightly integrated with your host system** (sharing your home directory, audio, display, network, and USB devices).

You can use commands in two formats: `distrobox <subcommand>` or `distrobox-<subcommand>`. Both work identically.

---

## 1. Container Lifecycle Management

Commands for creating, entering, stopping, and deleting Distrobox environments.

* **`distrobox create --name <name> --image <image>`**: Creates a new container from a specific distro image (e.g., `distrobox create -n my-ubuntu -i ubuntu:latest`).
* **`distrobox enter <name>`**: Starts and enters an interactive shell inside the specified container.
* **`distrobox list`**: Lists all active Distrobox containers, their status, and base images.
* **`distrobox stop <name>`**: Stops a running container.
* **`distrobox rm <name>`**: Deletes a container (use `-f` or `--force` to force-delete if running).
* **`distrobox create --clone <existing_box> --name <new_box>`**: Clones an existing container setup into a new one.

---

## 2. Exporting Applications & Binaries (Host Integration)

The killer feature of Distrobox: seamlessly using software installed *inside* the container *directly on your host machine*.

* **`distrobox-export --app <app-name>`**: Exports a GUI application (e.g., installed via `apt` or `pacman`) so it appears in your host system’s application menu/launcher.
* *Example:* Run `distrobox-export --app gimp` inside an Arch container to use GIMP on your Fedora host.


* **`distrobox-export --bin /path/to/bin --export-path ~/.local/bin`**: Exports a command-line binary so you can execute it directly from your host terminal.
* **`distrobox-export --service <service_name>`**: Exports a systemd service from the container to the host.
* **`distrobox-export --app <app-name> --delete`**: Unexports (removes) an exported application or binary from the host system.

---

## 3. Running Commands & Scripting

* **`distrobox enter <name> -- <command>`**: Executes a command inside the container without staying logged in (e.g., `distrobox enter arch-box -- pacman -Syu`).
* **`distrobox-host-exec <command>`**: **Used inside the container** to execute commands on the host OS.
* *Example:* Run `distrobox-host-exec flatpak install ...` while inside the box.


* **`distrobox ephemeral`**: Creates a temporary container, launches a shell, and automatically destroys the container as soon as you exit.

---

## 4. Maintenance & Upgrades

* **`distrobox upgrade <name>`**: Upgrades all packages inside a specific container using its native package manager (e.g., runs `apt update && apt upgrade` for Ubuntu, `pacman -Syu` for Arch).
* **`distrobox upgrade --all`**: Upgrades packages across **all** existing Distrobox containers at once.
* **`distrobox assemble create --file <file.ini>`**: Batch-creates and configures multiple Distrobox containers at once based on a single configuration file.
* **`distrobox assemble rm --file <file.ini>`**: Removes all containers specified in an assembly file.

---

## 5. Advanced Creation Flags

Useful options to append to `distrobox create`:

* **`--home /path/to/custom/home`**: Isolates container configs by using a separate home folder instead of sharing your host's default `$HOME`.
* **`--init`**: Enables systemd/init process inside the container (acts more like a traditional Linux VM).
* **`--nvidia`**: Automatically passes host NVIDIA GPU drivers and libraries into the container.
* **`--additional-packages "pkg1 pkg2"`**: Automatically installs specific packages during container creation.
* **`--root`**: Creates a rootful container (for commands that strictly require true root access).

---

> **Quick Workflow Example**
> ```bash
> # 1. Create an Arch container on your Ubuntu/Fedora host
> distrobox create -n arch-box -i archlinux:latest
> 
> # 2. Enter the box and install an app
> distrobox enter arch-box
> sudo pacman -S vlc
> 
> # 3. Export VLC so it opens from your host's application menu
> distrobox-export --app vlc
> 
> ```