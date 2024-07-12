aws iam attach-role-policy --role-name AmazonEKSNodeRole --policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy 

aws eks create-addon --addon-name amazon-cloudwatch-observability --cluster-name demo
