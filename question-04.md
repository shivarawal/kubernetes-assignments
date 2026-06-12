# Question 04: Working with Kubernetes Services

## Objective

- Understand ClusterIP and NodePort Services
- Expose applications internally and externally
- Test service communication

## Commands Used

### Create Deployment

```bash
kubectl create deployment web --image=nginx --replicas=2
```

### Expose Deployment

```bash
kubectl expose deployment web \
--port=80 \
--target-port=80 \
--name=web-svc
```

### Verify Services

```bash
kubectl get svc
```

### Internal Connectivity Test

```bash
kubectl run test \
--image=busybox \
--rm -it \
--restart=Never \
-- wget -qO- http://web-svc
```

### Convert ClusterIP to NodePort

```bash
kubectl edit svc web-svc
```

Change:

```yaml
type: ClusterIP
```

to:

```yaml
type: NodePort
```

## Learning Outcomes

- Services provide stable networking for Pods.
- ClusterIP exposes applications internally.
- NodePort exposes applications externally.
- Internal communication can be tested using Busybox Pods.

## Troubleshooting

### Issue 1

Namespace context pointing to deleted namespace.

Fixed using:

```bash
kubectl config set-context --current --namespace=default
```

### Issue 2

Wrong wget syntax:

Incorrect:

```bash
wget -qo-
```

Correct:

```bash
wget -qO-
```

### Issue 3

Incorrect service name:

```bash
websvc
```

Correct:

```bash
web-svc
```
