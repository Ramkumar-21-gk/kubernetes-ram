# DaemonSet in Kubernetes

## 1. What is a DaemonSet? (Core Concept)

A **DaemonSet** is a Kubernetes controller that **ensures exactly one Pod runs on every node** (or selected nodes) in a cluster.

- When a node is added → a Pod is created automatically
- When a node is removed → the Pod is deleted automatically

**DaemonSet = one Pod per node**

---

## 2. Why DaemonSet is Needed

Some workloads must run on **every node**, not just a fixed number of Pods.

Examples:
- Log collection agents
- Monitoring agents
- Networking components
- Security agents
- Storage agents

A Deployment is not suitable because:
- Deployment controls Pod count
- DaemonSet controls **node coverage**

---

## 3. How DaemonSet Works

- DaemonSet continuously watches cluster nodes
- Automatically schedules one Pod per node
- No `replicas` field is used
- Pods are bound directly to nodes
- New nodes get Pods automatically

---

## 4. What DaemonSet Manages

DaemonSet manages:
- Pod lifecycle
- Node addition and removal
- Automatic Pod placement on nodes

DaemonSet does NOT:
- Load balance traffic
- Manually scale Pods
- Replace application Deployments

---

## 5. Real Use Cases

DaemonSet is commonly used for:
- Log agents (Fluentd, Filebeat)
- Monitoring agents (Node Exporter)
- Network plugins (CNI)
- Security scanners
- Node-level system services

These workloads must run on **every node**, making DaemonSet the correct choice.

---

## 6. DaemonSet vs Deployment

| DaemonSet              | Deployment           |
|------------------------|----------------------|
| One Pod per node       | Fixed number of Pods |
| No replicas field      | Uses replicas        |
| Node-level workloads   | Application workloads|
| Auto-runs on new nodes | Manual scaling       |

---

## 7. Basic DaemonSet YAML Example

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: log-agent
spec:
  selector:
    matchLabels:
      app: log-agent
  template:
    metadata:
      labels:
        app: log-agent
    spec:
      containers:
      - name: agent
        image: nginx
````

Apply the DaemonSet:

```bash
kubectl apply -f daemonset.yaml
```

---

## 8. Important Fields Explained

### selector

Defines which Pods are managed by the DaemonSet.

### template

Contains the Pod definition.

Note:

* There is **no `replicas` field** in DaemonSet.

---

## 9. DaemonSet Commands

List DaemonSets:

```bash
kubectl get daemonset
```

Describe DaemonSet:

```bash
kubectl describe daemonset log-agent
```

View Pods created by DaemonSet:

```bash
kubectl get pods -l app=log-agent -o wide
```

Delete DaemonSet:

```bash
kubectl delete daemonset log-agent
```

---

## 10. DaemonSet with Node Selector

Run Pods only on selected nodes:

```yaml
spec:
  template:
    spec:
      nodeSelector:
        disk: ssd
```

Only nodes with label `disk=ssd` will run the DaemonSet Pod.

---

## 11. DaemonSet and New Nodes

When a new node joins the cluster:

* DaemonSet automatically creates a Pod on that node
* No manual action is required

---

## 12. Limitations of DaemonSet

* Cannot control Pod count manually
* Not suitable for application workloads
* No built-in load balancing
* Limited to one Pod per node

---

## 13. One-Line Exam Definition

A DaemonSet in Kubernetes ensures that a copy of a Pod runs on all or selected nodes in a cluster.

---

## 14. Key Points to Remember

* DaemonSet is a node-level controller
* One Pod per node
* No replicas field
* Used for logging, monitoring, networking
* Automatically handles node changes
