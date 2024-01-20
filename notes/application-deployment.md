# Deployment Strategy
- Recreate - causes downtime of apps/services
- Rolling - this is default behavior - no downtime
- Check rollout history using the following command

      kubectl rollout status deployment/<deployment-name>

- During an upgrade, the deployment creates a new ReplicaSet and peforms a rolling update starting pods on the new replicaset and deleting pods on the old one
- Rollout can be done a deployment using the following command:

      kubectl rollout undo deployment/<deployment-name>

