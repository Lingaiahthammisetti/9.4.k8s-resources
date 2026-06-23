* Question: In Kubernetes, how do taints and tolerations work together, and why do tolerations not guarantee that a pod will be scheduled on a tainted node?
🔄 How Taints & Tolerations Work Together
Tolerations don’t guarantee scheduling on a tainted node — they only allow it. The scheduler still checks other conditions.

🧠 Important Facts
Tolerations do not attract pods to nodes. They only permit scheduling.
If no toleration matches a taint, the pod will never be scheduled to that node.
If a toleration does match, the scheduler then continues to evaluate:
Node labels / selectors
Node affinity/anti-affinity
Resource availability (CPU, memory, etc.)
Pod affinity/anti-affinity
Constraints like topology spread, priority, etc.


* Explain the following Kubernetes scheduling concepts briefly:
* Taints & Tolerations
* Node Affinity and Node Anti-Affinity
* Pod Affinity and Pod Anti-Affinity
* Required vs Preferred scheduling rules

1. Taints & Tolerations: Control which pods can run on a node. Nodes repel pods using taints, and pods accept them using tolerations.
2. Node Affinity: Schedules pods to specific nodes based on node labels (e.g., region, type).
3. Node Anti-Affinity: Pods avoid nodes with certain labels.
4. Pod Affinity: Schedules pods close to other pods (same zone/app).
5. Pod Anti-Affinity: Schedules pods away from certain pods (for high availability).
6. Required: Must meet the rule — hard constraint.
7. Preferred: Nice to have, but not enforced — soft constraint. 

* Important Point on Node Anti-Affinity:
Kubernetes doesn't have a nodeAntiAffinity field because all node selection is handled through nodeAffinity. Anti-affinity behavior is expressed using negative operators like NotIn or DoesNotExist within nodeAffinity.
