# Question 12 – Kubernetes Jobs and cronjobs 

## Objective
Create a Job that calculates Pi using a Perl container.

## Steps Performed

1. Created `pi-job.yaml` with:
   - apiVersion: batch/v1
   - kind: Job
   - perl:5.34 image
   - command to print Pi value
   - restartPolicy: Never
   - backoffLimit: 4

2. Applied the Job:

```bash
kubectl apply -f pi-job.yaml
```

3. Monitored Job execution:

```bash
kubectl get jobs -w
```

Output:

```text
NAME   COMPLETIONS   DURATION   AGE
pi     1/1           19s        19s
```

4. Verified the pod created by the Job:

```bash
kubectl get pods
```

5. Retrieved Job output:

```bash
kubectl logs -l job-name=pi
```

## Learning Outcomes

- Understood the purpose of Kubernetes Jobs.
- Learned that Jobs run tasks until completion.
- Used `kubectl get jobs -w` to monitor execution.
- Used `kubectl logs` to verify Job output.
- Understood how `backoffLimit` controls retries on failure.
