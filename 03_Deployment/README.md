
# Deployment in Kubernetes

## 1. What is a Deployment? (Core Concept)

A **Deployment** is a Kubernetes object used to **run and manage Pods in a controlled way**.

You **do not use Pods directly** for real applications.  
You use **Deployments**, and Kubernetes creates and manages Pods for you.

**Deployment = Manager of Pods**

---

## 2. Why Deployment is Needed

Pods have limitations:

- If a Pod dies, it is not recreated automatically
- Pods cannot scale easily
- Updating Pods manually is risky

Deployment solves this by:

- Keeping Pods always running
- Creating multiple replicas
- Updating Pods safely
- Rolling back if an update fails

---

## 3. What Deployment Controls

A Deployment manages:

- ReplicaSets (automatically)
- Pods (indirectly)
- Application updates
- Scaling
- Self-healing

You never manage ReplicaSets directly.  
Deployment handles them for you.

---

## 4. Deployment Architecture (Internal Flow)

```

Deployment
↓
ReplicaSet
↓
Pods

````

- Deployment defines the desired state
- ReplicaSet maintains the number of Pods
- Pods run the application

---

## 5. Key Features of Deployment

- Self-healing
- Manual or auto-scaling
- Rolling updates
- Rollback support
- Zero-downtime updates

---

## 6. Deployment vs Pod

| Pod                  | Deployment        |
|----------------------|------------------|
| Single instance      | Manages many Pods |
| Not scalable         | Scalable          |
| No self-healing      | Self-healing      |
| Not production ready | Production ready  |

---

## 7. Basic Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
````

Apply the Deployment:

```bash
kubectl apply -f deployment.yaml
```

---

## 8. Important Fields Explained

### replicas

Defines how many Pods should run.

### selector

Tells the Deployment which Pods it manages.

### template

Contains the Pod definition.

---

## 9. Common Deployment Commands

Create Deployment:

```bash
kubectl create deployment nginx --image=nginx
```

List Deployments:

```bash
kubectl get deployments
```

Describe Deployment:

```bash
kubectl describe deployment nginx
```

View Pods:

```bash
kubectl get pods
```

Scale Deployment:

```bash
kubectl scale deployment nginx --replicas=5
```

Delete Deployment:

```bash
kubectl delete deployment nginx
```

---

## 10. Rolling Update

Update image:

```bash
kubectl set image deployment/nginx nginx=nginx:1.25
```

Kubernetes:

* Creates new Pods
* Deletes old Pods gradually
* Keeps application running

This is called a **Rolling Update**.

---

## 11. Rollback Deployment

Check rollout history:

```bash
kubectl rollout history deployment nginx
```

Rollback to previous version:

```bash
kubectl rollout undo deployment nginx
```

---

## 12. Deployment Status Commands

```bash
kubectl rollout status deployment nginx
```

```bash
kubectl rollout history deployment nginx
```

---

## 13. Deployment Update Strategies

### RollingUpdate (Default)

* Updates Pods one by one
* Zero downtime

### Recreate

* Deletes all Pods first
* Creates new Pods
* Causes downtime

---

## 14. Deployment in Namespace

List Deployments in namespace:

```bash
kubectl get deployment -n dev
```

Create Deployment in namespace:

```bash
kubectl apply -f deployment.yaml -n dev
```

---

## 15. Limitations of Deployment

* Not suitable for stateful applications
* Pod names change on restart
* Storage requires extra configuration

For stateful workloads:

* Use StatefulSet

---

## 16. One-Line Exam Definition

A Deployment in Kubernetes is a controller that manages Pods and ReplicaSets to provide scaling, self-healing, and rolling updates.

---

## 17. Key Points to Remember

* Never use Pods directly in production
* Deployment is the standard way to run applications
* Deployment manages ReplicaSets automatically
* Supports rollback and zero-downtime updates
