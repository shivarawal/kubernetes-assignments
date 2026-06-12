# Question 02: Understanding Kubernetes Pods

## Objective

- Define what a Pod is in Kubernetes
- Create a Pod using a YAML manifest
- Inspect Pod logs and status

## Pod Manifest

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: busybox
    image: busybox
    command: ['sh', '-c', 'echo Hello Kubernetes && sleep 3600']
```

## Commands Used

### Create Pod

```bash
kubectl apply -f pod.yaml
```

### Verify Pod

```bash
kubectl get pods
```

### Describe Pod

```bash
kubectl describe pod my-pod
```

### View Logs

```bash
kubectl logs my-pod
```

## Output

```text
Hello Kubernetes
```

## Learning Outcome

- Pods are the smallest deployable unit in Kubernetes
- Learned to create Pods using YAML
- Inspected Pod details using describe
- Viewed container logs
