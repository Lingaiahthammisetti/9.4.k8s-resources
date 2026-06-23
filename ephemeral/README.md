* emptyDir is used at the Pod level:
* hostPath is used to access the node's file system directly

# emptyDir definition:
    * emptyDir will hold the volume until pod is available, if the pod deleted, then emptyDir volume will be deleted.
    * we have a pod and inside that pod, we are using two containers. if our requirement is to share a volume between those two containers (inside the single pod) then we will use the emptyDir volume type.
    * we have to define a volume which will be located inside the pod. and those two containers will share the volume.

* Advantages -
    * If one container gets deleted then the new container will automatically access the existing volume.

* Disadvantages -
    * We have to remember that the volume where is located in pod, if pod deleted then that volume would also be deleted.
    * we cannot share the volume between two pods

# host path definition:
    * host path is a type of ephemeral volume where pod can access the underlaying host files and directories. this is dangerous, but only one case, we can use the hostpath to access the server logs and pushing them into elastic search.
    * From emptyDir volume, we have seen the major disadvantages, two pods won’t share volume. To overcome that problem, we can use the hostPath volume where we can map the volume of a container with a host machine (Kubernetes node).
    * In this case if pods get deleted also then we can access the volume from the host machine (Kubernetes node). And also, we can share the volume with two or multiple pods.

* Advantages -
    * We can share this between multiple pods.
    * If pods get deleted then still, we can access the data from host machine.

* Disadvantages -
    * In case of hostPath, it is located on a single node but after destroying the pod, the new pod can be created on a different node. in that case, the new pod cannot access its previous hostPath location.


# emptyDir:
Important note: emptyDir is a ephemeral volume, it will be created when pod the created. It will be deleted when the pod the deleted. Containers inside pod can access this storage. Upto now, we have discussed pod generated logs. Pod logs and underlying server logs also important.

```
git clone https://github.com/Lingaiahthammisetti/9.17.k8s-ephemeral-volumes.git
```
```
cd 9.17.k8s-ephemeral-volumes
```

* 01-emptyDir-Nginx-Filebeat.yaml

```
kubens default
```
```
kubectl apply -f 01-emptyDir-Nginx-Filebeat.yaml
```
```
kubectl get pods
```
```
kubectl get pod nginx -o jsonpath='{range .spec.containers[*]}{.name}{"\n"}{end}'
```
```
kubectl logs nginx -c filebeat
```

Here nginx pod contains two containers one nginx, second is filebeat.

Note: Pods logs and Node server logs also important.

# hostPath:
Important Note: hostPath is a type of ephemeral volume where a pod can access underlying host's files and directories. This is dangerous. But only one case we get the use of host path nothing but accessing the server logs and pushing them to the elasticSearch in read only mode.

We have different types of sets statefulsets, daemonsets.  
Note: daemonset will make sure a pod runs in every node. why this is useful meaning?

We have to access underlying node logs and push them to elastic search for log monitoring. if we run daemonset in Kubernetes, it make sure a pod is automatically created when new node is added. So we use daemonset in hostpath creation.

Note: Daemonset will be created in kube-system namespace only.

```
git clone https://github.com/Lingaiahthammisetti/9.17.k8s-ephemeral-volumes.git
```
```
cd 9.17.k8s-ephemeral-volumes
```

* 02-hostpath-DaemonSet.yaml

```
kubens default
```
```
kubectl apply -f 01-emptyDir-Nginx-Filebeat.yaml
```
```
kubectl get pods -n kube-system
```

Important: 3 pods are running in kube-system because 3 nodes are running right now.

# What is a Deamon Set?
    DeamonSet is used to run a pod on each and every node. whenever a new node is added to cluster, then Daemon Set automatically spin up the pod inside that node, whenever a node is deleted from the cluster then DaemonSet will delete the pod from that node. This is useful to collect the server logs and push them into the ElasticSearch for log monitoring. We use hostpath to access underlaying logs those should be read only.