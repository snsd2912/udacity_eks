# Project: Operationalizing a Coworking Space Microservice

## Contents

+ [Overview](#Overview)
+ [Workflow](#Workflow)

## Overview

The Coworking Space Service is a set of APIs that enables users to request one-time tokens and administrators to authorize access to a coworking space.

This service follows a microservice pattern and the APIs are split into distinct services that can be deployed and managed independently of one another.

For this project, you are a DevOps engineer who will be collaborating with a team that is building an API for business analysts. The API provides business analysts with basic analytics data on user activity in the coworking space service. The application they provide you functions as expected, and you will help build a pipeline to deploy it to Kubernetes.

## Workflow

### [Build and Deploy Containers to ECR](work/ecr.md)

Store Docker images in ECR: 

![Example-DAG](images/ecr_image.png)

Run CodeBuild pipeline to deploy Docker image to AWS ECR:

![Example-DAG](images/ecr_build.png)

### [Kubernetes Configuration](work/k8s.md)
