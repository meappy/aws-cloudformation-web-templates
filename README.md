# AWS CloudFormation Sample Web Templates

### Prerequisites

#### AWS Command Line Interface
This guide relies on AWS Command Line Interface, please refer to [AWS Command Line Interface](https://aws.amazon.com/cli/) to set up CLI in your local environment.

### Deploying full stack with AWS CloudFormation
Change directory working directory to [cloudformation](./) folder

#### CloudFormation sequqnce
It is crucial to wait until a stack completes before proceeding to the next step

1. Create VPC
```
aws --profile aws-profile cloudformation create-stack --stack-name vpc-v1 \
    --parameters file://params/01-vpc-params.json --template-body file://01-vpc.yaml
```

2. Create EFS, RDS, Elasticsearch, ElastiCache
```
aws --profile aws-profile cloudformation create-stack --stack-name efs-v1 \
    --parameters file://params/02-efs-params.json --template-body file://02-efs.yaml 
```

```
aws --profile aws-profile cloudformation create-stack --stack-name rds-v1 \
    --parameters file://params/03-rds-params.json --template-body file://03-rds.yaml 
```

```
aws --profile aws-profile cloudformation create-stack --stack-name ec2-elk-v1 \
    --parameters file://params/05-ec2-elk-params.json --template-body file://05-ec2-elk.yaml 
```

```
aws --profile aws-profile cloudformation create-stack --stack-name elasticache-v1 \
   --parameters file://params/06-elasticache-params.json --template-body file://06-elasticache.yaml
```

3. Create EC2 Bastion 
```
aws --profile aws-profile cloudformation create-stack --stack-name ec2-bastion-v1 \
    --parameters file://params/04-ec2-bastion-params.json --template-body file://04-ec2-bastion.yaml
```

4. Create EC2 Web
```
aws --profile aws-profile cloudformation create-stack --stack-name ec2-web-v1 \
    --parameters file://params/07-ec2-web-params.json --template-body file://07-ec2-web.yaml 
```

### If using CloudFormation, validation is via email only https://bit.ly/2RlfxtB
```
aws --profile aws-profile cloudformation create-stack --stack-name certificates-v1 \
    --template-body file://08-certificatemanager.yaml
```

### Sync to s3
```
aws --profile aws-profile s3 sync brandsite-v1 s3://deploy-v1/brandsite \
    --delete --exclude={*.git*,README*}
```

### Deleting a stack
```
aws --profile aws-profile cloudformation delete-stack --stack-name ec2-web-v1

```

### Watching changes/progress of stack https://bit.ly/2Rrrj64
```
watch -n1 'aws-cloudformation-stack-status --profile aws-profile --stack-name ec2-web-v1'
```
