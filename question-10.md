# Question 10: Labels and Selectors

## Objective

Understand how to use labels and selectors in Kubernetes to organize resources and enable Services to discover Pods.

---

## Step 1: Create a Pod with Labels

Created a Pod named `labeled-pod` with multiple labels.

### Command

```bash
kubectl run labeled-pod \
  --image=nginx \
  --labels=app=web,env=dev,tier=frontend
```

### Output

```text
pod/labeled-pod created
```

---

## Step 2: Filter Pods Using Label Selectors

Verified that Kubernetes selectors can filter resources based on labels.

### Command

```bash
kubectl get pods -l app=web,env=dev
```

### Output

```text
NAME          READY   STATUS    RESTARTS   AGE
labeled-pod   1/1     Running   0          34s
```

---

## Step 3: Add a New Label

Added an additional label (`version=v1`) to the existing Pod.

### Command

```bash
kubectl label pod labeled-pod version=v1
```

### Output

```text
pod/labeled-pod labeled
```

---

## Step 4: Remove a Label

Removed the previously added label.

### Command

```bash
kubectl label pod labeled-pod version-
```

### Output

```text
pod/labeled-pod unlabeled
```

---

## Step 5: Create a Service Using a Selector

Exposed the Pod through a Service using the `app=web` selector.

### Command

```bash
kubectl expose pod labeled-pod --port=80 --selector=app=web
```

### Output

```text
service/labeled-pod exposed
```

---

## Step 6: Verify Service Endpoints

Confirmed that the Service successfully discovered the Pod through label matching.

### Command

```bash
kubectl get endpoints labeled-pod
```

### Output

```text
NAME          ENDPOINTS           AGE
labeled-pod   192.168.171.99:80   15s
```

---

## Key Concepts Learned

* Labels are key-value pairs attached to Kubernetes objects.
* Labels help organize and categorize resources.
* Selectors are used to filter Kubernetes objects based on labels.
* Services use selectors to automatically identify target Pods.
* Endpoints are created dynamically when a Service matches Pods with corresponding labels.

### Service Discovery Flow

```text
Pod Labels:
app=web
env=dev
tier=frontend

        ↓

Service Selector:
app=web

        ↓

Endpoints Created:
192.168.171.99:80
```

---

## Conclusion

This assignment demonstrated how labels and selectors work together in Kubernetes. Labels provide a flexible way to organize resources, while selectors enable efficient resource filtering and service-to-pod communication. Understanding this mechanism is essential because Services rely on label selectors to route traffic to the correct application Pods.

