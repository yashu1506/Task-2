# Kubernetes Web Application Deployment

This guide provides step-by-step instructions for deploying a web application on Kubernetes using Minikube. The deployment includes 3 replicas, uses the `nginx:latest` image, exposes port 80, and includes both liveness and readiness probes with guaranteed QoS.

## Prerequisites

- [Minikube](https://minikube.sigs.k8s.io/docs/start/) installed and running
- [kubectl](https://kubernetes.io/docs/tasks/tools/) command-line tool installed

## Steps

### 1. Create the Deployment Configuration File

Create a file named `deployment.yaml`:

### 2. Start Minikube

Ensure Minikube is running:

```sh
minikube start
```

### 3. Apply the YAML Configuration

Apply the deployment configuration to your Minikube cluster:

```sh
kubectl apply -f deployment.yaml
```

### 4. Check the Deployment Status

Verify that the deployment is created and running properly:

```sh
kubectl get deployments
```

### 5. Check the Pods Status

Ensure all pods are running:

```sh
kubectl get pods
```
### 6. Access the Application

To access the application, use port forwarding:

```sh
kubectl port-forward service/web-app-service 8080:80
```

Open your browser and go to `http://localhost:8080` to see the application.

### 7. Verify Liveness and Readiness Probes

Describe the pod to check the status of the liveness and readiness probes:

```sh
kubectl describe pod <pod-name>
```

Replace `<pod-name>` with the actual name of one of your pods. Look for the `Liveness` and `Readiness` sections in the output.

### 8. Simulate Probe Failures

To test the liveness and readiness probes, you can simulate a failure:

1. Edit the deployment:

   ```sh
   kubectl edit deployment web-app-deployment
   ```

2. Change the probe path to an incorrect value:

   ```yaml
   livenessProbe:
     httpGet:
       path: /nonexistent
       port: 80
     initialDelaySeconds: 30
     periodSeconds: 10
   readinessProbe:
     httpGet:
       path: /nonexistent
       port: 80
     initialDelaySeconds: 5
     periodSeconds: 10
   ```

3. Save and observe the behavior using `kubectl describe pod <pod-name>`.