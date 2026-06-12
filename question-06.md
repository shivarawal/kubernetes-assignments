# Question 06: Working with Kubernetes Secrets

## Objective

* Create Kubernetes Secrets using literals.
* Understand how Secrets store sensitive information.
* Decode Secret values from Base64.
* Inject Secrets into Pods as environment variables.
* Verify Secret values inside containers.

---

## Creating a Generic Secret

### Command

```bash
kubectl create secret generic app-secret \
--from-literal=DB_USER=admin \
--from-literal=DB_PASSWORD=secret123
```

### Output

```text
secret/app-secret created
```

---

## Verifying Secrets

### List Secrets

```bash
kubectl get secrets
```

### Output

```text
NAME         TYPE     DATA   AGE
app-secret   Opaque   2      14s
```

### Describe Secret

```bash
kubectl describe secret app-secret
```

### Output

```text
Name:         app-secret
Namespace:    default

Type:  Opaque

Data
====
DB_USER:      5 bytes
DB_PASSWORD:  9 bytes
```

---

## Viewing Secret YAML

### Command

```bash
kubectl get secret app-secret -o yaml
```

### Output

```yaml
apiVersion: v1
data:
  DB_PASSWORD: c2VjcmV0MTIz
  DB_USER: YWRtaW4=
kind: Secret
metadata:
  name: app-secret
  namespace: default
type: Opaque
```

---

## Decoding Secret Values

### Decode Username

```bash
echo 'YWRtaW4=' | base64 -d
```

### Output

```text
admin
```

### Decode Password

```bash
echo 'c2VjcmV0MTIz' | base64 -d
```

### Output

```text
secret123
```

> **Note:** Kubernetes Secrets are Base64 encoded, not encrypted.

---

## Pod Manifest Using Secret

### File: `secret-pod.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-demo
spec:
  containers:
  - name: nginx-container
    image: nginx
    env:
    - name: DB_USER
      valueFrom:
        secretKeyRef:
          name: app-secret
          key: DB_USER
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: app-secret
          key: DB_PASSWORD
```

---

## Deploying the Pod

### Command

```bash
kubectl apply -f secret-pod.yaml
```

### Output

```text
pod/secret-demo created
```

---

## Verifying Pod Status

### Command

```bash
kubectl get pods
```

### Output

```text
NAME             READY   STATUS    RESTARTS   AGE
configmap-demo   1/1     Running   1          4h
secret-demo      1/1     Running   0          5m
```

---

## Verifying Secret Injection

### Access Pod

```bash
kubectl exec -it secret-demo -- sh
```

### Verify Environment Variables

```bash
env | grep DB
```

### Output

```text
DB_PASSWORD=secret123
DB_USER=admin
```

### Exit Container

```bash
exit
```

---

## Troubleshooting Encountered

### Issue 1: Incorrect Base64 String

Incorrect:

```bash
echo 'YWRtaW4' | base64 -d
```

Output:

```text
adminbase64: invalid input
```

Reason:

* Missing Base64 padding character (`=`).

Correct:

```bash
echo 'YWRtaW4=' | base64 -d
```

---

### Issue 2: Incorrect Command Usage

Incorrect:

```bash
kubectl 'c2VjcmV0MTIz' | base64 -d
```

Error:

```text
unknown command "c2VjcmV0MTIz" for "kubectl"
```

Correct:

```bash
echo 'c2VjcmV0MTIz' | base64 -d
```

---

### Issue 3: Wrong YAML File Name

Incorrect:

```bash
kubectl apply -f secret-demo.yaml
```

Error:

```text
the path "secret-demo.yaml" does not exist
```

Correct:

```bash
kubectl apply -f secret-pod.yaml
```

---

### Issue 4: Temporary API Server Connectivity Issue

Error:

```text
Unable to connect to the server: net/http: TLS handshake timeout
```

Resolution:

* Waited a few seconds and retried the command successfully.

---

## Learning Outcomes

* Secrets are used to store sensitive application data.
* `generic` Secrets create Opaque Secrets containing arbitrary key-value pairs.
* Secret values are Base64 encoded by default.
* Secrets can be injected into Pods using environment variables.
* Base64 encoding does not provide encryption; additional secret management solutions are recommended for production environments.

---

## Cleanup

Delete Pod:

```bash
kubectl delete pod secret-demo
```

Delete Secret:

```bash
kubectl delete secret app-secret
```

---

## Conclusion

Successfully created Kubernetes Secrets, verified their contents, decoded Base64 values, injected Secret values into a Pod, and validated that the application could securely access sensitive configuration data.

