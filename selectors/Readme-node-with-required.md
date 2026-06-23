✅ Solution Recap
To make taints + tolerations + affinity work together:

Taint the node:
kubectl taint node ip-192-168-2-230.ec2.internal project=expense:NoSchedule


Label the node:
kubectl label node ip-192-168-2-230.ec2.internal project=expense


Define the pod with matching toleration and node affinity:
tolerations:
- key: "project"
  operator: "Equal"
  value: "expense"
  effect: "NoSchedule"

affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: project
          operator: In
          values:
          - expense

Node Setup Commands
Ensure the node is tainted and labeled like this:
kubectl taint node ip-192-168-2-230.ec2.internal project=expense:NoSchedule
kubectl label node ip-192-168-2-230.ec2.internal project=expense

Check:
kubectl get nodes --show-labels
kubectl describe node ip-192-168-2-230.ec2.internal | grep -i taint

Apply the Pod
kubectl apply -f pod.yaml
kubectl get pod with-node-affinity -o wide

* 