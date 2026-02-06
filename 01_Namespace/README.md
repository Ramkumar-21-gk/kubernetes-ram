
# Kubernetes Namespace – Complete Notes

## 1. Concept of Namespace

A **Namespace** in Kubernetes is a **logical separation** inside a single cluster.

One Kubernetes cluster can run **multiple applications, teams, or environments**.
Namespaces help Kubernetes **organize and isolate resources logically**.

Namespaces do **not create new clusters**.
They divide **one cluster into multiple virtual spaces**.

---

## 2. Why Namespace is Needed

Namespaces are used to:

- Separate applications
- Separate environments (dev, test, prod)
- Avoid resource name conflicts
- Apply access control
- Manage cluster resources easily

Without namespaces:
- All resources exist together
- Same names cannot be reused
- Management becomes difficult

---

## 3. How Namespace Works

- Every Kubernetes object belongs to **one namespace**
- Resources in one namespace are **isolated from others**
- Same resource name can exist in different namespaces
- Cross-namespace access needs explicit reference

Example:
- Pod `web` in namespace `dev`
- Pod `web` in namespace `prod`

Both are valid because namespaces are different.

---

## 4. Default Namespaces in Kubernetes

| Namespace         | Purpose                             |
|------------------|-------------------------------------|
| default          | Used when no namespace is specified |
| kube-system      | Kubernetes internal components      |
| kube-public      | Public readable data                |
| kube-node-lease  | Node heartbeat information          |

---

## 5. Namespaced vs Non-Namespaced Resources

### Namespaced Resources

- Pods
- Services
- Deployments
- ConfigMaps
- Secrets
- ReplicaSets

### Non-Namespaced Resources (Cluster-wide)

- Nodes
- PersistentVolumes
- Namespaces
- StorageClasses

---

## 6. Important Namespace Commands

### List all namespaces
```bash
kubectl get namespaces
````

### Create a namespace

```bash
kubectl create namespace dev
```

### Delete a namespace

```bash
kubectl delete namespace dev
```

### Get pods in a namespace

```bash
kubectl get pods -n dev
```

### Create resource in a namespace

```bash
kubectl run nginx --image=nginx -n dev
```

### View resources in all namespaces

```bash
kubectl get pods --all-namespaces
```

### Set default namespace

```bash
kubectl config set-context --current --namespace=dev
```

---

## 7. Namespace YAML Definition

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: test
```

Apply the file:

```bash
kubectl apply -f namespace.yaml
```

---

## 8. Pod Example with Namespace

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
  namespace: dev
spec:
  containers:
  - name: nginx
    image: nginx
```

---

## 9. Accessing Resources Across Namespaces

* Resources cannot directly access others in different namespaces
* Services are accessed using full DNS name:

```
service-name.namespace.svc.cluster.local
```

---

## 10. Resource Management Using Namespace

Namespaces support:

* ResourceQuota – limits CPU, memory, and objects
* LimitRange – default and max/min resource limits
* RBAC – role-based access control per namespace

---

## 11. Limitations of Namespace

* Does not isolate nodes
* Does not isolate network by default
* CPU and memory are not limited automatically
* Requires additional policies for strong isolation

---

## 12. One-Line Exam Definition

A Namespace in Kubernetes is a logical isolation mechanism used to organize, manage, and control access to resources within a single cluster.

---

## 13. Key Points to Remember

* Namespace is logical, not physical
* Same resource name allowed in different namespaces
* Some resources are cluster-wide
* Mainly used in multi-team and multi-environment clusters
