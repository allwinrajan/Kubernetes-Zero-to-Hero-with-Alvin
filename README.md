## **Section 1: Kubernetes Core Concepts and Architecture**

**Scope:**

* Kubernetes purpose and orchestration advantages over manual deployments.
* Cluster architecture: Control Plane vs Worker Nodes.
* Detailed roles of API Server, etcd, Scheduler, Controller Manager, and Cloud Controller Manager.
* Node components: Kubelet, kube-proxy, Container Runtime Interface (CRI).
* Kubernetes API design and declarative model.

**Discussion Points:**

* Why Kubernetes uses etcd as a distributed key-value store.
* How API Server acts as the single point of truth.
* The importance of control plane high availability in production.

---

## **Section 2: Kubernetes Workloads and Deployment Strategies**

**Scope:**

* Core workload objects: Pods, ReplicaSets, Deployments, StatefulSets, DaemonSets, and Jobs.
* Deployment strategies: Rolling updates, Blue-Green deployments, Canary releases.
* Multi-tier application deployment patterns in Kubernetes.

**Discussion Points:**

* Difference between Deployments and StatefulSets in terms of persistence and scaling.
* Strategies to minimize downtime during updates.
* Common failure scenarios during deployments and mitigation techniques.

---

## **Section 3: Networking and Service Discovery**

**Scope:**

* Kubernetes networking model and CNI plugin architecture.
* Service types: ClusterIP, NodePort, LoadBalancer, and Ingress.
* DNS-based service discovery in Kubernetes clusters.
* External traffic routing via Ingress controllers (NGINX, ALB).

**Discussion Points:**

* How kube-proxy manages networking for services.
* When to choose NodePort vs LoadBalancer.
* Designing an ingress layer for multi-service microservices architecture.

---

## **Section 4: Storage and Data Persistence**

**Scope:**

* Persistent Volume (PV) and Persistent Volume Claim (PVC) model.
* StorageClasses and dynamic provisioning.
* Stateful workloads and data consistency.
* Integration with cloud-based storage (EBS, EFS) and CSI drivers.

**Discussion Points:**

* How Kubernetes handles stateful applications in a stateless environment.
* Storage binding and reclaim policies.
* Best practices for database deployments in Kubernetes.

---

## **Section 5: Scaling, Performance, and Resource Management**

**Scope:**

* Horizontal Pod Autoscaler (HPA) and Vertical Pod Autoscaler (VPA).
* Cluster Autoscaler and node scaling mechanisms.
* Resource requests, limits, and QoS classes.
* Pod eviction and node pressure handling.

**Discussion Points:**

* Designing resource allocation for predictable application performance.
* How autoscalers integrate with metrics APIs.
* Handling burst traffic in large-scale deployments.

---

## **Section 6: Security and Access Control**

**Scope:**

* Role-Based Access Control (RBAC) for multi-tenant clusters.
* Service Accounts and authentication mechanisms.
* Pod Security Standards and admission controllers.
* Network segmentation with Network Policies.

**Discussion Points:**

* Securing API server communication.
* Practical strategies for namespace isolation.
* Risk analysis of running privileged containers.

---

## **Section 7: Observability, Logging, and Troubleshooting**

**Scope:**

* Native logging and monitoring tools: Metrics Server, kube-state-metrics.
* Advanced observability with Prometheus, Grafana, and Alertmanager.
* Centralized logging using EFK or ELK stack.
* Troubleshooting common issues: CrashLoopBackOff, ImagePullBackOff, network failures.

**Discussion Points:**

* Designing a monitoring strategy for distributed clusters.
* Interpreting metrics for proactive scaling and fault detection.
* Structured troubleshooting methodologies for Kubernetes workloads.

---

## **Section 8: Kubernetes in Production and Managed Solutions**

**Scope:**

* Self-managed Kubernetes vs Managed Kubernetes services (Amazon EKS, GKE, AKS).
* High Availability control plane design.
* Cluster lifecycle management: Upgrades, backups, and disaster recovery.
* Integrating Kubernetes with CI/CD and GitOps workflows (Helm, ArgoCD).

**Discussion Points:**

* Operational challenges in self-managed clusters.
* EKS architecture and AWS-native integrations (IAM, ALB, CloudWatch).
* Best practices for enterprise Kubernetes deployments.
