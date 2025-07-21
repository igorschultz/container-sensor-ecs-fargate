# Deploy with a falcon utility

Deploying the Falcon Container sensor for Linux to ECS Fargate requires modification of the application container image. The Falcon Container sensor image contains a Falcon utility that supports patching the application container image with Falcon Container sensor for Linux and its related dependencies.

The Falcon Container has 2 components:

* Falcon Container sensor for Linux: At runtime, the Falcon Container sensor for Linux uses unique technology to launch and run inside the application container of the service or task.
* Falcon utility: The Falcon utility runs offline and takes the application container image as an input to generate a new container image patched with the Falcon Container sensor for Linux and its related dependencies. The Falcon utility also sets the Falcon entry point as the container entry point.

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
5. The script will collect the container image from the chosen task definition and patch it with Falcon Utility;
6. The new patched image will be pushed to the app container's repository;
7. A new task definition will be created and registered on your AWS account using the new patched image;
8. The script will also create a local folder containing the original task definition JSON file and the new patched one.

### manual-falcon-utility-service

1. The script will list all services running on your ECS cluster;
2. Type the name of the service you want to patch;
3. You will be asked if you have an existing ECR repository to store falcon container sensor image or if it should create one for you;
4. It will pull the latest image from CrowdStrike's registry and push to your existing/new ECR repository;
5. It collects and pulls the associated container image from the chosen service;
6. Patches it with falcon container sensor and push the new image to application's container ECR repository;
7. Creates a new task definition revision containing the new patched image;
8. The script will also create a local folder containing the original task definition JSON file and the new patched one.

## Usage

### manual-patch-utility-task-definition

```

./manual-falcon-utility-task-definition.sh -u <client-id> -s <client-secret> -r <aws-region> -p <platform>

Required Flags:
    -u, --client-id <FALCON_CLIENT_ID>             Falcon API OAUTH Client ID
    -s, --client-secret <FALCON_CLIENT_SECRET>     Falcon API OAUTH Client Secret
    -r, --region <AWS_REGION>                      Falcon Cloud Region [us-east-1, us-west-2, sa-east-1]
    -p, --platform <SENSOR_PLATFORM>               Specify sensor platform to retrieve, e.g., x86_64, aarch64
```

### manual-falcon-utility-service

```

./manual-falcon-utility-service.sh -u <client-id> -s <client-secret> -r <aws-region> -c <ecs-cluster-name> -p <platform>

Required Flags:
    -u, --client-id <FALCON_CLIENT_ID>             Falcon API OAUTH Client ID
    -s, --client-secret <FALCON_CLIENT_SECRET>     Falcon API OAUTH Client Secret
    -r, --region <AWS_REGION>                      AWS Cloud Region [us-east-1, us-west-2, sa-east-1]
    -c, --cluster <CLUSTER_NAME>                   ECS Cluster name
    -p, --platform <SENSOR_PLATFORM>               Specify sensor platform to retrieve, e.g., x86_64, aarch64
```
