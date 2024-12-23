# Application Deployment on EKS using Fargate and AWS ALB

This repository showcases a project that demonstrates deploying an application on Amazon Elastic Kubernetes Service (EKS) using Fargate profiles and the AWS Application Load Balancer (ALB). The project includes creating an EKS cluster, deploying application resources, and exposing the application using an ALB.

Project Overview

Key Features:

Serverless Kubernetes: Utilized Amazon EKS with Fargate profiles for serverless container orchestration.

Secure Access: Configured IAM roles and OpenID Connect (OIDC) for secure Kubernetes workload permissions.

Containerized Deployment: Deployed application components using Kubernetes manifests.

Traffic Management: Configured AWS ALB Ingress Controller for advanced traffic routing.

High Availability: Ensured the application is accessible via the ALB DNS name.

Architecture

The project consists of the following components:

EKS Cluster: Created using AWS Management Console/CLI with Fargate profiles for managing compute resources.

Container Image: Pulled from AWS Elastic Container Registry (ECR).

Kubernetes Manifests:

deployment.yaml: Defines the application’s deployment configuration.

service.yaml: Exposes the application internally within the cluster.

ingress.yaml: Manages external access to the application via ALB.

AWS ALB Ingress Controller: Installed using Helm for managing ingress traffic.

Deployment Steps

Prerequisites

AWS CLI installed and configured with sufficient permissions.

kubectl installed.

Helm installed.

AWS IAM OIDC provider set up for the EKS cluster.

Steps:

Create EKS Cluster with Fargate:

Use AWS Management Console or AWS CLI to create an EKS cluster and configure a Fargate profile.

Push Docker Image to ECR:

Build and tag your application image.

Push the image to an AWS ECR repository.

Deploy Kubernetes Resources:

Apply the following manifests:

kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml

Configure IAM and OIDC:

Associate the EKS cluster with an IAM OIDC provider.

Create an IAM policy and role with permissions for ALB ingress.

Annotate the Kubernetes service account with the IAM role.

Install AWS ALB Ingress Controller:

Add the Helm repository:

helm repo add eks https://aws.github.io/eks-charts
helm repo update

Install the ALB Ingress Controller:

helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  --set clusterName=<your-cluster-name> \
  --set serviceAccount.create=false \
  --set serviceAccount.name=alb-ingress-controller \
  -n kube-system

Access the Application:

Retrieve the DNS name of the ALB:

kubectl get ingress

Access the application in a browser using the ALB DNS name.

Files in Repository

deployment.yaml: Defines the application deployment.

service.yaml: Kubernetes Service for internal communication.

ingress.yaml: Configures external access via ALB.

IAM configuration script: Instructions or CLI commands for setting up IAM roles and OIDC.

Helm commands: Used for installing the ALB Ingress Controller.
