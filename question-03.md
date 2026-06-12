# Question 03: Exploring Kubernetes Namespaces

## Objective

- Understand Kubernetes Namespaces
- Create a Namespace
- Deploy workloads into a Namespace
- Change the default Namespace context
- Delete a Namespace

## Commands Used

### List Namespaces

```bash
kubectl get namespace
```

### Create Namespace

```bash
kubectl create ns dev
```

### Deploy Pod

```bash
kubectl run nginx --image=nginx -n dev
```

### Verify Pod

```bash
kubectl get pods -n dev
```

### Change Default Namespace

```bash
kubectl config set-context --current --namespace=dev
```

### Delete Namespace

```bash
kubectl delete namespace dev
```

## Learning Outcome

- Namespaces provide logical isolation within a cluster
- Workloads can be deployed into specific namespaces
- Default kubectl namespace context can be changed
- Deleting a namespace removes all resources inside it

## Errors Encountered

### Incorrect Context Command

```bash
kubectl config set-context --namespace=dev
```

Fixed using:

```bash
kubectl config set-context --current --namespace=dev
```

### Typographical Error

```bash
kubectl deleet namespace dev
```

Correct command:

```bash
kubectl delete namespace dev
```
