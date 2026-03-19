# TryHackMe Room Writeup — Protecting the Docker Daemon

---

## Protecting the Docker Daemon

Developers can interact with other devices running Docker using **SSH authentication**. A weak SSH password can allow an attacker to authenticate and gain access.

The Docker daemon can also be interacted with over **HTTP/S**, which is useful when a web service or application needs to communicate with Docker on a remote device.

### Docker Context Commands

| Command | Description |
|---|---|
| `docker context create` | Creates a new Docker profile/context. |
| `docker context use` | Switches to a specified Docker profile/context. |

---

## Implementing Control Groups (cgroups)

**Control Groups (cgroups)** are a kernel feature that facilitates restricting and prioritising the number of system resources a process can utilise. Implementing cgroups helps achieve **isolation and stability**.

Docker uses **namespaces** to create isolated environments. Namespaces allow different actions to be performed without affecting other processes — think of them as separate rooms in an office.

| Command | Description |
|---|---|
| `--cpus` | Enforces how much CPU a container can use. |
| `docker inspect apache` | Inspects a container named "apache". |

---

## Preventing Over-Privileged Containers

**Privileged containers** have unchecked access to the host and should be avoided.

**Capabilities** are a Linux security feature that determines what processes can and cannot do at a granular level. Traditionally, processes could either have full root privileges or none at all — capabilities provide a middle ground, allowing specific permissions to be granted without full root access.

> It is recommended to assign capabilities to containers **individually** rather than using the `--privileged` flag, which grants all capabilities at once.

| Command | Description |
|---|---|
| `capsh --print` | Displays the capabilities assigned to a process. |
| `--cap-add <CAPABILITY>` | Adds a specific capability to a container. |

**Example capability:**
- `CAP_NET_BIND_SERVICE` — Allows services to bind to ports.

---

## Seccomp & AppArmor 101

### Seccomp

**Seccomp** is an important Linux security feature that restricts the actions a program can and cannot perform. It allows you to create and enforce a list of allowed **system calls** for an application.

Docker already applies a default Seccomp profile at runtime, but this may not be sufficient for more hardened use cases. A custom profile can be created and applied as needed.

> Seccomp operates **within the program itself**, restricting what system calls the process can make (i.e. what parts of the kernel and OS functions it can access).

### AppArmor

AppArmor determines what **resources** an application can access (CPU, memory, network interface, filesystem, etc.) and what **actions** it can take on those resources.

To apply an AppArmor profile to a container:

1. Create an AppArmor profile.
2. Load the profile into AppArmor.
3. Run the container with the new profile applied.

To confirm AppArmor is installed and enabled, look for the output:
```
apparmor module is loaded
```

### Seccomp vs AppArmor

| Feature | Scope |
|---|---|
| **Seccomp** | Restricts system calls a process can make (kernel-level). |
| **AppArmor** | Restricts what resources and actions an application can access (filesystem, network, etc.). |

---

## Reviewing Docker Images

Tools such as **Dive** allow you to reverse engineer Docker images by inspecting what is executed and changed at each layer of the image during the build process.

---

## Using Grype

**Grype** is a vulnerability scanner for container images and filesystems.

### Steps Performed

1. Listed running containers with `docker ps` — the running container was **couchdb**.
2. Scanned the image by running:
   ```bash
   grype struts2 --scope all-layers
   ```
   The library marked as **Critical** was **struts2-core**.
3. Analysed the exported filesystem by running:
   ```bash
   grype /root/container.tar
   ```
   The severity of the finding was listed as **Critical**.
