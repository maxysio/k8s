# Taking a snapshot for etcd

      export ETCDCTL_API=3
      export ETCDCTL_CACERT=/etc/etcd/ca.pem
      export ETCDCTL_CERT=/etc/etcd/kubernetes.pem
      export ETCDCTL_KEY=/etc/etcd/kubernetes-key.pem
      
      etcdctl member list --endpoints=https://127.0.0.1:2379 
      
      etcdctl snapshot save ~/etcd.backup

# Restore from Backup

      ETCDCTL_API=3 etcdctl --data-dir <data-dir-location> snapshot restore snapshot.db

- where <data-dir-location> is a directory that will be created during the restore process.
- Which means etcd service should now look for this new location for the data.
- Change the etcd manifest file at "/etc/kubernetes/manifests/etcd.yaml" and change "host path" for "etcd-data" to the backup folder

      - hostPath:
          path: /tmp/backup-restart
          type: DirectoryOrCreate
        name: etcd-data
