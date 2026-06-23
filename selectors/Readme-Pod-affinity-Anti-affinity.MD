✅ Affinity and Anti-Affinity: What Is Checked?
Affinity and anti-affinity rules in Kubernetes work in two parts:
LabelSelector → which pods to match (based on their labels) using label name one pod with labelSelector in another pod.
topologyKey → where to evaluate the match (node-level key like hostname or zone)

🔍 LabelSelector → Matches Pods
You define which pods you're looking for based on labels.
Example:
labelSelector:
  matchExpressions:
  - key: name
    operator: In
    values:
    - pod-1
This means: "I'm interested in pods that have name=pod-1." So yes — matching is always done using pod labels.

🗺️ topologyKey → Defines the Scope of Scheduling
Once the matching pods are found by labels, the scheduler uses topologyKey to decide where those matches matter.

Common keys:
kubernetes.io/hostname → match within the same node
topology.kubernetes.io/zone → match within the same availability zone
topology.kubernetes.io/region → match within the same cloud region (less common)

Sample values:
kubernetes.io/hostname=node-1
topology.kubernetes.io/zone=us-east-1a
topology.kubernetes.io/region=us-east-1


✅ Examples from Your Manifest:
Pod 2: Affinity
topologyKey: topology.kubernetes.io/zone
→ Pod 2 will prefer to be scheduled in the same zone as pods with label name=pod-1.

Pod 3: Anti-Affinity
topologyKey: kubernetes.io/hostname
→ Pod 3 will avoid being placed on the same node (hostname) as pods with label name=pod-1.

So the scheduler:
First finds matching pods using labels
Then enforces the affinity rule based on the nodes (via topologyKey)

kubernetes.io/hostname: ip-10-0-1-23.ec2.internal --> node name
or
kubernetes.io/hostname: node-1

topology.kubernetes.io/zone: us-east-1a
topology.kubernetes.io/region: us-east-1

LabelSelector → to find matching pods
topologyKey → to decide the scope of influence (node, zone, region)


kubernetes.io/hostname=node-1
topology.kubernetes.io/zone=us-east-1a
topology.kubernetes.io/region=us-east-1
