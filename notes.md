Taints and Tolerances

- Taints are applied to Nodes
- Tolerances are applied to Pods
- Taints causes no pods to be scheduled on a Node unless it has tolerance for the taint
- Applying a taint
  
      kubectl taint nodes node-name key=value:taint-effect

- There are 3 possible values for taint-effect
  - NoSchedule - Pods will not be scheduled on the node
  - PreferNoSchedule - System will try to avoid scheduling any pods to the node but no gurantees
  - NoExecute - No pods will be scheduled on the pod and if any existing pods are on the node they will be evicted if they do not have tolerance for the taint
- Example for taint

      kubectl taint nodes node1 app=blue:NoSchedule

- Tolerances can be added to the Pod definition by adding segment tolerations

      apiVersion: 
