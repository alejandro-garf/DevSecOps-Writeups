# TryHackMe Room Writeup — Intro to Kubernetes

---

## Kubernetes 101

**Kubernetes** is a container orchestration system that makes applications **highly available and scalable**. It achieves this by, for example, duplicating an application and its database component, then load balancing external requests to available resources. Kubernetes can run virtually anywhere, making it highly portable.

---

## Kubernetes Architecture

### Core Concepts

- **Pods** are the smallest deployable unit of computing in Kubernetes. A pod is a group of one or more containers. Containers on the same pod can communicate easily as if they were on the same machine, while maintaining a degree of isolation.
- **Nodes** contain all the services necessary to run pods.
- A **Cluster** is simply a set of nodes.

### Control Plane vs Worker Nodes

- The **control plane** (also known as the master node) manages the worker nodes and pods in the cluster.
- The **kube-scheduler** actively monitors the cluster — its job is to catch any newly created pods that have not yet been assigned to a node and ensure they get assigned to one.
- **etcd** acts as an information store that the other control plane components query for information such as available resources.
- **Worker nodes** are responsible for maintaining running pods.

---

## Kubernetes Landscape

| Resource | Description |
|---|---|
| **Namespace** | Used to isolate groups of resources within a single cluster. |
| **ReplicaSet** | Maintains a set of replica pods and guarantees the availability of a specified number of identical pods. Acts as a backup to ensure availability. |
| **Ingress** | Acts as a single access point to the cluster with all routing rules defined in one resource. |
| **Service** | Exposes a Kubernetes application to an access point. |

---

## Kubernetes Config

- Two configuration files are typically required: one for the **deployment** and one for the **service**.
- Config files are usually written in **YAML** (best practice), though JSON is also supported.

---

## kubectl

`kubectl` is a command-line tool that allows us to communicate with a cluster's control plane.

| Command | Description |
|---|---|
| `kubectl apply -f example-deployment.yaml` | Turns a Kubernetes config file into a running process. |
| `kubectl get pods -n example-namespace` | Checks the state of resources. |
| `kubectl describe pod example-pod -n example-namespace` | Provides details on why a pod may have crashed. |
| `kubectl logs example-pod -n example-namespace` | Views logs for troubleshooting. |
| `kubectl exec -it example-pod -n example-namespace -- sh` | Opens a shell inside a running container. |
| `kubectl port-forward` | Creates a secure tunnel between your local machine and a running pod in the cluster. |

---

## Kubernetes and DevSecOps

- Introducing any new tool into a system increases security risk, as it creates a new potential attack surface. Security best practices should always be applied.
- **RBAC (Role-Based Access Control)** is used to regulate access to a Kubernetes cluster and its resources.
- **Pod Security Standards** define security policies at three levels, enforced by **Pod Security Admission**.
- **Secrets** can and should be used to store sensitive information.

---

## Hands-On with Kubernetes

Steps performed during the practical exercise:

1. Started the cluster with `minikube start`.
2. Verified running pods with `kubectl get pods -A`.
3. Applied the configuration files:
   ```
   kubectl apply -f nginx-service.yaml
   kubectl apply -f nginx-deployment.yaml
   ```
4. Confirmed the pods were running with `kubectl get pods -A`.
5. Forwarded a local port to the Kubernetes service using:
   ```
   kubectl port-forward service/nginx-service 8090:8080
   ```
6. Accessed the service on `localhost:8090`.
7. Ran `kubectl get secrets` to retrieve credentials, piped them to log in, and captured the flag.

> Flag: `THM{k8s_k3nno1ssarus}`
