# Falcon Container Sensor Deployment Scripts

Bash script to inject the latest version of Falcon Container Sensor (sidecar) into your ECS Fargate Task Definitions, pulling the ECS Services or Task Definition from your AWS environment and patching them with our Patching Utility or Falcon Utility.

## Purpose:

To facilitate quick deployment of recommended CWP resources for testing.

For other deployment methods including, advanced customization and highly automated deployments, please use our official documentation to help meeting your requirements and needs.

## Prerequisites

> [!IMPORTANT]
> - CrowdStrike API Key Pair created with `Falcon Images Download (read)` and `Sensor Download (read)` scopes assigned.
>
>    > If you need help creating a new API key pair, review our docs: [CrowdStrike Falcon](https://falcon.crowdstrike.com/support/api-clients-and-keys).
>
> - `curl` installed.
> - `jq` installed.
> - `docker` installed.
> - ECS Cluster name.
> - ECR repository to store Falcon Container Sensor image (optional).

## AWS Required Permissions

> [!IMPORTANT]
> - ECS (list):
>    > ListServices
>    > ListTaskDefinitionFamilies
> - ECS (read):
>    > DescribeClusters
>    > DescribeServices
>    > DescribeTaskDefinition
> - ECS (write):
>    > RegisterTaskDefinition
> - ECR (read):
>    > DescribeRepositories
>    > GetAuthorizationToken
>    > BatchGetImage
> - ECR (write):
>    > CreateRepository

## Deployment Options:

To inject Falcon Container Sensor into your container application, you can choose between the following methods:

* [Patching Utility](patching-utility/README.md): Injecting our sidecar by appending or patching our specific parameters to your existing AWS task definition.

* [Falcon Utility](falcon-utility/README.md): Injecting our sidecar by appending or patching our specific parameters directly to your application container image.
