╔══════════════════════════════════════════════════════════════════════════════╗
║             ☸  KUBERNETES — ADVANCED SCHEDULING & POD MANAGEMENT            ║
║                            Assignment 7                                     ║
╚══════════════════════════════════════════════════════════════════════════════╝

  Configurations and execution logs for Kubernetes advanced scheduling tasks.
  Covers: nodeSelector · node affinity · taints · QoS classes · DaemonSets
          · static pods

  Stack: [ Kubernetes ]  [ Linux ]  [ Minikube ]


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  OVERVIEW
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  This project focuses on advanced pod scheduling techniques, node management,
  and how the Kubernetes scheduler handles resource allocation and constraints.


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ARCHITECTURE & CONCEPTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  01 · DIRECT PINNING
       Using `nodeName` to force a pod onto a specific node,
       bypassing the default scheduler entirely.

  02 · LABEL-BASED SCHEDULING
       Utilizing `nodeSelector` and Node Affinity rules:
         - Hard  →  requiredDuringSchedulingIgnoredDuringExecution
         - Soft  →  preferredDuringSchedulingIgnoredDuringExecution

  03 · TAINTS & TOLERATIONS
       Controlling which pods land on which nodes:
         - NoSchedule       →  new pods rejected
         - PreferNoSchedule →  new pods avoid if possible
         - NoExecute        →  existing pods evicted

  04 · QUALITY OF SERVICE (QoS)
       Pod classes based on CPU/memory requests & limits:
         - Guaranteed   →  requests == limits (both set)
         - Burstable    →  requests < limits
         - BestEffort   →  no requests or limits set

  05 · DAEMONSETS
       Ensures exactly one pod runs on every available node.
       Used here for: node-monitor deployment.

  06 · STATIC PODS
       Manifests written directly to /etc/kubernetes/manifests/
       and managed by the kubelet — not the API server.


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  PREREQUISITES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  ✦  A running Kubernetes cluster (e.g. minikube)
  ✦  kubectl installed and configured
  ✦  SSH access to cluster nodes (for static pod creation)


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  TASKS & EXECUTION LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  ┌──────┬──────────────────────┬──────────────────────────────────────────┬────────┐
  │  #   │  Feature             │  Description                             │ Status │
  ├──────┼──────────────────────┼──────────────────────────────────────────┼────────┤
  │  01  │  nodeName            │  Hard-pin pod to minikube node,          │  [OK]  │
  │      │                      │  bypassing the scheduler.                │        │
  ├──────┼──────────────────────┼──────────────────────────────────────────┼────────┤
  │  02  │  nodeSelector        │  Schedule pods via env=lab label.        │  [OK]  │
  │      │                      │  Observe Pending when label is absent.   │        │
  ├──────┼──────────────────────┼──────────────────────────────────────────┼────────┤
  │  03  │  Taints &            │  Test NoSchedule (rejection) and         │  [OK]  │
  │      │  Tolerations         │  NoExecute (eviction) effects.           │        │
  ├──────┼──────────────────────┼──────────────────────────────────────────┼────────┤
  │  04  │  Node Affinity       │  Hard + Soft rules based on disk types   │  [OK]  │
  │      │                      │  and availability zones.                 │        │
  ├──────┼──────────────────────┼──────────────────────────────────────────┼────────┤
  │  05  │  QoS Classes         │  3 pods with varying requests/limits     │  [OK]  │
  │      │                      │  → Guaranteed, Burstable, BestEffort.    │        │
  ├──────┼──────────────────────┼──────────────────────────────────────────┼────────┤
  │  06  │  DaemonSet           │  Deploy node-monitor — one pod per node. │  [OK]  │
  ├──────┼──────────────────────┼──────────────────────────────────────────┼────────┤
  │  07  │  Static Pods         │  Write manifest to manifests dir via     │  [OK]  │
  │      │                      │  minikube ssh for kubelet management.    │        │
  └──────┴──────────────────────┴──────────────────────────────────────────┴────────┘


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  HOW TO RUN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  1. Start your local cluster:
     ─────────────────────────
     minikube start


  2. Apply the manifests:
     ─────────────────────
     kubectl apply -f <your-yaml-file>.yaml


  3. Verify Static Pods (Task 07):
     ───────────────────────────────
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


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  DELIVERABLES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  All required command outputs have been piped into k8s_proof.txt.
  To view the consolidated proof of execution:

     cat k8s_proof.txt

  The completed k8s_proof.txt is located in the root of this repository.


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  MAINTAINED BY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Kareem Waleed AbdulHameed
  DevOps Engineer

══════════════════════════════════════════════════════════════════════════════
