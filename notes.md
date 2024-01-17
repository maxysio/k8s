Taints and Tolerances

- Taints are applied to Nodes
- Tolerances are applied to Pods
- Taints causes no pods to be scheduled on a Node unless it has tolerance for the taint
- Applying a taint
  
      kubectl taint nodes node-name key=value:taint-effect

  
