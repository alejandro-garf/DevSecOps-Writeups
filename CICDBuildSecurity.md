# TryHackMe Room Writeup — CI/CD and Build Security

**Room:** https://tryhackme.com/room/cicdandbuildsecurity

---

## CI/CD Pipeline Fundamentals

- The **build orchestrator** coordinates and manages the automation of build and deployment environments.
- **Build agents** are responsible for building, testing, and packaging code.
- **Maximum visibility** is the CI/CD fundamental that promotes developers having access to the latest builds and code in order to understand and track changes.
- The build agent used with GitLab is the **GitLab Runner**.

---

## Authentication & Access

- Flag received after authenticating to Timekeep: `THM{Welcome.to.CICD.Pipelines}`

---

## Source Code & Version Control Security

- The **`.gitignore`** file specifies which directories and files should be excluded from version control.
- **Branches** should be protected to ensure direct pushes and vulnerable code changes are avoided.
- A lack of access control and unauthorised code changes leads to **unauthorised tampering**.
- The API key stored within the mobile application accessible by any GitLab user: `THM{You.Found.The.API.Key}`

---

## Artefact & Dependency Security

- Artefacts should be stored in a **secure registry** to prevent tampering.
- **Secret management** should always be used to store and inject sensitive data.
- Malicious actors can perform a **dependency confusion** attack to inject malicious code into the build process.

---

## Build Server Security

- Flag 1: `THM{7753f7e9-6543-4914-90ad-7153609831c3}`
- A **VPN** can be used to ensure that remote access to the build server is performed securely.
- **Token-based authentication** can be used to add an additional layer of authentication security for build agents.
- Flag 2: `THM{1769f776-e03c-40b6-b2eb-b298297c15cc}`

---

## Pipeline Hardening

- **Merge requests** should be added so that code changes are raised for review instead of being pushed directly.
- **Limiting runner access** ensures that only trusted runners execute CI/CD jobs.
- Flag 3: `THM{2411b26f-b213-462e-b94c-39d974e503e6}`

---

## Environment Isolation

- **Isolating environments** ensures that a compromised environment does not affect other environments.
- Flag 4 (DEV environment): `THM{28f36e4a-7c35-4e4d-bede-be698ddf0883}`
- Flag 5 (PROD environment): `THM{e9f99dbe-6bae-4849-adf7-18a449c93fe6}`

---

## Secrets Management

- Using environment variables alone is **not** enough to protect build secrets.
- PROD API_KEY: `THM{Secrets.are.meant.to.be.kept.Secret}`
