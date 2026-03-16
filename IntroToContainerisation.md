# TryHackMe Room Writeup — Docker & Containerisation

---

## What is Containerisation?

**Containerisation** is the process of packaging an application along with all the necessary resources it requires — such as libraries and packages — into one self-contained package.

Containerisation makes use of the **namespace feature of the kernel**. In a normal configuration, containers cannot communicate with each other.

---

## Docker

- Docker employs the same technology used in containerisation to isolate applications into containers, powered by the **Docker Engine**.
- When an application is published via Docker, it becomes an **image**.
- Docker uses the **YAML** language for its configuration files.

---

## History of Docker

- Containerisation was first introduced in **Unix V7**.
- Docker is open-source and was developed in **2013**, where it was first showcased at **PyCon**.
- It has since become one of the most well-known names in containerisation.

---

## Benefits and Features

Docker simplifies the development and deployment of applications by ensuring consistency across environments, reducing compatibility issues, and making software easy to ship and scale.

---

## How Does it Work?

- The command `ps aux` can be used to view all currently running processes on a system.

---

## Practical

Follow through the site to complete the exercise.

> Flag: `THM{APPLICATION_SHIPPED}`
> 
