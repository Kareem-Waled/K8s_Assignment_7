☸️ Kubernetes Advanced Scheduling & Pod Management

> Configurations and execution logs for Kubernetes advanced scheduling tasks. This repository covers practical implementations of nodeSelector, hard/soft node affinity, pod evictions via taints, resource-based QoS classes, and specialized pod deployments.

![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Minikube](https://img.shields.io/badge/Minikube-2496ED?style=for-the-badge&logo=kubernetes&logoColor=white)

## 📖 Overview
This repository contains the configurations and execution proofs for **Kubernetes Assignment 7**. The project focuses on advanced pod scheduling techniques, node management, and understanding how the Kubernetes scheduler handles resource allocation and node constraints.

---

## 🏗️ Architecture & Concepts

This assignment explores several methods of bypassing or guiding the default Kubernetes scheduler:

1. **Direct Pinning:** Using `nodeName` to force a pod onto a specific node.
2. **Label-Based Scheduling:** Utilizing `nodeSelector` and **Node Affinity** (both Hard and Soft rules).
3. **Taints & Tolerations:** Controlling which pods can be scheduled on specific nodes using `NoSchedule`, `PreferNoSchedule`, and `NoExecute` effects.
4. **Quality of Service (QoS):** Categorizing pods into `Guaranteed`, `Burstable`, and `BestEffort` classes based on CPU and memory limits/requests.
5. **Specialized Pods:** Deploying **DaemonSets** (one pod per node) and **Static Pods** (managed directly by the `kubelet`).

---

## 🛠️ Prerequisites
* A running Kubernetes cluster (e.g., `minikube`)
* `kubectl` command-line tool installed and configured
* Access to the cluster nodes via SSH (for static pod creation)

---

## 🚀 Tasks & Execution Log

| Task | Feature | Description | Status |
| :--- | :--- | :--- | :---: |
| **1** | `nodeName` | Hard-pinning a pod to the `minikube` node, bypassing the scheduler. | ✅ |
| **2** | `nodeSelector` | Scheduling pods based on the `env=lab` node label and observing `Pending` status when labels are absent. | ✅ |
| **3** | Taints & Tolerations | Testing `NoSchedule` (new pod rejection) and `NoExecute` (existing pod eviction) taints. | ✅ |
| **4** | Node Affinity | Implementing `requiredDuringScheduling` (Hard) and `preferredDuringScheduling` (Soft) rules based on disk types and zones. | ✅ |
| **5** | QoS Classes | Creating 3 pods with varying resource requests/limits to trigger `Guaranteed`, `Burstable`, and `BestEffort` assignments. | ✅ |
| **6** | DaemonSet | Deploying a `node-monitor` to ensure exactly one pod runs on every available node. | ✅ |
| **7** | Static Pods | Using `minikube ssh` to write a manifest directly to `/etc/kubernetes/manifests/` for kubelet management. | ✅ |

---

## 💻 How to Run

1. **Start your local cluster:**
   ```bash
   minikube start
Apply the manifests:
Navigate to the respective task directories (if you split your YAMLs) or apply them directly:

Bash
kubectl apply -f <your-yaml-file>.yaml
Verify Static Pods (Task 7):

Bash
minikube ssh
sudo tee /etc/kubernetes/manifests/static-nginx.yaml << 'EOF'
apiVersion: v1
kind: Pod
metadata:
  name: static-nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
EOF
exit
📄 Deliverables
The output of the required commands has been piped into k8s_proof.txt. To view the consolidated proof of execution:

Bash
cat k8s_proof.txt
(You can find the completed k8s_proof.txt file in the root of this repository).

Maintained by
Mohanad Khairy
(Devops engineer)
