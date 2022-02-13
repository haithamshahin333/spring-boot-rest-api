# Spring Boot REST API

This repo is designed to show patterns for spring boot REST APIs as it relates to building and deploying to container-based platforms and services in Azure.

To get started with an enterprise deployment, review the following associated [Enterprise AKS Repo](https://github.com/haithamshahin333/enterprise-kubernetes-patterns)  to deploy a Private AKS Cluster and demonstrate concepts such as GitOps, ACR Build Tasks, and Self-Host GitHub Runners.

## Setup for Deployment

1. Deploy [Enterprise AKS Repo](https://github.com/haithamshahin333/enterprise-kubernetes-patterns)

    > Be sure to complete the deployment and associated steps documented in the repo above before proceeding to step 2 for app deployment

2. Update GitHub Secrets:

    1. `ACR_REGISTRY_NAME`: This is the name of the ACR resource deployed (do not include `.azurecr.io`, only the resource name itself)

    2. `AKS_CLUSTER_NAME`: Name of target AKS Cluster

    3. `RESOURCE_GROUP`: Resource Group containing AKS Cluster

    4. `AZURE_CREDENTIALS`: This is the service principal that the agent uses to authenticate to Azure. Contributor on ACR

        > You will need to create the service principal with the following command and copy and paste the full json to the secret:

        ```bash
        export SUBSCRIPTION_ID=$(az account show --query 'id')
        export RESOURCE_GROUP=app-innovation-landing-zone #update based on where deployment for ACR occurred
        export ACR_RESOURCE_ID=$(az acr list -g $RESOURCE_GROUP -o tsv --query '[0].id') #assumes only one ACR in the resource group
        export AKS_RESOURCE_ID=$(az aks list -g $RESOURCE_GROUP -o tsv --query '[0].id')
        az ad sp create-for-rbac --name "githubActionServicePrincipal" \
                                --role Contributor \
                                --scopes $ACR_RESOURCE_ID $AKS_RESOURCE_ID \
                                --sdk-auth
                                
        # The command should output a JSON object similar to this which you should copy and paste
        {
            "clientId": "<GUID>",
            "clientSecret": "<GUID>",
            "subscriptionId": "<GUID>",
            "tenantId": "<GUID>",
            (...)
        }
        ```

## Deploying and Running CI/CD Workflow

- Open [pom.xml](pom.xml) and add your forked repo url under `<distributionManagement>`.