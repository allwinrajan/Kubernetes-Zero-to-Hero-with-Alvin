### **Section 7: Observability, Logging, and Troubleshooting in Kubernetes**

*This section explains how to monitor, log, and troubleshoot Kubernetes environments effectively with real-world strategies and production-level best practices.*

---

## **7.1 Why Observability Matters**

Kubernetes is a **distributed system** with multiple layers (Pods, Nodes, Control Plane, Network, Storage). Without observability:

* Failures remain hidden until service disruption occurs.
* Root cause analysis becomes time-consuming.
* Autoscaling decisions are ineffective without accurate metrics.

**Observability Goals:**

* Collect **metrics**, **logs**, and **traces** for actionable insights.
* Enable **proactive alerts** to prevent outages.
* Correlate events across workloads, nodes, and infrastructure.

---

## **7.2 Core Observability Components**

* **Metrics:** Quantitative data (CPU, memory, latency).
* **Logs:** Event records from containers, Pods, and cluster components.
* **Traces:** Distributed tracing for microservices interactions.

---

## **7.3 Metrics and Monitoring**

### **Native Tools**

* **metrics-server:**

  * Collects resource usage metrics for Pods and Nodes.
  * Enables Horizontal Pod Autoscaler (HPA).
* **kube-state-metrics:**

  * Exposes cluster state metrics (Deployments, Nodes, etc.).

### **Prometheus & Grafana**

* **Prometheus:**

  * Scrapes metrics via pull model from Kubernetes endpoints.
  * Stores time-series data for analysis.
* **Grafana:**

  * Visualizes Prometheus metrics with custom dashboards.

**Key Metrics to Monitor:**

* Pod CPU & memory utilization.
* Node disk and network performance.
* API server latency.
* etcd health and quorum status.

---

### **Alerting**

* Use **Alertmanager** with Prometheus for notifications (Slack, PagerDuty, Email).
* Define alerts for:

  * Pod CrashLoopBackOff.
  * Node NotReady.
  * High API server latency.
  * etcd quorum failure.

---

## **7.4 Logging**

### **Challenges**

* Containers are ephemeral; logs disappear when a Pod is deleted.
* Multi-container Pods and multi-node clusters require centralized logging.

### **Solutions**

* **EFK Stack (Elasticsearch, Fluentd, Kibana):**

  * Fluentd collects logs.
  * Elasticsearch stores logs.
  * Kibana visualizes logs.
* **Loki + Promtail + Grafana:**

  * Lightweight alternative to EFK.
* **Cloud-Native Logging (EKS):**

  * Send logs to AWS CloudWatch via Fluent Bit.

---

## **7.5 Tracing**

* Use **Jaeger** or **OpenTelemetry** for distributed tracing.
* Essential for debugging latency in microservices architectures.

---

## **7.6 Troubleshooting Common Issues**

### **Pod-Level**

* **CrashLoopBackOff:**
  Cause: Application error or readiness probe failure.
  Fix: Check logs using:

  ```bash
  kubectl logs <pod-name>
  kubectl describe pod <pod-name>
  ```

* **ImagePullBackOff:**
  Cause: Incorrect image name or private registry issue.
  Fix: Verify image name and registry credentials.

---

### **Node-Level**

* **Node NotReady:**
  Cause: Network plugin failure or resource exhaustion.
  Fix: Check `kubelet` logs and CNI health.

---

### **Control Plane**

* **API Server Latency:**
  Cause: etcd performance issue.
  Fix: Monitor etcd health using:

  ```bash
  etcdctl endpoint health
  ```

---

## **7.7 Real-World Observability Stack**

**Scenario: EKS Production Monitoring**

* **Metrics:** Prometheus + Grafana dashboards.
* **Logs:** Fluent Bit → CloudWatch.
* **Traces:** AWS X-Ray or Jaeger.
* **Alerting:** Prometheus Alertmanager integrated with PagerDuty.

---

## **7.8 Interview-Level Discussion Points**

* How do you monitor the health of etcd and the API server?
* Explain the difference between metrics, logs, and traces.
* How would you set up centralized logging in Kubernetes?
* What steps do you take to troubleshoot a Pod in CrashLoopBackOff?
* How does Prometheus differ from metrics-server?

---

## **7.9 Hands-On Exercise**

* Deploy **metrics-server** and enable HPA.
* Install **Prometheus and Grafana** via Helm.
* Configure **Alertmanager** for node health alerts.
* Deploy **Fluent Bit** to forward logs to CloudWatch (EKS scenario).
* Implement **Jaeger** for distributed tracing of a sample microservice.
