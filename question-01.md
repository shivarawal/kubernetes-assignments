# Question 01: Deploying Your First Application on Kubernetes

## Objective

- Understand the basic Kubernetes object model
- Deploy a simple web application using kubectl
- Verify the deployment is running successfully

## Commands Used

### Verify Cluster Access

kubectl cluster-info


### Create Deployment


kubectl create deployment my-deployment --image=nginx


### Expose Deployment


kubectl expose deployment my-deployment --port=80 --type=NodePort


### Verify Pod


kubectl get pods


### Verify Service


kubectl get svc my-deployment


## Output

### Pod Status


NAME                             READY   STATUS    RESTARTS   AGE
my-deployment-5f8fc99b79-ppmrs   1/1     Running   0          60s


### Service Status


NAME            TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
my-deployment   NodePort   10.101.35.97   <none>        80:30325/TCP   25s

### Application URL


http://192.168.56.10:30325


## Learning Outcome

- Created a Deployment using kubectl
- Exposed the application using a NodePort Service
- Verified Pod and Service status
- Learned that Kubernetes Service types are case-sensitive (`NodePort` vs `nodeport`)
- Successfully accessed the application through the browser
