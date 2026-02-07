# ☸️ Kubernetes Workloads

### ReplicaSet & Deployment — Managing Pods and Updates Properly

<p align="center">
  <img src="https://img.shields.io/badge/Focus-Kubernetes-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Category-Workloads-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white" />
  <img src="https://img.shields.io/badge/Type-Hands--On-success?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Status-Completed-orange?style=for-the-badge" />
</p>

---

## 🚀 About This Topic

In Kubernetes, **Pods are ephemeral**:

* They can crash
* They can be deleted
* They can be rescheduled to another node

If Pods were used directly, applications would be **unreliable**.

To solve this, Kubernetes provides **Workload Controllers** such as **ReplicaSet** and **Deployment**. These controllers:

* Keep Pods running
* Maintain desired replica count
* Handle scaling and updates safely

This document is a **clean, complete, production‑ready reference**, based on **hands‑on practice and real cluster behavior**.

---

## 🔁 ReplicaSet

A **ReplicaSet** ensures that a **specified number of identical Pods are always running**.

### What ReplicaSet Does

* Maintains the desired number of Pods
* Automatically creates Pods if they crash or are deleted
* Continuously monitors actual vs desired state

### Example Scenario

```
Desired replicas = 3
Running pods = 2
→ ReplicaSet creates 1 new Pod
```

---

### ❓ Why ReplicaSet Is Needed

* Pods are temporary
* Manual Pod recreation is unreliable
* Applications must stay available

ReplicaSet solves this by:

* Self‑healing Pods
* Enforcing replica count

---

### ⚠️ Important Points About ReplicaSet

* ReplicaSet **only manages Pods**
* ReplicaSet **does not manage updates**
* ReplicaSet is **rarely used directly** in production

👉 In real‑world usage, **Deployments manage ReplicaSets**.

---

## 🚀 Deployment

A **Deployment** is a higher‑level controller that:

* Manages ReplicaSets
* Manages Pods
* Handles application updates
* Supports rollback and scaling

Deployment is the **most commonly used workload** in Kubernetes.

---

### ❓ Why Deployment Is Needed

ReplicaSet alone cannot:

* Perform rolling updates
* Track application versions
* Roll back failed updates

Deployment solves this by:

* Creating new ReplicaSets during updates
* Gradually shifting traffic
* Preserving application availability

---

## 🔄 Rollouts (VERY IMPORTANT)

A **rollout** is the process of updating Pods **gradually**.

### Rolling Update Behavior

* Old Pods are terminated one by one
* New Pods are created one by one
* Application stays available (zero or minimal downtime)

---

## 🧠 Deployment Update Strategy

### RollingUpdate Strategy (Default)

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 1
    maxSurge: 1
```

| Field            | Meaning                                      |
| ---------------- | -------------------------------------------- |
| `maxUnavailable` | Pods allowed to be unavailable during update |
| `maxSurge`       | Extra Pods allowed above desired replicas    |

---

## 🧩 ReplicaSet vs Deployment (Key Difference)

| Feature          | ReplicaSet | Deployment |
| ---------------- | ---------- | ---------- |
| Pod management   | ✅          | ✅          |
| Self‑healing     | ✅          | ✅          |
| Rolling updates  | ❌          | ✅          |
| Rollback         | ❌          | ✅          |
| Production usage | ❌          | ✅          |

---

## 📌 Common Commands (Hands‑On)

### Create ReplicaSet

```bash
kubectl apply -f replicaset.yaml
```

### Edit ReplicaSet

```bash
kubectl edit replicaset my-replicaset -n myspace
```

### Check Pods

```bash
kubectl get pods -n myspace
```

### Update Image in Deployment

```bash
kubectl set image deployment my-deployment nginx=nginx:1.21 -n myspace
```

### Rollback Deployment

```bash
kubectl rollout undo deployment my-deployment -n myspace
```

### Scale Deployment

```bash
kubectl scale deployment my-deployment --replicas=5 -n myspace
```

---

## 📄 YAML Examples

---

### 1️⃣ ReplicaSet YAML Example

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
  namespace: myspace
spec:
  replicas: 3
  selector:
    matchLabels:
      app: demo-app
  template:
    metadata:
      labels:
        app: demo-app
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```

---

### 2️⃣ Deployment YAML (Basic)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
  namespace: myspace
spec:
  replicas: 3
  selector:
    matchLabels:
      app: demo-app
  template:
    metadata:
      labels:
        app: demo-app
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```

---

### 3️⃣ Deployment YAML (Rolling Update Strategy)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
  namespace: myspace
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  selector:
    matchLabels:
      app: demo-app
  template:
    metadata:
      labels:
        app: demo-app
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80
```

---

## ❌ Common Mistakes (Important)

* Using ReplicaSet directly in production ❌
* Updating Pods manually ❌
* Forgetting rollout status checks ❌
* Not defining update strategy ❌

---

## 🧠 Final Takeaway

> **ReplicaSet keeps Pods alive, Deployment keeps applications healthy, scalable, and up‑to‑date.**

In real projects:

* You almost always use **Deployments**
* ReplicaSets work **behind the scenes**

---

📌 This document is suitable for:

* README.md
* Kubernetes learning notes
* Interview preparation
* GitHub documentation

---

### 🔜 Next Recommended Topics

* StatefulSet vs Deployment
* DaemonSet
* Horizontal Pod Autoscaling (HPA)
* Rolling vs Recreate strategy

---

✅ **Status: Complete, Clean & Production‑Ready Notes**
