# Build and Deploy Containers to ECR

AWS ECR is a managed service used to handle the lifecycle of Docker images. It is currently one of the more popular options for managing private container registries for internal use.

## Working with ECR

### Creating a Registry

- Run the following to authenticate against ECR. The token is valid for 12 hours. Without running this, docker pull and docker push won't work.

```sh
aws ecr get-login-password --region [region] | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.[region].amazonaws.com
```

- If you use a separate profile on your CLI configuration (through aws configure --profile [profile_name] or similar means), you may use that profile when getting the login and password of your ECR. Run the following command instead (notice the --profile addition):

```sh
aws ecr get-login-password --region region --profile [profile_name] | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
```

### Creating a Repository

- It's recommended to create one unique repository for the lifecycle of one Docker image. You should not create one Docker repository to manage Docker images from different applications. Tag immutability enabled is best practice and thus highly encouraged. KMS encryption is typically not needed unless requested by your security team.

### Pushing to a Repository

Tag your local Docker image with the appropriate ECR tag so Docker knows where it should push to.

```sh
docker tag <IMAGE_ID> aws_account_id.dkr.ecr.region.amazonaws.com/my-repository:tag
```

### Push the Docker image

```sh
docker push aws_account_id.dkr.ecr.region.amazonaws.com/my-repository:tag
```

If you ever forget the exact steps to push a Docker image, the AWS Console conveniently has the steps detailed in every repository.

### Pulling from a Repository

```sh
docker pull aws_account_id.dkr.ecr.region.amazonaws.com/my-repository:tag
```

## Working with CodeBuild

This is the process for how we can take code that we have in GitHub, connect it to use CodeBuild to build this code into a Docker image, and then use CodeBuild to push the same Docker image into ECR.

- To start off, this is the build spec YAML file that tells CodeBuild how to process and what to do in a specific build job:
<!--@include:./buildspec.yml-->

### Create CodeBuild Project

- Create a project using the Create Build Project button and go through the guided process for creating a new CodeBuild project.
- Inside Source, we need to specify where our code is going to be located so that CodeBuild knows how to connect and what to run it with. Our code is hosted in GitHub, you'll need to set up access between CodeBuild and GitHub: either using OAuth or a personal access token.

### Environment Variables and IAM Role Modifications
- Environment variables are set during project creation to replace placeholders in the build spec file. The IAM role created for CodeBuild needs adjustments to grant permissions for ECR access.

- Add permissions to update ECR to the newly created role.
Go to the IAM console > Roles, then find the role you have created when creating the CodeBuild project.

Under the Permissions tab, click on Add Permissions then choose Create inline policy. Choose JSON, then replace the policy with the following:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:*"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```