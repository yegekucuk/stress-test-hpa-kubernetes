
# HPA Stress Tool

This repository contains the configuration for deploying a Horizontal Pod Autoscaler (HPA) stress testing tool in a Kubernetes cluster. The tool uses an Alpine-based image to simulate CPU load and helps in testing the HPA configuration.

## Deployment

The deployment consists of a Deployment and a Horizontal Pod Autoscaler (HPA) configuration.

### Requirements

- A running Kubernetes cluster
- `kubectl` configured to interact with your cluster

### Steps

1. **Apply the Deployment and HPA Configuration**

   Apply the configuration using the following command and apply all files:

   ```sh
   cd hpa
   kubectl apply -f .
   ```

2. **Verify the Deployment and HPA**

   Check if the Deployment is created successfully:

   ```sh
   kubectl get deployments
   ```

   You should see `hpa-stress-tool` in the list of deployments.

   Check the status of the HPA:

   ```sh
   kubectl get hpa
   ```

   You should see `hpa-stress-tool` in the list of HPAs.

3. **Access the Pod**

   Get the name of the running pod:

   ```sh
   kubectl get pods -l app=hpa-stress-tool
   ```

   Use the pod name to open a terminal session in the pod:

   ```sh
   kubectl exec -it <pod-name> -- /bin/sh
   ```

## Stress Test

To apply a stress test on the pod, run the following command inside the pod's terminal:

```sh
stress-ng --cpu 4 --metrics
```

This command will start a CPU stress test with 4 workers and provide metrics output.

## Notes

- The Deployment requests 100m CPU and limits it to 500m CPU.
- The HPA is configured to maintain an average CPU utilization of 80% across all pods.
- The HPA can scale the number of pods from a minimum of 1 to a maximum of 5 based on the CPU utilization.

## Cleanup

To remove the deployment and HPA, run:

```sh
kubectl delete -f .
```

This will delete the Deployment and the HPA associated with `hpa-stress-tool`.
