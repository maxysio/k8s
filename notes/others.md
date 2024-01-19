# Static Pods
- Can be created by placing pod definition files on the worker node location: /etc/kubernetes/manifests
- Kubelet reads the definitions and creates and mainatins pods from this folder
- Location of the folder is passed on to the Kubelet as the --pod-manifest-path in the kubelet.service file
- Or in the kubelet.service can have a config file using the config option have option staticPodPath
- static pods cannot be deleted from a kube api server
- Static pods can be used to deploy pods on the control plane

# Multiple Scheduler
- K8s can be extended to have multiple schedulers
- the default scheduler is named default-scheduler

      apiVersion: kubescheduler.config.k8s.io/v1
      kind: KubeSchedulerConfiguration
      profiles:
      - schedulerName: default-scheduler

- Custom schedulers can be defined as below

      apiVersion: kubescheduler.config.k8s.io/v1
      kind: KubeSchedulerConfiguration
      profiles:
      - schedulerName: custom-scheduler1

      apiVersion: kubescheduler.config.k8s.io/v1
      kind: KubeSchedulerConfiguration
      profiles:
      - schedulerName: custom-scheduler2
      leaderElection:
        leaderElect: true
        resourceNamespace: kube-system
        resourceName: lock-object-myscheduler
