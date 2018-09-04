# NVIDIA Docker AMI template

CloudFormation template to build an AMI with [NVIDIA Docker 2.0](https://github.com/NVIDIA/nvidia-docker), 
based on Ubuntu 16.04 LTS. AMI will be created using a [Spot Instance](https://aws.amazon.com/ec2/spot/) with GPU.

## Building an AMI

1. Make sure you've installed and configured the AWS CLI tool: [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html).

2. Create an EC2 Key Pair if you don't have one:
    ```bash
    aws ec2 create-key-pair --key-name my-key --query 'KeyMaterial' --output text > my-key.pem
    ```

3. Build an AMI:
    ```bash
    aws cloudformation create-stack \
        --template-body file://create_ami.yaml \
        --capabilities CAPABILITY_IAM \
        --parameters \
            ParameterKey=ImageName,ParameterValue=nvidia-docker \
            ParameterKey=InstanceType,ParameterValue=p2.xlarge \
            ParameterKey=KeyName,ParameterValue=my-key \
        --stack-name nvidia-docker-ami
        --on-failure DELETE
    ```

    If during the build something goes wrong, you can debug it:
    - first of all check CloudFormation and CloudWatch logs
    - if it didn't help, change `--on-failure` parameter to "DO_NOTHING", build an AMI again and 
    connect to the instance using your EC2 key

4. To delete the AMI, delete its stack:
    ```bash
    aws cloudformation delete-stack --stack-name nvidia-docker-ami
    ```
