# Networking & DNS

This document covers diagnostic workflows for service connectivity failures, DNS resolution errors, and traffic routing issues in Kubernetes.

## 1. NetworkPolicy Misconfiguration

### Diagnosis
- **Default Deny Effect**: When a NetworkPolicy selects a Pod, all traffic not explicitly allowed is denied. Verify that both ingress and egress rules cover the required traffic paths.
- **Label Mismatch**: NetworkPolicy selectors use labels to identify target Pods. A mismatch between the policy's podSelector and actual Pod labels silently drops traffic without generating errors.
- **Namespace Isolation**: Cross-namespace traffic requires namespaceSelector in the NetworkPolicy. Missing namespace selectors block inter-namespace communication even when port rules are correct.

### Resolution
- **Audit active policies** by listing all NetworkPolicies in the namespace and verifying their selectors match the affected Pods.
- **Test incrementally** by temporarily relaxing policies to confirm that the NetworkPolicy is the root cause before tightening rules.

## 2. Service & Endpoint Resolution

### Diagnosis
- **No Endpoints**: The Service has no backing Endpoints because no Pods match the Service's selector. Verify that the selector labels match running Pod labels exactly.
- **Port Mismatch**: The Service's targetPort does not match the container's actual listening port. Check that the container exposes the expected port and the Service definition references it correctly.
- **Headless Service Pitfalls**: Headless Services (clusterIP: None) return Pod IPs directly. Clients must handle multiple A records and Pod IP changes.

## 3. CoreDNS Issues

### Diagnosis
- **DNS Timeout**: Pods receive SERVFAIL or timeout errors when resolving Service names. Check that CoreDNS Pods are running and healthy, and that the kube-dns Service has active Endpoints.
- **Search Domain Misconfiguration**: Pods use the cluster's search domain to resolve short names. Custom dnsConfig or dnsPolicy settings can override this behavior and break resolution.
- **Upstream DNS Failure**: CoreDNS forwards external queries to upstream resolvers. If the upstream resolver is unreachable, external DNS lookups fail while cluster-internal resolution continues to work.

## 4. Ingress & Gateway Misrouting

### Diagnosis
- **Backend Not Found**: The Ingress references a Service that does not exist or has no ready Endpoints. Verify the Service name, port, and namespace in the Ingress spec.
- **TLS Termination Errors**: Certificate mismatches between the Ingress TLS secret and the requested hostname cause connection failures. Verify that the Secret contains the correct certificate and key for the host.
- **Path Matching Issues**: Different Ingress controllers interpret path matching differently (prefix vs exact). Verify the pathType setting matches the intended routing behavior.

## 5. CNI Plugin Diagnostics

### Diagnosis
- **Pod Network Unavailable**: If the CNI plugin (Calico, Cilium, Flannel) is not running on a node, Pods scheduled to that node cannot obtain an IP address and remain in ContainerCreating state.
- **IP Exhaustion**: The Pod CIDR range may be exhausted, preventing new Pods from receiving IP addresses. Check the IPAM allocation status for the affected node.
