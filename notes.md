# Taints and Tolerances

- Taints are applied to Nodes
- Tolerances are applied to Pods
- Taints causes no pods to be scheduled on a Node unless it has tolerance for the taint
- Applying a taint
  
      kubectl taint nodes <node-name> <key>=<value>:<taint-effect>

- There are 3 possible values for taint-effect
  - NoSchedule - Pods will not be scheduled on the node
  - PreferNoSchedule - System will try to avoid scheduling any pods to the node but no gurantees
  - NoExecute - No pods will be scheduled on the pod and if any existing pods are on the node they will be evicted if they do not have tolerance for the taint
- Example for taint

      kubectl taint nodes node1 app=blue:NoSchedule

- Tolerances can be added to the Pod definition by adding segment tolerations

      apiVersion: v1
      kind: Pod
      metadata:
        name: myapp-pod
      spec:
        containers:
        - name: nginx-container
          image: nginx
        tolerations:
        - key: "app"
          operator: "Equal"
          value: "blue"
          effect: "NoSchedule"

# Node Selectors

- Has limited functionality in terms of restricting pods to run on specific nodes
- Label a node

      kubectl label nodes <node-name> <label-key>=<label-value>
      Eg: kubectl label nodes node-1 size=large

- Specify the label selector in the pod definition

      apiVersion: v1
      kind: Pod
      metadata:
        name: myapp-pod
      spec:
        containers:
        - name: data-processor
          image: data-processor
        nodeSelector:
          size=large

# Node Affinity

- Specify which nodes the pod should run on
- This has more granular options

      apiVersion: v1
      kind: Pod
      metadata:
        name: myapp-pod
      spec:
        containers:
        - name: data-processor
          image: data-processor
        affinity:
          nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
              nodeSelectorTerms:
              - matchExpressions:
                - key: size
                  operator: In
                  - Large

## Node Affinity Types

### requiredDuringSchedulingIgnoredDuringExecution
  - This specifies that a node with the right affinity labels are required for pod scheduling
  - If a node is not found with the right affinity labels, then the pod will not be scheduled
  - Once a pod is executing, any changes to affinity will be ignored
      
### preferredDuringSchedulingIgnoredDuringExecution
  - This specifies that if a matching node is not found, then the pod will be scheduled in any other available node
  - Once a pod is executing, any changes to affinity will be ignored

### Two new Types coming up in a newer version of K8s will have "requiredDuringSchedulingRequiredDuringExecution" and "preferredDuringSchedulingRequiredDuringExecution" - here only the execution portion has changed where in case affinity changes during execution, may evict/terminate pods 
 

