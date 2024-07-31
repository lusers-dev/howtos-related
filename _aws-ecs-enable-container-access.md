
# Linux Users - Enabling Command Execution on ECS Services

This guide outlines the steps needed to enable command execution on Amazon ECS services, specifically for a service called FOOBARNGINX within the FOOBARNGINXECSCluster cluster. Additionally, it clarifies the requirement for the container to be in a healthy state to establish an SSM connection successfully.

## Prerequisites

- AWS CLI installed and configured
- AWS account with the necessary permissions to update ECS services and execute commands

## How to Enable Command Execution

1. **Update the ECS Service to Enable Command Execution**

   To enable command execution on the ECS service, you need to use the `aws ecs update-service` command with the `--enable-execute-command` flag. This command must be executed in the region where your ECS cluster is deployed.

   Here's the command to run:

   ```sh
   aws ecs update-service --cluster FOOBARNGINXECSCluster \
   --service FOOBARNGINX --region $AWS_REGION  \
   --enable-execute-command --force-new-deployment
   ```

   - Replace `$AWS_REGION` with your AWS region (e.g., `us-east-1`).
   - Ensure `FOOBARNGINXECSCluster` and `FOOBARNGINX` match the names of your ECS cluster and service, respectively.

2. **Check the ECS Service Status**

   Before proceeding, ensure your ECS service is fully updated and the new deployment is in a healthy state. You can check this through the AWS Management Console under ECS services, or by using the AWS CLI to describe your service status.

## Connecting to the Container with SSM

After ensuring the command execution is enabled and the service is updated, you can connect to your container instance using SSM. Note that your container needs to be in a healthy state for the SSM connection to work.

1. **Execute Command via SSM**

   Use the following AWS CLI command to connect interactively to your container:

   ```sh
   aws ecs execute-command --region $AWS_REGION --cluster FOOBARNGINXECSCluster \
   --task c75a93bef87b407082289343474ee9de \
   --container FOOBARNGINX \
   --command "/bin/bash" --interactive
   ```

   - Replace `$AWS_REGION` with your AWS region.
   - Ensure the task ID `c75a93bef87b407082289343474ee9de` matches your target task's ID.
   - Replace `FOOBARNGINX` with your container name if different.



