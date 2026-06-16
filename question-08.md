# Question 08: Rolling Updates and Rollbacks in Kubernetes

## Objective

Learn how to perform rolling updates on a Kubernetes Deployment and roll back to a previous stable version when an update fails.

---

## Step 1: Create a Deployment

Create a Deployment named `rollout-demo` using the `nginx:1.21` image.

```bash
kubectl create deployment rollout-demo --image=nginx:1.21
```

### Output

```text
deployment.apps/rollout-demo created
```

Verify the Deployment:

```bash
kubectl get deployments
kubectl get pods
```

---

## Step 2: Perform a Rolling Update

Update the Deployment image from `nginx:1.21` to `nginx:1.25`.

```bash
kubectl set image deployment/rollout-demo nginx=nginx:1.25
```

### Output

```text
deployment.apps/rollout-demo image updated
```

Monitor the rollout status:

```bash
kubectl rollout status deployment/rollout-demo
```

### Output

```text
deployment "rollout-demo" successfully rolled out
```

---

## Step 3: View Rollout History

Check the Deployment revision history.

```bash
kubectl rollout history deployment/rollout-demo
```

### Output

```text
deployment.apps/rollout-demo

REVISION  CHANGE-CAUSE
1         <none>
2         <none>
```

---

## Step 4: Verify Updated Image

Confirm that the Deployment is running the new image version.

```bash
kubectl describe deployment rollout-demo | grep Image
```

### Output

```text
Image: nginx:1.25
```

---

## Step 5: Simulate a Failed Update (Optional)

Update the Deployment to an invalid image.

```bash
kubectl set image deployment/rollout-demo nginx=nginx:invalid
```

Check rollout status:

```bash
kubectl rollout status deployment/rollout-demo
```

Verify pod status:

```bash
kubectl get pods
```

Example output:

```text
ImagePullBackOff
ErrImagePull
```

---

## Step 6: Roll Back to Previous Revision

Undo the failed rollout.

```bash
kubectl rollout undo deployment/rollout-demo
```

### Output

```text
deployment.apps/rollout-demo rolled back
```

Verify rollback status:

```bash
kubectl rollout status deployment/rollout-demo
```

Verify the image version:

```bash
kubectl describe deployment rollout-demo | grep Image
```

Expected output:

```text
Image: nginx:1.25
```

---

## Key Concepts Learned

* Created a Kubernetes Deployment.
* Performed a rolling update using `kubectl set image`.
* Monitored rollout progress using `kubectl rollout status`.
* Viewed Deployment revision history.
* Verified updated container images.
* Simulated a failed deployment update.
* Performed a rollback using `kubectl rollout undo`.

---

## Conclusion

Rolling updates enable zero-downtime application deployments by gradually replacing old Pods with new ones. Kubernetes maintains revision history, allowing administrators to quickly roll back to a stable version if an update introduces issues. This ensures application availability and minimizes operational risk during deployments.

