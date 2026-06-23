* Important Point on Node Affinity:
Node affinity:
Required: Must schedule on matching nodes (requiredDuringSchedulingIgnoredDuringExecution).
Preferred: Prefer matching nodes but allow others (preferredDuringSchedulingIgnoredDuringExecution).

Node affinity = node labels (for preference or requirement)
Taints/tolerations = node taints + pod permissions to tolerate them
you do not need to taint nodes to use node affinity.

Example:
nodeSelectorTerms:
- matchExpressions:
  - key: disktype
    operator: In
    values:
    - ssd
  - key: region
    operator: NotIn
    values:
    - us-west
- matchExpressions:
  - key: cpu
    operator: Gt
    values:
    - "4"
This means:

Nodes with disktype=ssd AND region != us-west
OR
Nodes with cpu > 4

* Important Point on Node Anti-Affinity:
Kubernetes doesn't have a nodeAntiAffinity field because all node selection is handled through nodeAffinity. Anti-affinity behavior is expressed using negative operators like NotIn or DoesNotExist within nodeAffinity.

* How do you implement node anti-affinity in Kubernetes?
 Use nodeAffinity with NotIn or DoesNotExist operators under required or preferred scheduling rules

 Node anti-affinity is done using nodeAffinity with:
 Required: Use operator: NotIn or DoesNotExist under requiredDuringSchedulingIgnoredDuringExecution to strictly avoid certain nodes.
 Preferred: Use the same operators under preferredDuringSchedulingIgnoredDuringExecution to try to avoid them.