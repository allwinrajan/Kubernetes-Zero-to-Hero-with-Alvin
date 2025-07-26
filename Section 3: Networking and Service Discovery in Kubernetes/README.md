### **Section 3: Networking and Service Discovery in Kubernetes**

*This section provides an enterprise-level view of Kubernetes networking, service abstraction, and traffic management mechanisms with production considerations and troubleshooting strategies.*

---

## **3.1 Kubernetes Networking Model**

Kubernetes networking adheres to these principles:

* **Every Pod has its own IP address** (no NAT inside the cluster).
* **Pods can communicate with all other Pods** in the cluster by default.
* **Nodes can communicate with all Pods**, and vice versa.
* Network implementation is **plug-in driven** using **Container Network Interface (CNI)**.

---

### **Core Networking Components**

* **Pod-to-Pod Communication:** Implemented via a CNI plugin (Calico, Flannel, Cilium).
* **Service Abstraction:** Provides a stable network identity and load balancing for Pods.
* **kube-proxy:** Maintains network rules to forward traffic to appropriate Pod endpoints.
* **Cluster DNS:** Resolves service names to cluster IP addresses.

---

## **3.2 Service Types and Use Cases**

Kubernetes Services expose workloads internally and externally:

### **ClusterIP (Default)**

* Internal access within the cluster.
* Suitable for microservices communicating internally.

**Example:**
Backend API service accessible only to frontend Pods.

---

### **NodePort**

* Exposes the service on each Node’s IP at a static port.
* Allows external access without a Load Balancer.

**Limitations:**
Manual node IP management, not scalable for production.

---

### **LoadBalancer**

* Provisions an external load balancer from the cloud provider.
* Ideal for exposing services to the internet.

**Example:**
EKS automatically provisions an AWS ELB/ALB for LoadBalancer services.

---

### **ExternalName**

* Maps a service to an external DNS name.
* Useful for integrating external systems into a cluster.

---

## **3.3 Ingress and Traffic Routing**

* **Ingress Resource:** Defines HTTP(S) routing rules to services inside the cluster.
* **Ingress Controller:** Implements routing logic (NGINX, AWS ALB Ingress Controller).
* Supports:

  * Path-based routing (`/api`, `/admin`)
  * Host-based routing (`app.company.com`)

**Enterprise Use Case:**

* Deploying a single ingress for 50 microservices under different paths or subdomains.

---

## **3.4 CNI Plugins and Advanced Networking**

* **Calico:** Provides networking and network policies.
* **Flannel:** Simple overlay network for Pod connectivity.
* **Cilium:** eBPF-based networking for performance and security.

**Production Note:**
Calico is preferred for policy enforcement; Cilium for high-performance workloads.

---

## **3.5 DNS and Service Discovery**

* Kubernetes uses **CoreDNS** for DNS resolution.
* Services are accessible via:

  ```
  <service-name>.<namespace>.svc.cluster.local
  ```

**Example:**
`mysql.database.svc.cluster.local`

---

## **3.6 Real-World Scenarios**

**Scenario 1:**
Expose an internal API to external clients securely.

* Use **Ingress** with TLS termination.
* Apply **Network Policies** to limit Pod-to-Pod communication.

**Scenario 2:**
Multi-region cluster needing internal DNS resolution for inter-service communication.

* Configure **CoreDNS** with custom stub domains.
* Use service mesh (Istio) for traffic control and failover.

---

## **3.7 Common Challenges**

* **Service IP Conflicts:** Overlapping CIDR ranges between clusters and VPC.
* **DNS Resolution Failures:** CoreDNS misconfiguration or network plugin issues.
* **Unintended Network Access:** Lack of network policies leading to security risks.

---

## **3.8 Interview-Level Discussion Points**

* Explain the role of kube-proxy in service networking.
* Difference between ClusterIP, NodePort, and LoadBalancer.
* How does Kubernetes implement DNS-based service discovery?
* When should you use Ingress over LoadBalancer?
* How would you enforce east-west traffic security between Pods?

---

## **3.9 Hands-On Exercise**

* Deploy a **ClusterIP Service** for a backend application.
* Expose it externally using **NodePort** and **LoadBalancer**.
* Configure **Ingress** with TLS and path-based routing.
* Implement a **Network Policy** to restrict Pod communication by namespace.
