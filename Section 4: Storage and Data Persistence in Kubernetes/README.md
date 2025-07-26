### **Section 4: Storage and Data Persistence in Kubernetes**

*This section focuses on how Kubernetes manages data persistence, storage abstractions, and enterprise-grade strategies for stateful applications in dynamic environments.*

---

## **4.1 Core Storage Challenge in Containers**

Containers are **ephemeral** by design:

* If a Pod dies, its local filesystem is lost.
* Stateless apps like web servers are unaffected, but **databases, queues, and caches require persistent storage**.

Kubernetes solves this with **Persistent Volumes (PVs)** and **Persistent Volume Claims (PVCs)**, enabling data persistence across Pod restarts and node failures.

---

## **4.2 Kubernetes Storage Architecture**

**Key Components:**

* **Volume:** Ephemeral storage attached to a Pod.
* **PersistentVolume (PV):** A cluster-wide storage resource provisioned by an admin or dynamically.
* **PersistentVolumeClaim (PVC):** A request for storage by a Pod.
* **StorageClass:** Defines storage types, provisioners, and policies for dynamic volume provisioning.
* **CSI (Container Storage Interface):** Standard API for storage vendors to integrate with Kubernetes.

---

### **Volume Types**

1. **EmptyDir**

   * Temporary storage tied to Pod lifecycle.
   * Deleted when Pod is deleted.
   * **Use Case:** Caching data for short-lived processes.

2. **hostPath**

   * Maps a file or directory on the host node to a Pod.
   * **Risk:** Node affinity; not portable or scalable.

3. **Persistent Volumes (Preferred)**

   * Supports durable, external storage independent of Pod lifecycle.

---

## **4.3 Persistent Volume Workflow**

1. **Admin defines a PV** or storage class for dynamic provisioning.
2. **Developer creates a PVC** requesting specific size and access mode.
3. Kubernetes binds PVC to an available PV (or creates one dynamically if a StorageClass is defined).
4. Pod mounts the PVC for persistent storage.

---

## **4.4 Storage Classes and Dynamic Provisioning**

* **Why Dynamic Provisioning?**
  Manual PV creation does not scale for large environments.
* StorageClass enables **on-demand provisioning** by specifying:

  * Provisioner (e.g., `kubernetes.io/aws-ebs`)
  * ReclaimPolicy (`Retain`, `Delete`)
  * VolumeBindingMode (`Immediate` or `WaitForFirstConsumer`)

**Example: AWS EBS StorageClass**

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: gp2
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
reclaimPolicy: Delete
```

---

## **4.5 Access Modes**

* **ReadWriteOnce (RWO):** Single node read/write access.
* **ReadOnlyMany (ROX):** Multiple nodes read-only access.
* **ReadWriteMany (RWX):** Multiple nodes read/write access (e.g., NFS, EFS).

---

## **4.6 StatefulSets for Stateful Applications**

* StatefulSets maintain **stable network identity and persistent storage** for Pods.
* Each Pod in a StatefulSet has its own PVC, ensuring data is not lost when Pods restart.

**Example Use Case:**
Deploying a replicated MySQL cluster:

* Each replica has its own EBS-backed PVC.
* Pods are started and terminated in an ordered sequence.

---

## **4.7 Real-World Scenarios**

### **Scenario 1: EKS with Dynamic Storage**

* Application requires 500GB persistent storage in AWS.
* Configure StorageClass with `kubernetes.io/aws-ebs` provisioner.
* Use PVC for automatic EBS volume provisioning.

### **Scenario 2: Multi-Pod Access (Shared Storage)**

* Analytics application requires shared read/write across multiple Pods.
* Use Amazon EFS with RWX mode via CSI driver.

---

## **4.8 Data Lifecycle and Reclaim Policies**

* **Retain:** PV remains after PVC deletion (manual cleanup required).
* **Delete:** PV is deleted when PVC is deleted (common for dynamic provisioning).
* **Recycle (Deprecated):** Basic scrub before reuse.

---

## **4.9 Common Challenges**

* **Node Affinity Issues:** EBS volumes are AZ-specific.
* **Pod Scheduling Delays:** PVC binding mode is `WaitForFirstConsumer`.
* **Data Corruption Risks:** Improper handling of RWX with databases.

---

## **4.10 Interview-Level Discussion Points**

* Explain the difference between PV, PVC, and StorageClass.
* How does dynamic provisioning work in Kubernetes?
* Why are StatefulSets preferred for databases over Deployments?
* How would you enable RWX storage in EKS?
* What happens when a Pod using hostPath is rescheduled?

---

## **4.11 Hands-On Exercise**

* Create a **PersistentVolume** and **PersistentVolumeClaim** manually.
* Deploy a Pod that mounts the PVC and writes data.
* Delete the Pod and verify data persistence.
* Implement **dynamic provisioning** with AWS EBS in EKS.
* Deploy a **StatefulSet** for a database using Persistent Volumes.
