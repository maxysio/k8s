# Deployment Strategy
- Recreate - causes downtime of apps/services
- Rolling - this is default behavior - no downtime
- Check rollout history using the following command

      kubectl rollout status deployment/<deployment-name>

-
