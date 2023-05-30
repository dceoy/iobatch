aws-cfn-batch-for-slc
=====================

AWS CloudFormation stacks of Batch for serverless HPC

[![Lint](https://github.com/dceoy/aws-cfn-batch-for-slc/actions/workflows/lint.yml/badge.svg)](https://github.com/dceoy/aws-cfn-batch-for-slc/actions/workflows/lint.yml)

Installation
------------

1.  Check out the repository.

    ```sh
    $ git clone --recurse-submodules git@github.com:dceoy/aws-cfn-batch-for-slc.git
    $ cd aws-cfn-batch-for-slc
    ```

2.  Install [Rain](https://github.com/aws-cloudformation/rain) and set `~/.aws/config` and `~/.aws/credentials`.

3.  Set a Slack client on AWS Chatbot and get the Slack workspace ID. (optional)

4.  Deploy stacks for IAM user groups. (optional)

    ```sh
    $ rain deploy \
        --params ProjectName=hpc-dev \
        iam-user-groups-for-devops.cfn.yml \
        hpc-dev-iam-user-groups-for-devops
    ```

5.  Deploy stacks for S3 buckets.

    ```sh
    $ rain deploy \
        --params ProjectName=hpc-dev \
        aws-cfn-s3-for-io/s3-buckets-for-io.cfn.yml \
        hpc-dev-s3-buckets-for-io
    ```

6.  Deploy stacks for IAM roles.

    ```sh
    $ rain deploy \
        --params ProjectName=hpc-dev,S3StackName=hpc-dev-s3-buckets-for-io \
        iam-roles-for-batch-services.cfn.yml \
        hpc-dev-iam-roles-for-batch-services
    ```

7.  Deploy stacks for VPC private subnets and VPC endpoints.

    ```sh
    $ rain deploy \
        --params ProjectName=hpc-dev \
        aws-cfn-vpc-for-slc/vpc-private-subnets-with-gateway-endpoints.cfn.yml \
        hpc-dev-vpc-private-subnets-with-gateway-endpoints
    ```

8.  Deploy stacks for Batch.

    ```sh
    $ rain deploy \
        --params ProjectName=hpc-dev,IamStackName=hpc-dev-iam-roles-for-batch-services,VpcStackName=hpc-dev-vpc-private-subnets-with-gateway-endpoints \
        batch-for-hpc.cfn.yml hpc-dev-batch-for-hpc
    ```

9.  Deploy stacks for VPC public subnets and NAT gateways for internet access. (optional)

    ```sh
    $ rain deploy \
        --params ProjectName=hpc-dev,VpcStackName=hpc-dev-vpc-private-subnets-with-gateway-endpoints \
        aws-cfn-vpc-for-slc/vpc-public-subnets-with-nat-gateway-per-az.cfn.yml \
        hpc-dev-vpc-public-subnets-with-nat-gateway-per-az
    ```

10. Deploy a Chatbot for AWS Step Functions. (optional)

    ```sh
    $ rain deploy \
        chatbot-and-sns-for-stepfunctions.cfn.yml \
        hpc-dev-chatbot-and-sns-for-stepfunctions
    ```
