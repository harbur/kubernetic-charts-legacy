# Sample Secret

This Chart installs a dynamically created Secret and a Pod that consumes the secret.

We don't want to store sensitive information under version control, and since Charts are stored under version control we want to avoid exposing them. So instead of creating a password and storing it inside the Chart, we'll generate it on-the-fly during deployment.

Here is the Secret definition:

```
apiVersion: v1
kind: Secret
metadata:
  name: "{{.Release.Name}}-secret"
  annotations:
    "helm.sh/hook": pre-install
type: Opaque
data:
  password: {{ b64enc (randAlphaNum 32) }}
  username: {{ b64enc "user1" }}
```

Charts are written in [Helm](https://github.com/kubernetes/helm/) format, and they support various [template functions](https://github.com/kubernetes/helm/blob/master/docs/charts.md#templates-and-values). Here is a [list](https://github.com/Masterminds/sprig) of some functions.

* The `.Release.Name` is resolved during the deployment of the release. This enables to have multiple Chart releases on the same namespace.
* In our scenario we'll use `randAlphaNum 32` to generate a random alpha-numeric string of 32 characters.
* Secret values are stored in base64 inside Kubernetes, so we wrap the command with `b64enc` function.
* We want the secret to be deployed before deploying the accompanying Pod, so we add a special Helm annotation `"helm.sh/hook": pre-install` to activate a [Hook](https://github.com/kubernetes/helm/blob/master/docs/charts.md#hooks). Note that the secret will not be deleted automatically when the release is deleted and it needs to be handled separately.

Here is the Pod definition inside the same Chart:

```
apiVersion: v1
kind: Pod
metadata:
  name: "{{.Release.Name}}-pod"
spec:
  terminationGracePeriodSeconds: 0
  restartPolicy: Never
  containers:
  - name: alpine
    image: alpine:3.4
    command: ["echo", "hello $(password)"]
    env:
    - name: password
      valueFrom:
        secretKeyRef:
          name: {{.Release.Name}}-secret
          key: password
```

The Pod runs with environment `password` variable filled by the dynamically generated secret's `password` key value.
