# Pod Lifecycle Failures

This document covers diagnostic workflows for Pods that fail to start, crash repeatedly, or get unexpectedly terminated.

## 1. CrashLoopBackOff

### Diagnosis
- **Application Error**: The container starts but exits with a non-zero exit code. Check container logs for stack traces, configuration errors, or missing environment variables.
- **Liveness Probe Failure**: The container starts successfully but the liveness probe fails, causing kubelet to restart it. Verify probe endpoints, timeouts, and initial delay settings.
- **Resource Starvation**: The container is killed due to exceeding memory limits (OOMKilled). Check the Pod's last termination reason and adjust resource limits.

### Resolution
- **Fix the application error** by correcting configuration, environment variables, or dependencies.
- **Adjust probe configuration** by increasing initialDelaySeconds, timeoutSeconds, or failureThreshold to match the application's actual startup time.
- **Increase memory limits** if the container is consistently OOMKilled, or investigate memory leaks in the application.

## 2. ImagePullBackOff

### Diagnosis
- **Image Not Found**: The image tag does not exist in the registry. Verify the image name and tag are correct.
- **Authentication Failure**: The cluster lacks credentials to pull from a private registry. Check that the correct imagePullSecrets are configured on the Pod or ServiceAccount.
- **Registry Unavailable**: The container registry is temporarily unreachable. Check network connectivity from the node to the registry endpoint.

## 3. Pending Pods (Scheduling Failures)

### Diagnosis
- **Insufficient Resources**: No node has enough CPU or memory to satisfy the Pod's resource requests. Check node allocatable resources and pending Pod requests.
- **Node Affinity/Taints**: The Pod's nodeSelector, affinity rules, or tolerations do not match any available node. Review scheduling constraints against node labels and taints.
- **PVC Not Bound**: The Pod references a PersistentVolumeClaim that has not been bound. See the Storage reference for PVC troubleshooting.

## 4. Init Container Failures

### Diagnosis
- **Dependency Unavailable**: Init containers often wait for external dependencies (databases, config servers). Check init container logs for connection errors or timeouts.
- **Permission Issues**: The init container may lack the necessary security context or RBAC permissions to perform its setup tasks.

## 5. Pod Eviction

### Diagnosis
- **Node Pressure**: Kubelet evicts Pods when the node is under disk, memory, or PID pressure. Check node conditions and eviction thresholds.
- **Priority-Based Preemption**: Lower-priority Pods are evicted to make room for higher-priority Pods. Review PriorityClass assignments across the namespace.
