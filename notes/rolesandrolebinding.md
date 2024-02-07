# Create a Role Object

    apiVersion: rbac.authorization.k8s.io/v1
    kind: Role
    metadata:
      name: developer
    rules:
    - apiGroups: [""]
      resources: ["pods"]
      verbs: ["list", "get", "create", "update", "delete"]
      resourceNames: ["blue", "orange"]
    - apiGroups: [""]
      resources: ["ConfigMap"]
      verbs: ["create"]

# Create a Role Binding

    apiVersion: rbac.authorization.k8s.io/v1
    kind: RoleBinding
    metadata:
      name: devuser-developer-binding
    subjects:
    - kind: User
      name: dev-user
      apiGroup: rbac.authorization.k8s.io
    roleRef:
      kind: Role
      name: developer
      apiGroup: rbac.authorization.k8s.io

# Check if one has access

    kubectl auth can-i create deployments
    kubectl auth can-i delete nodes
    kubectl auth can-i create deployments --as dev-user
    kubectl auth can-i delete nodes --as dev-user
