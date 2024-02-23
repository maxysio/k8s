# Volumes and Mounts

- The following is the definition of a Pod that generates a random number and writes it to a file at /opt/number.out
- A volume is identified data-volume and it is on the host of the container which is the node itself. The directory on the node is /data
- This is mounted to the containers location /opt
- So when the program runs it writes to the file number.out which is stored on the nodes directory /data

        apiVersion: v1
        kind: Pod
        metadata:
          name: random-number-generator
        spec:
          containers:
          - image: alpine
            name: alpine
            command: ["/bin/sh", "-c"]
            args: ["shuf -i 0-100 -n 1 >> /opt/number.out"]
            volumeMounts: 
            - mountPath: /opt
              name: data-volume
          volumes:
          - name: data-volume
            hostpath:
              path: /data
              type: Directory

- The above does not work for solutions with multiple nodes - data consistency becomes an issue
- In these cases external persistent storage can be used like awsElasticBlockStore

            volumnes:
            - name: data-volume
              awsElasticBlockStore:
                volumeID: <volumeId>
                fsType: ext4

# Persistent Volume Definition

        apiVersion: v1
        kind: PersistentVolume
        metadata:
          name: pv-vol1
        spec:
          accessModes: 
            - ReadWriteOnce
          capacity:
            storage: 1Gi
          hostPath:
            path: /tmp/data

# Persistent Volume Claims

        apiVersion: v1
        kind: PersistentVolumeClaim
        metadata:
          name: pvc-1
        spec:
          accessModes: 
            - ReadWriteOnce
          resources:
            requests:
              storage: 500Mi

- Once a Persistent Volume Claim is deleted, the persistent volume can either be Delete, Retain or Recycle (scrubbed and used for anaother claim) - this is done by setting the value of the property persistentVolumeReclaimPolicy to the respective values

- Persistent Volume claims can be specified in the Pod definition as shown below

        apiVersion: v1
        kind: Pod
        metadata:
          name: mypod
        spec:
          containers:
            - name: myfrontend
              image: nginx
              volumeMounts:
              - mountPath: "/var/www/html"
                name: mypd
          volumes:
            - name: mypd
              persistentVolumeClaim:
                claimName: myclaim
