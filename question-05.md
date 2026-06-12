# Question 05: Using ConfigMaps

## Objective

* Create ConfigMaps using literals and files.
* Inject ConfigMap values into Pods as environment variables.
* Verify that the application can access the configuration values.
* Understand how ConfigMaps help separate configuration from application code.

---

## Creating ConfigMap Using Literals

### Command

```bash
kubectl create configmap app-config \
--from-literal=APP_ENV=production \
--from-literal=APP_PORT=8080
```

### Verification

```bash
kubectl describe configmap app-config
```

### Output

```text
Name:         app-config
Namespace:    default

Data
====
APP_PORT:
----
8080

APP_ENV:
----
production
```

---

## Creating ConfigMap Using a File

### Create Configuration File

```bash
echo 'LOG_LEVEL=info' > app.properties
```

### Create ConfigMap

```bash
kubectl create configmap file-config --from-file=app.properties
```

### Verify ConfigMap

```bash
kubectl describe configmap file-config
```

---

## Pod Manifest Using ConfigMap

### File: `configmap-pod.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: configmap-demo
spec:
  containers:
  - name: nginx-container
    image: nginx
    env:
    - name: APP_ENV
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: APP_ENV
    - name: APP_PORT
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: APP_PORT
```

---

## Deploying the Pod

### Apply Manifest

```bash
kubectl apply -f configmap-pod.yaml
```

### Output

```text
pod/configmap-demo created
```

### Verify Pod Status

```bash
kubectl get pods
```

### Output

```text
NAME             READY   STATUS    RESTARTS   AGE
configmap-demo   1/1     Running   0          20s
```

---

## Verifying Environment Variables

### Access the Pod

```bash
kubectl exec -it configmap-demo -- sh
```

### Check Environment Variables

```bash
env | grep APP
```

### Output

```text
APP_PORT=8080
APP_ENV=production
```

---

## Troubleshooting Encountered

### Issue 1: Typographical Error

Incorrect command:

```bash
kubectl cretae configmap file-config --from-file=app.properties
```

Error:

```text
unknown command "cretae" for "kubectl"
```

Solution:

```bash
kubectl create configmap file-config --from-file=app.properties
```

---

### Issue 2: Incorrect ConfigMap Key Reference

Incorrect YAML:

```yaml
key: 8080
```

Error:

```text
json: cannot unmarshal number into Go struct field ConfigMapKeySelector.key
```

Solution:

```yaml
key: APP_PORT
```

**Note:** The `key` field should reference the ConfigMap key name, not its value.

---

### Issue 3: Missing Container Name

Incorrect YAML:

```yaml
containers:
- image: nginx-container
  image: nginx
```

Error:

```text
spec.containers[0].name: Required value
```

Solution:

```yaml
containers:
- name: nginx-container
  image: nginx
```

---

### Issue 4: Typographical Error in Exec Command

Incorrect command:

```bash
kubectl exxec -it configmap-demo -- sh
```

Error:

```text
unknown command "exxec" for "kubectl"
```

Solution:

```bash
kubectl exec -it configmap-demo -- sh
```

---

## Learning Outcomes

* ConfigMaps are used to store non-sensitive configuration data.
* Configurations can be created from literals or files.
* Applications can consume ConfigMap values as environment variables.
* Separating configuration from container images improves portability and maintainability.
* Proper YAML syntax and accurate key references are critical for successful deployments.

---

## Cleanup

Delete the Pod:

```bash
kubectl delete pod configmap-demo
```

Delete ConfigMaps:

```bash
kubectl delete configmap app-config
kubectl delete configmap file-config
```

---

## Conclusion

Successfully created and utilized ConfigMaps to externalize application configuration in Kubernetes. Verified that environment variables were correctly injected into the container and gained practical troubleshooting experience by resolving common configuration and YAML errors.

