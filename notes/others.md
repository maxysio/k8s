# Static Pods
- Can be created by placing pod definition files on the worker node location: /etc/kubernetes/manifests
- Kubelet reads the definitions and creates and mainatins pods from this folder
- Location of the folder is passed on to the Kubelet as the --pod-manifest-path
