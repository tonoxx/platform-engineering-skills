# Storage & Resource Exhaustion

This document covers diagnostic workflows for persistent volume issues, disk pressure conditions, and compute resource quota violations.

## 1. PersistentVolumeClaim Binding Failures

### Diagnosis
- **No Matching PV**: The PVC's requested storage class, access mode, or capacity does not match any available PersistentVolume. Check the PVC events for scheduling or provisioning errors.
- **StorageClass Misconfiguration**: The StorageClass referenced by the PVC does not exist, or its provisioner is not installed in the cluster. Verify the StorageClass name and provisioner availability.
- **Volume Topology Constraints**: In multi-zone clusters, a PV provisioned in one availability zone cannot be attached to a Pod scheduled in a different zone. Check volume and node zone labels.

### Resolution
- **Verify StorageClass** exists and the provisioner is healthy by checking its controller Pod status.
- **Adjust access modes** to match the storage backend's capabilities (ReadWriteOnce vs ReadWriteMany).
- **Use volumeBindingMode: WaitForFirstConsumer** to delay PV provisioning until a Pod is scheduled, ensuring zone alignment.

## 2. Node Disk Pressure

### Diagnosis
- **Container Image Accumulation**: Unused container images consume disk space over time. Kubelet's image garbage collection may not keep up with image churn on busy nodes.
- **Log Volume Growth**: Container logs written to stdout/stderr are stored on the node's filesystem. High-volume logging without rotation can fill the node's disk.
- **Ephemeral Storage Overuse**: Pods writing large amounts of data to emptyDir volumes or the container's writable layer consume ephemeral storage, triggering eviction when limits are exceeded.

### Resolution
- **Configure kubelet garbage collection** thresholds to aggressively reclaim unused images and dead containers.
- **Implement log rotation** at the application level or configure container runtime log rotation settings.
- **Set ephemeral storage limits** on Pods to prevent individual workloads from consuming excessive node disk space.

## 3. CPU and Memory Resource Quota Exhaustion

### Diagnosis
- **ResourceQuota Exceeded**: New Pods are rejected because the namespace's ResourceQuota for CPU or memory has been fully consumed. Check the quota's used vs hard limits.
- **LimitRange Violations**: The Pod's resource requests or limits fall outside the LimitRange bounds defined for the namespace. Verify the LimitRange min, max, and default values.
- **Over-Provisioned Requests**: Pods request significantly more resources than they actually use, artificially exhausting the quota. Compare actual usage against requested resources to identify over-provisioning.

### Resolution
- **Right-size resource requests** based on actual usage data from metrics-server or Prometheus to free quota capacity.
- **Increase the ResourceQuota** if the namespace legitimately needs more capacity.
- **Use VPA recommendations** from the Vertical Pod Autoscaler to automatically suggest appropriate resource requests.

## 4. Node NotReady Conditions

### Diagnosis
- **Kubelet Failure**: The kubelet process on the node has stopped or is unresponsive. The node controller marks the node as NotReady after the node-monitor-grace-period expires.
- **Network Partitioning**: The node cannot reach the API server due to network issues. The node appears NotReady from the control plane's perspective even though workloads may still be running locally.
- **System Resource Exhaustion**: The node has exhausted system-level resources (file descriptors, PID limits, kernel memory) causing kubelet to become unresponsive.
