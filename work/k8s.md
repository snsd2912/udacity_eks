# Setting Up kubeconfig File in EKS
The AWS command line tool has a convenient method set up to set up access to your EKS cluster.

The following command will update your ~/.kube/config file:

aws eks update-kubeconfig --name <CLUSTER_NAME>
After this is set up, running kubectl commands will point to your Kubernetes cluster set up with EKS.

# Create storage

## Check storage class

```shell
kubectl get storageclass
```

Sample output:

```shell
NAME   PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
gp2    kubernetes.io/aws-ebs   Delete          WaitForFirstConsumer   false                  18h
```

## Create Persistent Volume



## Create Persistent Volume Claim

- A request to use Persistent Volume by user

