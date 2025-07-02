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