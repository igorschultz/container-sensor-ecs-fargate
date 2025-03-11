# Deploy with a patching utility

The Falcon patching utility runs offline and takes task definition JSON as an input to generate a new task definition JSON file. The new task definition does 2 things:

* Injects `crowdstrike-falcon-init-container` into each task container.

* Makes the Falcon Container sensor the container `EntryPoint`.

## Prerequisites

> [!IMPORTANT]
>
> * CrowdStrike API Key Pair created with `Falcon Images Download (read)` and `Sensor Download (read)` scopes assigned.
>
>    > If you need help creating a new API key pair, review our docs: [CrowdStrike Falcon](https://falcon.crowdstrike.com/support/api-clients-and-keys).
>
> * `curl` installed.
> * `jq` installed.
> * `docker` installed.
> * ECS Cluster name.
> * ECR repository to store Falcon Container Sensor image (optional).

## Installation workflow

### manual-patch-utility-task-definition

1. The script will list all ACTIVE task definition from the region you defined;
2. Type the number representing the task definition you want to patch;
3. You will be asked if you have an existing ECR repository to store falcon container sensor image or if it should create one for you;
4. It will pull the latest image from CrowdStrike's registry and push it to your existing/new ECR repository;
5. Then it will patch your task definition with falcon container sensor information and register the new task definition in your AWS account.
6. The script will also create a local folder containing the original task definition JSON file and the new patched one.

### manual-patch-utility-service

1. The script will list all services running on your ECS cluster;
2. Type the name of the service you want to patch;
3. You will be asked if you have an existing ECR repository to store falcon container sensor image or if it should create one for you;
4. It will pull the latest image from CrowdStrike's registry and push to your existing/new ECR repository;
5. It collects the associated task definition to the chosen service, patch it with falcon container sensor and register the new task definition in your AWS account.
6. The script will also create a local folder containing the original task definition JSON file and the new patched one.

### automated-patch-utility-cluster

1. The script will list and patch all services running on your ECS cluster;
2. You will be asked if you have an existing ECR repository to store falcon container sensor image or if it should create one for you;
3. It will pull the latest image from CrowdStrike's registry and push to your existing/new ECR repository;
4. It collects the associated task definition from each service, patch it with falcon container sensor and register all new tasks definition in your AWS account.
5. The script will also create a local folder containing the original task definition JSON file and the new patched one.

## Usage

### manual-patch-utility-task-definition

```

./manual-patch-utility-task-definition.sh -u <client-id> -s <client-secret> -r <aws-region>

Required Flags:
    -u, --client-id <FALCON_CLIENT_ID>             Falcon API OAUTH Client ID
    -s, --client-secret <FALCON_CLIENT_SECRET>     Falcon API OAUTH Client Secret
    -r, --region <AWS_REGION>                      Falcon Cloud Region [us-east-1, us-west-2, sa-east-1]
```

### manual-patch-utility-service

```

./manual-patch-utility-service.sh -u <client-id> -s <client-secret> -r <aws-region> -c <ecs-cluster-name>

Required Flags:
    -u, --client-id <FALCON_CLIENT_ID>             Falcon API OAUTH Client ID
    -s, --client-secret <FALCON_CLIENT_SECRET>     Falcon API OAUTH Client Secret
    -r, --region <AWS_REGION>                      AWS Cloud Region [us-east-1, us-west-2, sa-east-1]
    -c, --cluster <CLUSTER_NAME>                   ECS Cluster name
```

### automated-patch-utility-cluster

```

./automated-patch-utility-cluster.sh -u <client-id> -s <client-secret> -r <aws-region> -c <ecs-cluster-name>

Required Flags:
    -u, --client-id <FALCON_CLIENT_ID>             Falcon API OAUTH Client ID
    -s, --client-secret <FALCON_CLIENT_SECRET>     Falcon API OAUTH Client Secret
    -r, --region <AWS_REGION>                      AWS Cloud Region [us-east-1, us-west-2, sa-east-1]
    -c, --cluster <CLUSTER_NAME>                   ECS Cluster name
```
