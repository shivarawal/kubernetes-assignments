# Question 09: Resource Requests and Limits

## Objective

Learn how to define CPU and memory requests and limits for Pods, understand Kubernetes scheduling decisions, and observe resource enforcement.

---

## Step 1: Create a Pod with Resource Requests and Limits

Created `resource-pod.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resource-pod
spec:
  containers:
  - name: nginx
    image: nginx
    resources:
      requests:
        cpu: "250m"
        memory: "64Mi"
      limits:
        cpu: "500m"
        memory: "128Mi"
```

Applied the manifest:

```bash
kubectl apply -f resource-pod.yaml
```

Verified the pod was running:

```bash
kubectl get pods
```

Verified resource configuration:

```bash
kubectl describe pod resource-pod | grep -A 5 Requests
```

Output:

* CPU Request: 250m
* Memory Request: 64Mi
* CPU Limit: 500m
* Memory Limit: 128Mi

---

## Step 2: Check Node Allocatable Resources

Executed:

```bash
kubectl describe nodes | grep -A 5 Allocatable
```

Observed available CPU and memory resources on both master and worker nodes.

---

## Step 3: Simulate an OOMKilled Container

Created `oom-demo.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: oom-demo
spec:
  containers:
  - name: memory-hog
    image: polinux/stress
    command: ["stress"]
    args: ["--vm", "1", "--vm-bytes", "200M", "--vm-hang", "1"]
    resources:
      requests:
        memory: "50Mi"
      limits:
        memory: "100Mi"
```

Applied:

```bash
kubectl apply -f oom-demo.yaml
```

Monitored the pod:

```bash
kubectl get pod oom-demo -w
```

Observed:

```text
OOMKilled
CrashLoopBackOff
```

This demonstrated Kubernetes enforcing memory limits.

---

## Step 4: Configure LimitRange

Created `limit-range.yaml`:

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: default-limits
spec:
  limits:
  - type: Container
    default:
      cpu: "500m"
      memory: "256Mi"
    defaultRequest:
      cpu: "250m"
      memory: "128Mi"
```

Applied:

```bash
kubectl apply -f limit-range.yaml
```

Verified:

```bash
kubectl get limitrange
```

---

## Step 5: Verify Default Resource Assignment

Created `no-resources-pod.yaml` without defining requests or limits:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: no-resources-pod
spec:
  containers:
  - name: nginx
    image: nginx
```

Applied:

```bash
kubectl apply -f no-resources-pod.yaml
```

Verified automatic resource injection:

```bash
kubectl describe pod no-resources-pod | grep -A 10 Requests
```

Observed:

* CPU Request: 250m
* Memory Request: 128Mi

These values matched the LimitRange defaults.

---

## Key Learnings

* Requests determine scheduling decisions.
* Limits prevent containers from consuming excessive resources.
* Exceeding memory limits results in OOMKilled events.
* LimitRanges enforce default resource policies within namespaces.
* Proper resource management improves cluster stability and efficiency.

