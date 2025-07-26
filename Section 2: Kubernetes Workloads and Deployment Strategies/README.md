### **Section 2: Kubernetes Workloads and Deployment Strategies**

*This section explores the primary building blocks for deploying and managing applications on Kubernetes, with production-level strategies and insights.*

---

## **2.1 Core Kubernetes Workload Resources**

### **Pod**

* The smallest deployable unit in Kubernetes.
* Encapsulates one or more containers that share storage, networking, and lifecycle.
* Designed as an abstraction layer over containers for orchestration purposes.

**Key Points:**

* Containers within the same Pod share a network namespace and can communicate via `localhost`.
* Pods are **ephemeral**; they do not self-heal and are recreated by higher-level controllers.

---

### **ReplicaSet**

* Maintains a stable set of replica Pods at any given time.
* Ensures the specified number of replicas are running even if Pods fail.
* Used internally by Deployments.

---

### **Deployment**

* Provides **declarative updates** for Pods and ReplicaSets.
* Manages rollout and rollback strategies.
* Commonly used for stateless applications.

**Key Features:**

* Supports rolling updates and rollback in case of failure.
* Handles versioned upgrades seamlessly.

---

### **StatefulSet**

* Ensures **sticky identities** for Pods.
* Designed for **stateful applications** such as databases.
* Provides predictable Pod naming, persistent storage, and ordered scaling.

**Key Challenges:**

* StatefulSets require a **Persistent Volume Claim (PVC)** for each replica.
* Scaling and failover are more complex compared to Deployments.

---

### **DaemonSet**

* Ensures a copy of a Pod runs on **every node**.
* Typically used for logging agents, monitoring agents, or security daemons.

---

### **Job & CronJob**

* **Job:** Runs a task to completion, useful for batch processing.
* **CronJob:** Schedules Jobs based on time intervals or cron expressions.

---

## **2.2 Deployment Strategies**

### **Rolling Update (Default)**

* Gradually replaces old Pods with new Pods.
* Ensures zero downtime when configured properly.

**Production Consideration:**
Configure `maxSurge` and `maxUnavailable` to balance speed and availability.

---

### **Blue-Green Deployment**

* Runs two environments: Blue (current) and Green (new).
* Switches traffic to the new version after validation.

**Advantages:**

* Instant rollback capability.
* Safer for critical systems but requires extra resources.

---

### **Canary Deployment**

* Gradually shifts traffic to the new version for a subset of users.
* Validates stability before full rollout.

**Common Tools:**

* Istio or service mesh for fine-grained traffic control.
* Argo Rollouts for Kubernetes-native canary deployments.

---

## **2.3 Practical Considerations for Enterprise Deployments**

* Always define **resource requests and limits** for containers.
* Implement **readiness probes** and **liveness probes** for application health.
* Use **labels and selectors** effectively for deployment targeting and service discovery.
* Enable **revision history** in Deployments for quick rollbacks.

---

## **2.4 Common Pitfalls**

* Deploying large updates without configuring rollout strategies leads to outages.
* Not configuring readiness probes results in traffic hitting unready Pods.
* Absence of resource limits can cause **node resource exhaustion**.

---

## **2.5 Real-World Scenario**

**Scenario:**
A payment processing application requires an upgrade from v1 to v2 with zero downtime.

**Solution Using Rolling Update:**

* Define `strategy` in Deployment spec:

  ```yaml
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  ```
* Validate new Pods via readiness probes before routing traffic.

---

## **2.6 Interview-Level Discussion Points**

* Explain differences between Deployments, StatefulSets, and DaemonSets.
* How do rolling updates differ from blue-green deployments in Kubernetes?
* What is the role of readiness and liveness probes during rollouts?
* How does Kubernetes ensure consistency during Deployment updates?
* When would you prefer a StatefulSet over a Deployment?

---

## **2.7 Hands-On Exercise**

* Create a **Deployment** for a stateless web application.
* Perform a rolling update and rollback using:

  ```bash
  kubectl rollout status deployment/<name>
  kubectl rollout undo deployment/<name>
  ```
* Implement a **readiness probe** to delay traffic until the application is ready.
* Deploy a **StatefulSet** for a database (e.g., MySQL) with Persistent Volumes.
