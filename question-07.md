# Question 07: Scaling Applications Manually

## Objective

Learn how to manually scale Kubernetes applications using Deployments and ReplicaSets, and understand Kubernetes self-healing capabilities.

---

## Environment Details

* Kubernetes Cluster: kubeadm-based cluster
* Namespace: default
* Application: Nginx
* Deployment Name: `scale-demo`

---

## Step 1: Create a Deployment

Create a deployment running the Nginx image.

```bash
kubectl create deployment scale-demo --image=nginx
```

### Output

```bash
deployment.apps/scale-demo created
```

Verify the deployment:

```bash
kubectl get deployments
```

---

## Step 2: Scale the Deployment to 5 Replicas

Increase the number of pod replicas from 1 to 5.

```bash
kubectl scale deployment scale-demo --replicas=5
```

### Output

```bash
deployment.apps/scale-demo scaled
```

Verify the deployment scaling:

```bash
kubectl get deployment scale-demo
```

Expected Output:

```text
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
scale-demo   5/5     5            5           <age>
```

---

## Step 3: Watch Pod Creation

Monitor pods being created in real time.

```bash
kubectl get pods -w
```

Observed Pod Lifecycle:

```text
Pending
↓
ContainerCreating
↓
Running
```

This demonstrates how Kubernetes schedules and starts containers automatically.

---

## Step 4: Verify ReplicaSet

Check the ReplicaSet managing the deployment.

```bash
kubectl get rs
```

Output:

```text
NAME                   DESIRED   CURRENT   READY   AGE
scale-demo-9bf8b4bbc   5         5         5       8m
```

### Understanding ReplicaSet Status

* DESIRED: Number of replicas requested.
* CURRENT: Number of pods currently created.
* READY: Number of healthy pods available.

Architecture:

Deployment → ReplicaSet → Pods

---

## Step 5: Demonstrate Self-Healing

Delete one pod manually.

```bash
kubectl delete pod scale-demo-9bf8b4bbc-9bmnt
```

Watch pod recreation:

```bash
kubectl get pods -w
```

Observation:

Kubernetes automatically created a replacement pod to maintain the desired replica count.

Example:

```text
Deleted Pod:
scale-demo-9bf8b4bbc-9bmnt

New Pod Created:
scale-demo-9bf8b4bbc-ksd86
```

### Self-Healing Workflow

```text
Desired Replicas = 5
Actual Replicas = 4 (after deletion)
ReplicaSet detects mismatch
New Pod created automatically
Actual Replicas = 5
```

---

## Step 6: Scale Down the Deployment

Reduce replicas from 5 to 2.

```bash
kubectl scale deployment scale-demo --replicas=2
```

### Output

```bash
deployment.apps/scale-demo scaled
```

Verify:

```bash
kubectl get pods
```

Expected Result:

Only two pods remain in the Running state.

---

## Key Concepts Learned

### Deployment

Manages application updates and desired replica counts.

### ReplicaSet

Ensures the specified number of pod replicas are always running.

### Manual Scaling

Adjusts application capacity based on workload requirements.

### Self-Healing

Automatically recreates failed or deleted pods to maintain desired state.

### Desired State Management

Kubernetes continuously works to match the actual cluster state with the desired state.

---

## Commands Used

```bash
kubectl create deployment scale-demo --image=nginx

kubectl scale deployment scale-demo --replicas=5

kubectl get pods -w

kubectl get rs

kubectl delete pod <pod-name>

kubectl scale deployment scale-demo --replicas=2

kubectl get pods
```

---

## Outcome

Successfully:

* Created a Deployment.
* Scaled the application from 1 to 5 replicas.
* Verified ReplicaSet behavior.
* Demonstrated Kubernetes self-healing capability.
* Scaled the application down to 2 replicas.

This assignment provided practical experience with Kubernetes scaling and high availability concepts.

