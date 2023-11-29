Description
This CloudFormation template automates the deployment of an Elastic Beanstalk application within an AWS environment. The architecture includes an Elastic Load Balancer (ELB) and an Auto Scaling Group (ASG) with scaling policies based on CPU utilization to ensure optimal performance under varying workloads.

Parameters
AppEnvironment: (Default: pilotenv)
The environment for the Elastic Beanstalk application.

EC2InstanceType: (Default: t2.medium)
The instance type for the EC2 instances within the Auto Scaling Group.

AccountVpc: (Default: vpc-004639285051f3cd7)
The Virtual Private Cloud (VPC) where the resources will be created.

EC2Subnet: (Default: subnet-0bd5312909cceefb9)
The subnet where the EC2 instances will be launched.

LoadBalancerSubnet: (Default: subnet-0a2ea4589dffaaa98)
The subnet where the Elastic Load Balancer (ELB) will be deployed.

LoadBalancerVisibility: (Default: public)
The visibility of the Elastic Load Balancer (public or internal).

Resources
CloudWatchLogsRole:
IAM role for CloudWatch Logs with specific permissions for various AWS services.

InstanceProfile:
IAM instance profile associated with the CloudWatchLogsRole.

LogGroup:
AWS CloudWatch Logs log group for storing Elastic Beanstalk application logs.

App:
Elastic Beanstalk application definition with the name "nodejs-app" and description.

AppVersion:
Elastic Beanstalk application version pointing to a specified S3 source bundle (nodejs.zip).

AppEnvironment1:
Elastic Beanstalk environment configuration with various settings:

EnvironmentName: (Generated as a combination of App and AppEnvironment parameters)
ApplicationName: (Reference to the Elastic Beanstalk application)
SolutionStackName: "64bit Amazon Linux 2023 v6.0.3 running Node.js 18"
Tier: WebServer - Standard (Version 1.0)
Auto Scaling Configuration:

MinSize: 2 (Minimum number of instances in the Auto Scaling Group)
MaxSize: 4 (Maximum number of instances in the Auto Scaling Group)
UpperThreshold: 80 (Upper CPU utilization threshold triggering scaling up)
BreachDuration: 5 (Duration of breach for scaling actions)
UpperBreachScaleIncrement: 1 (Number of instances to add when scaling up)
LowerThreshold: 30 (Lower CPU utilization threshold triggering scaling down)
LowerBreachScaleIncrement: -1 (Number of instances to remove when scaling down)
MeasureName: "CPUUtilization" (Metric used for scaling decisions)
Unit: "Percent" (Unit of the metric)
