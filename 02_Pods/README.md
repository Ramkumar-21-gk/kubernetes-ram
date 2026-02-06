
# Pods in Kubernetes

## 1. What is a Pod? (Core Concept)

A **Pod** is the **smallest deployable unit** in Kubernetes.

Kubernetes does **not run containers directly**.  
It runs **containers inside Pods**.

**Pod = wrapper around one or more containers**

---

## 2. Why Pod Exists

Containers alone are not enough because:

- Containers need networking
- Containers may need shared storage
- Some containers must run together

A Pod provides:

- Shared network
- Shared storage
- Shared lifecycle

---

## 3. What a Pod Contains

A Pod contains:

- One or more containers
- One IP address
- One network namespace
- Shared volumes
- Pod metadata

All containers inside a Pod:

- Share the same IP
- Communicate using `localhost`
- Start and stop together

---

## 4. Single-Container Pod (Most Common)

Most Pods run **one container**.

Reasons:

- Simple design
- Easy to scale
- Easy to manage

Example use case:
- One web application container

---

## 5. Multi-Container Pod

Some Pods run **multiple containers**.

These containers:

- Work together
- Are tightly coupled
- Cannot run independently

Example:
- Main application container
- Log collector container
- Helper (sidecar) container

---

## 6. Pod Lifecycle

Pod lifecycle steps:

1. Pod is created
2. Scheduled to a node
3. Containers start
4. Pod runs
5. Pod terminates or fails

Important points:

- If a Pod dies, Kubernetes creates a **new Pod**
- Pod IP **changes when recreated**

---

## 7. Pod Networking

- Each Pod gets **one unique IP**
- All containers share the same IP
- Containers communicate using `localhost`
- Pods communicate using Pod IP or Service
- No NAT between containers inside the same Pod

---

## 8. Pod Storage (Volumes)

Pods support volumes:

- Temporary or persistent
- Shared among containers in the Pod
- Exists as long as the Pod exists (unless persistent)

---

## 9. Pod vs Container

| Pod                   | Container                    |
|-----------------------|------------------------------|
| Kubernetes object     | Runtime object               |
| Wraps containers      | Runs application             |
| Has IP and volumes    | No independent IP            |
| Managed by Kubernetes | Managed by container runtime |

---

## 10. Pod vs Node

- **Node** → Machine (VM or Server)
- **Pod** → Runs on a Node
- Multiple Pods can run on one Node

---

## 11. Basic Pod Commands

Create Pod:
```bash
kubectl run nginx-pod --image=nginx
````

List Pods:

```bash
kubectl get pods
```

Describe Pod:

```bash
kubectl describe pod nginx-pod
```

Get detailed Pod info:

```bash
kubectl get pods -o wide
```

Delete Pod:

```bash
kubectl delete pod nginx-pod
```

View Pod logs:

```bash
kubectl logs nginx-pod
```

Execute command inside Pod:

```bash
kubectl exec -it nginx-pod -- /bin/bash
```

---

## 12. Pod YAML Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: nginx-container
    image: nginx
```

Apply:

```bash
kubectl apply -f pod.yaml
```

---

## 13. Multi-Container Pod Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-pod
spec:
  containers:
  - name: app
    image: nginx
  - name: sidecar
    image: busybox
    command: ["sleep", "3600"]
```

---

## 14. Important Pod States

* Pending
* Running
* Succeeded
* Failed
* CrashLoopBackOff

---

## 15. Limitations of Pods

* Pods are not self-healing
* Pods are not scalable
* Pods are temporary
* Controllers are required for management

Pods are usually managed using:

* Deployment
* ReplicaSet
* StatefulSet

---

## 16. One-Line Exam Definition

A Pod is the smallest execution unit in Kubernetes that encapsulates one or more containers with shared networking and storage.

---

## 17. Key Points to Remember

* Kubernetes manages Pods, not containers
* Pod is the smallest unit in Kubernetes
* Containers inside a Pod share network and storage
* Pods are short-lived
* Use Deployments for real-world applications

