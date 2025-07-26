### **Section 6: Security and Access Control in Kubernetes**

*This section focuses on securing Kubernetes clusters at multiple layers: API access, workloads, and networking, while ensuring compliance and enterprise-grade isolation.*

---

## **6.1 Why Security is Critical in Kubernetes**

* **Dynamic nature of containers:** Rapid Pod creation increases the attack surface.
* **Shared infrastructure:** Multi-tenant clusters require strict isolation.
* **API-centric design:** The Kubernetes API is the primary attack vector if not properly secured.

---

## **6.2 Core Security Layers**

Kubernetes security involves **three key layers**:

1. **Cluster Access Control** – Who can interact with the cluster and what they can do.
2. **Workload Security** – How Pods and containers run securely.
3. **Network Security** – Controlling east-west (Pod-to-Pod) and north-south (external) traffic.

---

## **6.3 Cluster Access Control**

### **Authentication**

* **Supported Methods:**

  * Certificates
  * Bearer Tokens
  * OIDC Providers (AWS IAM in EKS)
* In EKS:

  * IAM roles mapped to Kubernetes RBAC using `aws-auth` ConfigMap.

### **Authorization**

* **Role-Based Access Control (RBAC):**

  * Roles define permissions for resources (e.g., Pods, Services).
  * RoleBindings or ClusterRoleBindings assign Roles to users or service accounts.

**Example: RBAC for Read-Only Access**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list"]
```

---

## **6.4 Workload Security**

### **Pod Security**

* **Run as Non-Root:** Enforce containers to avoid root privileges.
* **Drop Unnecessary Linux Capabilities:** Limit container permissions.
* **Read-Only Root Filesystem:** Prevent tampering with container image.
* **Seccomp & AppArmor Profiles:** Harden system calls.

**Pod Security Admission (PSA)**

* Kubernetes 1.25+ replaces PodSecurityPolicy (deprecated).
* PSA enforces three levels:

  * **Privileged**
  * **Baseline**
  * **Restricted** (preferred for production)

---

### **Service Accounts and Secrets**

* Service Accounts provide Pod identity for API interactions.
* Use Kubernetes Secrets for sensitive data (TLS keys, credentials).
* Integrate with **KMS solutions** like AWS KMS or HashiCorp Vault for encryption.

---

## **6.5 Network Security**

* **Network Policies:** Define ingress/egress rules for Pod communication.

  * Example: Allow only the frontend to talk to the backend service.
* **CNI Integration:** Use Calico or Cilium for policy enforcement.
* **Ingress Security:**

  * TLS termination at ingress.
  * Web Application Firewall (WAF) for additional protection.

---

## **6.6 API Server Hardening**

* Enable **API audit logging** for compliance.
* Restrict API access with:

  * Network policies for the control plane.
  * API whitelisting via `--authorization-mode=Node,RBAC`.
* Use **etcd encryption** for secrets at rest.

---

## **6.7 Real-World Enterprise Scenario**

**Scenario:** Multi-tenant EKS cluster for dev, staging, and production.

* Implement **namespace-level RBAC** for teams.
* Apply **Network Policies** to prevent cross-namespace traffic.
* Use **PodSecurity Admission** for restricted policies in production.
* Enable **IRSA (IAM Roles for Service Accounts)** for fine-grained AWS access.

---

## **6.8 Common Security Risks**

* **Privilege Escalation:** Pods running as root.
* **Exposed Dashboard:** Unauthenticated access to Kubernetes dashboard.
* **Hardcoded Secrets:** Credentials in environment variables or ConfigMaps.
* **Wide RBAC Roles:** Over-permissive roles like `cluster-admin`.

---

## **6.9 Compliance and Best Practices**

* Enable **image scanning** (Trivy, Clair) for vulnerabilities.
* Enforce **signed container images** using Notary or Cosign.
* Regularly **rotate API tokens and credentials**.
* Apply **CIS Kubernetes Benchmarks** for security auditing.

---

## **6.10 Interview-Level Discussion Points**

* How does RBAC work in Kubernetes, and how does it differ from IAM in EKS?
* Explain Pod Security Admission and its levels.
* What is the difference between Network Policies and Security Groups in EKS?
* How would you secure secrets in Kubernetes?
* How does IRSA improve security in EKS?

---

## **6.11 Hands-On Exercise**

* Create an RBAC role for read-only access to Pods in `dev` namespace.
* Enforce **PodSecurity Admission** for restricted profiles.
* Implement **Network Policies** to isolate backend services.
* Integrate **IRSA** in EKS for accessing AWS S3 securely.\
