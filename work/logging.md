# CloudWatch Container Insights

Container Insights can be installed to your CloudWatch to give you periodic application logging that will be useful to periodically check your application's health.

## Workflow

All you need to do is run a couple of short commands on your terminal:

- Step 1. Attach the CloudWatchAgentServerPolicy IAM policy to your worker nodes:

```
aws iam attach-role-policy --role-name EKSNodeGroupRole --policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy
```

Replace `EKSNodeGroupRole` with your EKS cluster's Node Group's IAM role.

- Step 2. Use AWS CLI to install the Amazon CloudWatch Observability EKS add-on:

```
aws eks create-addon --addon-name amazon-cloudwatch-observability --cluster-name my-cluster-name
```

Replace `my-cluster-name` with your EKS cluster's name.

- Step 3. Trigger logging by accessing your application.

- Step 4. Open up CloudWatch Log groups page. You should see `aws/containerinsights/my-cluster-name/application` there.

## Sample

```
aws iam attach-role-policy --role-name AmazonEKSNodeRole --policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy 
aws eks create-addon --addon-name amazon-cloudwatch-observability --cluster-name demo
```
