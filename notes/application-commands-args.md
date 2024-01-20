# Application Commands

- CMD allows to specify the command and params in the format CMD sleep 5 or CMD["sleep","5"]
- ENTRYPOINT allows to specify the command the params are passed in as arguments when running the container like ENTRYPOINT["sleep"] and command line parameters will get appended
- If command line params are not specified, default values can be taken from the CMD["5"] value but will be overwritten by the command line parameter if specified.

      apiVersion: v1
      kind: Pod
      metadata:
        name: ubuntu-sleeper-pod
      spec:
        containers:
        - name: ubuntu-sleeper
          image: ubuntu-sleeper
          command: ["sleep2.0"]
          args: ["10"]
