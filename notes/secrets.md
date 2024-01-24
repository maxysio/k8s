# Secrets

- Create a secret
- Inject into Pod

## Creating a Secret
- Imperative Method

      kubectl create secret generic <secret-name> --from-literal=key=value

      kubectl create secret generic <secret-name> --from-file=<filepath>

- Declartive way - create a secret definition file and apply

      apiVersion: v1
      kind: Secret
      metadata:
        name: app-secret
      data:
        DB_HOST: <base64 encoded value>
        DB_USER: <base64 encoded value>
        DB_Password: <base64 encoded value>
