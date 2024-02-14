# Static Pods
- Can be created by placing pod definition files on the worker node location: /etc/kubernetes/manifests
- Kubelet reads the definitions and creates and mainatins pods from this folder
- Location of the folder is passed on to the Kubelet as the --pod-manifest-path in the kubelet.service file
- Or in the kubelet.service can have a config file using the config option have option staticPodPath
- static pods cannot be deleted from a kube api server
- Static pods can be used to deploy pods on the control plane

# Multiple Schedulers
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

- Custom schedulers can be deployed as a Pod in a deinition
- Add property spec.schedulerName to a pod definition to specify that the pod should be scheduled using that the scheduler
- If scheduler is not available or not correctly setup, the pod will go to pending state

# Scheduler Priority
- PriorityClass can be defined which can be used to define the priority of a Pod

# List all clusters: 
      
      k config get-clusters

# Manifest File location

      /etc/kubernetes/manifests

# View Certificate Details in Unix

      openssl x509 -in (path to certificate and certificate filename) -text -noout

# CSR Object YAML

      apiVersion: certificates.k8s.io/v1
      kind: CertificateSigningRequest
      metadata:
        name: akshay
      spec:
        groups:
        - system:authenticated
        request: <Paste the base64 encoded value of the CSR file>
        signerName: kubernetes.io/kube-apiserver-client
        usages:
        - client auth

# Image Security

- Image name is usually {registry}/{user-account}/{image-name} like docker.io/library/nginx

## Using Private Registry

- Login to private regsitry using docker login command

        docker login <registry-name>
  
- In the pod specification, replace the image name with the entire path of the private registry and image name
- Images are pulled and run by the docker runtime on worker nodes
- The docker runtime requires credentials to access the private registry
- Create a secret object of type docker registry

        kubectl create secret docker-registry regcred \
              --docker-server=                          \
              --docker-username=                        \
              --docker-password=                        \
              --docker-email=

- Add an imagePullSecrets section in the pod specs to the new secret

            spec:
            containers:
            - image: myprivateregistry.com:5000/nginx:alpine
              imagePullPolicy: IfNotPresent
              name: nginx
            imagePullSecrets:
            - name: private-reg-cred
