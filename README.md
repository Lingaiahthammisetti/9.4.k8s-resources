

```
1. Pod is not running -> check pod status( kubectl get pods -A, -A means Short for --all-namespaces. )

2. Pod in CrashLoopBackOff -> check pod logs ( kubectl logs <pod-name> -n <ns> )

3. Previous container logs -> check container logs ( kubectl logs <pod-name> -n <ns> --previous )

4. Pod stuck in ContainerCreating -> Describe pod ( kubectl describe pod <pod-name> -n <ns> )

5. ImagePullBackOff error -> check events ( kubectl describe pod <pod-name> -n <ns> | grep -i image )

6. Pod is Pending -> see why pod is pending ( kubectl describe pod <pod-name> -n <ns> )

7. Insufficient resources (CPU/Memory) -> Check cluster resources ( kubectl top nodes )

8. Node is Not Ready -> Check node status ( kubectl get nodes )

9. High CPU/Memory usage -> Check usage ( kubectl top pods -A )

10. Pod OOMKilled -> check pod events (kubectl describe pod <pod-name> -n <ns> | grep -i oom ) 
```









* Created repo for kubernetes objects and its understanding.

Namespace, pod, mutli-containers, labels, annotations, env, resource limits, configmap, pod-configmap, secret, pod-secrets, config-as-file-volume, secret-as-file-volume, cluster, node-port, load balancer, nginx-pod, replicatset, deployment. 



# Kubernetes Objects:
Pod
Namespace
Configmap
Secret
Deployment
Service
ReplicaSet
StatefulSet
DaemonSet
Job
CronJob
PersistentVolume (PV) & PersistentVolumeClaim (PVC)
Ingress


# In Kubernetes, does a Service selector need to match all the specified labels on a pod, or is one matching label enough to route traffic?
A Kubernetes Service will only match pods that have all the labels specified in its selector.
Having just one matching label is not enough — every key-value pair in the selector must be present on the pod.

# Note:  all labels values of selector from service need to match with pod label values. Pod can contain additional values, but service.


Important Points:
---------------------------
🔍 Service–Pod Matching Check
✅ Service Selector:
selector:
  name: frontend
  project: expense

✅ Pod Labels:
labels:
  name: frontend  
  project: expense
  environment: dev   # This is extra — perfectly fine

✅ Will the Service Match the Pod?
Yes, this Service will successfully match the Pod because:
The pod has all the labels specified in the service selector.
Extra labels on the pod (like environment: dev) are ignored by the selector — they don't interfere.


*ReplicaSet vs Deployment:
Summary Rules
Pod labels must match all keys in matchLabels	✅ Required
Pod can have extra labels beyond matchLabels	✅ Allowed
If Pod misses any selector label, ReplicaSet won't manage it	❌ Not allowed
