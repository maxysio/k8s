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

## Injecting into Pods

        apiVersion: v1
        kind: Pod
        metadata:
          name: simple-webapp-color
          labels:
            name: simple-webapp-color
        spec:
          containers:
          - name: simple-webapp-color
            image: simple-webapp-color
            ports:
            - containerPort: 80
            envFrom:
            - secretRef:
                name: app-secret

- Secrets can be mounted as Volumes as well

        apiVersion: v1
        kind: Pod
        metadata:
          name: simple-webapp-color
          labels:
            name: simple-webapp-color
        spec:
          containers:
          - name: simple-webapp-color
            image: simple-webapp-color
            ports:
            - containerPort: 80
            volumes:
            - name: app-secret-volume
              secret:
                secretName: app-secret
