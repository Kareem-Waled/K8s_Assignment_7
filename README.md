Kubernetes Advanced Scheduling & Pod Management
Configurations and execution logs covering nodeSelector, node affinity, taints & tolerations, QoS classes, DaemonSets, and Static Pods.
☸ Kubernetes
🐧 Linux
⎈ Minikube
Architecture & concepts
01
Direct pinning
Force a pod onto a specific node using nodeName, bypassing the scheduler entirely.
02
Label-based scheduling
nodeSelector and Node Affinity — both hard and soft rules — for flexible placement.
03
Taints & tolerations
Control which pods land on which nodes using NoSchedule, PreferNoSchedule, and NoExecute.
04
Quality of service
Guaranteed, Burstable, and BestEffort classes based on CPU/memory limits and requests.
05
Specialized pods
DaemonSets (one pod per node) and Static Pods managed directly by the kubelet.
Prerequisites
A running Kubernetes cluster (e.g. minikube)
kubectl
installed and configured
SSH access to cluster nodes (required for static pod creation)
Tasks & execution log
#	Feature	Description	Status
1	nodeName	Hard-pinning a pod to the minikube node, bypassing the scheduler.	done
2	nodeSelector	Scheduling based on env=lab label; observing Pending when label is absent.	done
3	Taints & Tolerations	Testing NoSchedule (rejection) and NoExecute (eviction) effects.	done
4	Node Affinity	Hard (requiredDuringScheduling) and soft (preferredDuringScheduling) rules by disk type and zone.	done
5	QoS Classes	Three pods with varying resource configs triggering Guaranteed, Burstable, and BestEffort.	done
6	DaemonSet	Deploying node-monitor — exactly one pod on every available node.	done
7	Static Pods	Writing a manifest to /etc/kubernetes/manifests/ via minikube ssh for kubelet management.	done
How to run
1
Start your local cluster
minikube start
2
Apply the manifests
kubectl apply -f <your-yaml-file>.yaml
3
Verify static pods (task 7)
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
4
View consolidated proof of execution
cat k8s_proof.txt
Maintained by Kareem Waleed AbdulHameed — DevOps Engineer
