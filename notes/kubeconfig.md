# Kubeconfig

- Kubeconfig stores the kube api server, client key and certificate details for kubectl calls
- Default location for kubeconfig is $HOME/.kube/config
- kubeconfig file has 3 main sections: clusters, users and contexts
- Cluster section defines the different clusters like - dev, qa, prod
- Users section specifies the users who have access to different clusters like admin, dev, support
- Context section is a combination of cluster and user like admin@prod etc
- Sample file below

      apiVersion: v1
      kind: Config
      current-context: my-kube-dev@my-kube-playground
      clusters:
      - name: my-kube-playground
        cluster:
          certificate-authority: ca.crt
          server: https://my-kube-playground:6443
      context:
      - name: my-kube-admin@my-kube-playground
        context:
          cluster: my-kube-playground
          user: my-kube-admin
      - name: my-kube-dev@my-kube-playground
        context:
          cluster: my-kube-playground
          user: my-kube-dev
          namespace: dev
      users:
      - name: my-kube-admin
        user:
          client-certificate: admin.crt
          client-key: admin.key
      - name: my-kube-dev
        user:
          client-certificate: dev.crt
          client-key: dev.key

- View the config file using the command: kubectl config view
- To change the current context use the command: kubectl config use-context <context-name>
- Namespace can be used in a conext to specify which namespace be used when a specific context is used
