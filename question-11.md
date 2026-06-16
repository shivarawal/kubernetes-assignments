# Question 11: Persistent Volumes and Persistent Volume Claims

## Objective

* Create a PersistentVolume (PV)
* Create a PersistentVolumeClaim (PVC)
* Mount storage inside a Pod
* Verify data persistence after Pod deletion

---

## Step 1: Create a Persistent Volume

### pv.yaml

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: demo-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data
```

### Apply PV

```bash
kubectl apply -f pv.yaml
kubectl get pv
```

### Output

```text
NAME      CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS
demo-pv   1Gi        RWO            Retain           Available
```

---

## Step 2: Create a Persistent Volume Claim

### pvc.yaml

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: demo-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```

### Apply PVC

```bash
kubectl apply -f pvc.yaml
kubectl get pvc
```

### Output

```text
NAME       STATUS   VOLUME    CAPACITY   ACCESS MODES
demo-pvc   Bound    demo-pv   1Gi        RWO
```

---

## Step 3: Create a Pod Using PVC

### pvc-pod.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pvc-demo-pod
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: web-storage
      mountPath: /usr/share/nginx/html

  volumes:
  - name: web-storage
    persistentVolumeClaim:
      claimName: demo-pvc
```

### Deploy Pod

```bash
kubectl apply -f pvc-pod.yaml
kubectl get pods
```

### Output

```text
NAME           READY   STATUS
pvc-demo-pod   1/1     Running
```

---

## Step 4: Write Data to Persistent Storage

Access the Pod:

```bash
kubectl exec -it pvc-demo-pod -- sh
```

Inside the container:

```bash
echo "Persistent Storage Test" > /usr/share/nginx/html/index.html
exit
```

Verify:

```bash
kubectl exec pvc-demo-pod -- cat /usr/share/nginx/html/index.html
```

Output:

```text
Persistent Storage Test
```

---

## Step 5: Verify Data Persistence

Delete the Pod:

```bash
kubectl delete pod pvc-demo-pod
```

Recreate the Pod:

```bash
kubectl apply -f pvc-pod.yaml
```

Wait until the Pod is Running:

```bash
kubectl get pods -w
```

Verify the data again:

```bash
kubectl exec pvc-demo-pod -- cat /usr/share/nginx/html/index.html
```

Output:

```text
Persistent Storage Test
```

---

## Key Concepts Learned

* PersistentVolume (PV) represents actual storage in the cluster.
* PersistentVolumeClaim (PVC) is a request for storage by applications.
* Pods consume storage through PVCs.
* Data stored on Persistent Volumes survives Pod deletion.
* hostPath volumes use storage from the node's filesystem.

---

## Architecture

```text
Application Pod
       ↓
PersistentVolumeClaim (PVC)
       ↓
PersistentVolume (PV)
       ↓
Host Storage (/mnt/data)
```

---

## Conclusion

This assignment demonstrated how Kubernetes provides persistent storage using Persistent Volumes and Persistent Volume Claims. Even after deleting and recreating the Pod, the stored data remained intact, proving that application data can survive Pod lifecycle events. Persistent storage is essential for stateful applications such as databases and file storage systems.

