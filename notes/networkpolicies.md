# Network Policies
- Can be used to define ingress and egress rules for pods for communication
- The following example allows ingress from pods with name api-pod on port 3306. This applies to pods with labels for role=db

      apiVersion: networking.k8s.io/v1
      kind: NetworkPolicy
      metadata:
        name: db-policy
      spec:
        podSelector:
          matchLabels:
            role: db
        policyTypes:
        - Ingress
        ingress:
        - from:
          - podSelector:
              matchLabels:
                name: api-pod
          ports:
          - protocol: TCP
            port: 3306
