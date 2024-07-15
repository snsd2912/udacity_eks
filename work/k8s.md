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

### Connect to EKS Cluster using Cloud Shell

Run command:

```
aws eks --region region update-kubeconfig --name cluster_name
```

## Database Deployment

- Configuration files for database is at `./deployments/database` foler.

### Deploy database

Run commands:

```
# Create persistent volume
kubectl apply -f pv.yaml 

# Create persistent volume claim
kubectl apply -f pvc.yaml

# Deploy database
kubectl apply -f postgresql-deployment.yaml

# Create database service
kubectl apply -f postgresql-service.yaml
```

### Connecting via Port Forwarding

- Set up port-forwarding to `postgresql-service`

```
kubectl port-forward service/postgresql-service 5433:5432 &
```

### Mock data

Run commands:

```
export DB_PASSWORD=mypassword
PGPASSWORD="$DB_PASSWORD" psql --host 127.0.0.1 -U postgre -d mydatabase -p 5433 < <FILE_NAME.sql>
```

### Test connection

Run commands:

```
PGPASSWORD="$DB_PASSWORD" psql --host 127.0.0.1 -U postgre -d mydatabase -p 5433

# From it, you may run queries like to ensure they are not empty.
select *from users;

# To exit from the terminal, run the following command:
\q
```

## Application Deployment

- Configuration files for database is at `./deployments/application` foler.

### Deploy application

Run commands:

```
# Create configmap
kubectl apply -f configmap.yaml 

# Create secret
kubectl apply -f secret.yaml

# Deploy application
kubectl apply -f application-deployment.yaml

# Create application service
kubectl apply -f application-service.yaml
```

Note: Update DB_HOST to IP of Load Balancer when creating Database Service.

### Test application

```
curl <SERVICE_LB_ENDPOINT>/api/reports/daily_usage
```

