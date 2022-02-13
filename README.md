# Spring Boot REST API

This repo is designed to show patterns for spring boot REST APIs as it relates to building and deploying to container-based platforms and services in Azure.

To get started with an enterprise deployment, review the following associated [Enterprise AKS Repo](https://github.com/haithamshahin333/enterprise-kubernetes-patterns)  to deploy a Private AKS Cluster and demonstrate concepts such as GitOps, ACR Build Tasks, and Self-Host GitHub Runners.

## Setup for Deployment

1. Deploy [Enterprise AKS Repo](https://github.com/haithamshahin333/enterprise-kubernetes-patterns)

2. Update GitHub Secrets:

## Deploying and Running CI/CD Workflow

- Open [pom.xml](pom.xml) and add your forked repo url under `<distributionManagement>`.