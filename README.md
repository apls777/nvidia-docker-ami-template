# NVIDIA Docker AMI template

This CloudFormation template creates an AMI with [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker) 
based on Ubuntu 16.04 LTS. The image will be created using a [Spot Instance](https://aws.amazon.com/ec2/spot/).

To create an AMI simply use the following command:
```bash
aws cloudformation create-stack \
    --region <REGION> \
    --template-body file://create_ami.yaml \
    --capabilities CAPABILITY_IAM \
    --parameters \
        ParameterKey=InstanceType,ParameterValue=<INSTANCE_TYPE> \
        ParameterKey=KeyName,ParameterValue=<KEY_NAME> \
    --stack-name gpu-ami
```

To delete an AMI, delete its stack:
```bash
aws cloudformation delete-stack \
    --region <REGION> \
    --stack-name gpu-ami
```
