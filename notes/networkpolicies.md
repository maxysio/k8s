# Network Policies
- Can be used to define ingress and egress rules for pods for communication
- The following example allows ingress from pods with name api-pod on port 3306. This applies to pods with labels for role=db and in the namespace with name prod and also from IP CIDR blocks
- It also allows egress from the db pod to CIDR block 192.1.1.1 on TCP port 80

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
        - Egress
        ingress:
        - from:
          - podSelector:
              matchLabels:
                name: api-pod
            namespaceSelector:
              matchLabels:
                name: prod
          - ipBlock:
              cidr: 192.1.0.0/32
          ports:
          - protocol: TCP
            port: 3306
        egress:
        - to:
          - ipBlock:
              cidr: 192.1.1.1/32
          ports:
          - protocol: TCP
            port: 80
