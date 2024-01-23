# Config Maps

- Used to centralize key value maps instead of defining them inside pod definition files
- Two stages:
  - Create the Config Map
  - Inject them into the Pod

## Creating the Config Map

- Imperative way
  
      kubectl create configmap \
        <configmap-name> --from-literal=<key>=<value>

      kubectl create configmap \
        <configmap-name> --from-file=<filepath>

- Declarative way - create a config definition file and use kubectl apply

      apiVersion: v1
      kind: ConfigMap
      metadata:
        name: app-config
      data:
        APP_COLOR: blue
        APP_MODE: prod

      
