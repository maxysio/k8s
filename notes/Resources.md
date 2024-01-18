# Resources

## Resource Requests
- When the resource request is specified in the pod definition, a pod is scheduled only when the requested resources are guranteed to be available on the node

      apiVersion: v1
      kind: Pod
      metadata:
        name: simple-webapp-color
        labels:
          name: simple-webapp-color
      spec:
        containers:
        - name: simple-webapp-color
          image: simple-webapp-color
          ports:
          - containerPort: 8080
        resources:
          requests:
            memory: "1Gi"
            cpu: 1

  - 1 count of CPU is equal to 1 AWS vCPU or 1 GCP Core or 1 Azure Core or 1 Hyperthread
 
  ## Resource Limits
  
  - If resource limits are not set, pods can consumer all resources on a node and suffocate other pods
 
        apiVersion: v1
        kind: Pod
        metadata:
          name: simple-webapp-color
          labels:
            name: simple-webapp-color
        spec:
          containers:
          - name: simple-webapp-color
            image: simple-webapp-color
            ports:
            - containerPort: 8080
          resources:
            requests:
              memory: "1Gi"
              cpu: 1
            limits:
              memory: "2Gi"
              cpu: 2
