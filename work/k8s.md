# Kubernetes Configuration

## Set Up a Kubernetes Cluster with EKS
Kubernetes is available as a hosted service in AWS as Elastic Kubernetes Service (EKS).

- Control Plane With EKS: When you are working with EKS, you do not have access to the control plane. The control plane is entirely managed by the AWS EKS team and generally not configurable to the end user. The main takeaway is while you likely don’t need to configure the control plane, you should acknowledge its existance and that you can’t configure the control plane when you use EKS.

- EKS Setup: EKS involves setting up a cluster and node groups.

### Cluster Setup

- The cluster role is relatively straightforward. It should have the following policy attached:

```
AmazonEKSClusterPolicy
```

### Node Group Setup

The node group must use a role that is different from the cluster role. At a minimum, it should have these policies:

```
AmazonEKSWorkerNodePolicy
AmazonEC2ContainerRegistryReadOnly
AmazonEKS_CNI_Policy
AmazonEMRReadOnlyAccessPolicy_v2
```

You may need to add additional policies depending on your own use cases. For example, you would need to add additional policies to manage S3 access.

### Connecting via Port Forwarding

- Set up port-forwarding to `postgresql-service`

```
kubectl port-forward service/postgresql-service 5433:5432 &
```

