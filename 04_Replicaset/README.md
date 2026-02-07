# ReplicaSet in Kubernetes

## 1. What is a ReplicaSet? (Core Concept)

A **ReplicaSet** is a Kubernetes controller that **ensures a fixed number of Pods are always running**.

- If a Pod dies → ReplicaSet creates a new Pod  
- If extra Pods exist → ReplicaSet deletes them  

**ReplicaSet maintains Pod count.**

---

## 2. Why ReplicaSet Exists

Pods are unreliable because:

- Pods can crash
- Pods can be deleted
- Nodes can fail

ReplicaSet solves this by:

- Continuously monitoring Pods
- Matching actual Pod count with desired count
- Automatically fixing differences

---

## 3. What ReplicaSet Does Exactly

ReplicaSet:

- Watches Pods using **labels**
- Creates Pods when count is less
- Deletes Pods when count is more
- Does **not** handle application updates properly

ReplicaSet focuses on **quantity**, not **versioning**.

---

## 4. ReplicaSet Architecture

```

ReplicaSet
↓
Label Selector
↓
Pods

````

- ReplicaSet does not manage containers directly
- It manages Pods using labels

---

## 5. ReplicaSet vs Deployment

| ReplicaSet           | Deployment               |
|----------------------|--------------------------|
| Maintains Pod count  | Manages ReplicaSets      |
| No rollout strategy  | Supports rolling updates |
| No rollback          | Supports rollback        |
| Rarely used directly | Used in production       |

**In real projects, Deployments are used instead of ReplicaSets.**

---

## 6. How ReplicaSet Identifies Pods

ReplicaSet uses labels:

```yaml
selector:
  matchLabels:
    app: web
````

Any Pod with label `app=web`:

* Is managed by the ReplicaSet
* Is counted toward replicas

⚠️ Wrong labels can cause unintended Pod deletion.

---

## 7. ReplicaSet YAML Example

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: web-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: nginx
        image: nginx
```

Apply the ReplicaSet:

```bash
kubectl apply -f replicaset.yaml
```

---

## 8. Important ReplicaSet Commands

List ReplicaSets:

```bash
kubectl get rs
```

Describe ReplicaSet:

```bash
kubectl describe rs web-rs
```

View Pods managed by ReplicaSet:

```bash
kubectl get pods -l app=web
```

Scale ReplicaSet:

```bash
kubectl scale rs web-rs --replicas=5
```

Delete ReplicaSet:

```bash
kubectl delete rs web-rs
```

---

## 9. ReplicaSet Self-Healing Example

* Desired replicas = 3
* One Pod crashes
* ReplicaSet automatically creates a new Pod
* Pod count returns to 3

This process is automatic and continuous.

---

## 10. ReplicaSet Limitations

* No rolling updates
* No version control
* No rollback support
* Manual image changes cause Pod recreation

Because of this:

**ReplicaSet is not used directly in production.**

---

## 11. Relationship: Deployment → ReplicaSet → Pod

* Deployment creates ReplicaSet
* ReplicaSet creates Pods
* On update:

  * New ReplicaSet is created
  * Old ReplicaSet is scaled down

ReplicaSet works silently in the background.

---

## 12. One-Line Exam Definition

A ReplicaSet is a Kubernetes controller that ensures a specified number of identical Pods are running at all times.

---

## 13. Key Points to Remember

* ReplicaSet manages Pod count
* Uses labels and selectors
* Provides self-healing
* Deployment is preferred over ReplicaSet
* Rarely managed directly by users

