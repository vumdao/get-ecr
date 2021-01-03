<p align="center">
  <a href="https://dev.to/vumdao">
    <img alt="A Tool to get latet image version of application from ECR" src="https://dev-to-uploads.s3.amazonaws.com/i/aqbkj87wr6mbiuble3zv.png" width="500" />
  </a>
</p>
<h2 align="center">
  <div>A python script to get latet image version of application from ECR.</div>
  <dive>In some case, developers want to know the latest image version of application such master branch for deploying to staging and product. We can provide to them a tool (slackbot, ChatOps, etc.) to call the script.</div>
</h2>


<h1 align="center">
  <img src="https://dev-to-uploads.s3.amazonaws.com/i/6c8o2euizjbx8kxnyxz0.png" width=600/>
</h1>

## What’s In This Document 
- [Get application code](#-Get-application-code)
- [Run test](#-Run-test)


### 🚀 **[Get application code](#-Get-application-code)**
- Get latest image version of master branch from ECR with prefix `master-` and lastest `pushed at`

```
import boto3
import re


def get_latest_master_image():
    """ Filter images with prefix master- and return the latest pushed one """
    client = boto3.client('ecr', region_name='ap-southeast-1')
    response = client.list_images(
        registryId='111111111111',
        repositoryName='repo/application',
        maxResults=1000
    )

    latest = None
    temp_tag = None

    for image in response['imageIds']:
        tag = image['imageTag']
        if re.search("^master-[0-9]+", tag):
            img = client.describe_images(
                registryId='111111111111',
                repositoryName='repo/application',
                imageIds=[
                    {
                        'imageTag': tag
                    },
                ]
            )
            pushed_at = img['imageDetails'][0]['imagePushedAt']
            if latest is None:
                latest = pushed_at
            else:
                if latest < pushed_at:
                    latest = pushed_at
                    temp_tag = tag
    return temp_tag, latest


version, pushed_at = get_latest_master_image()
print(f'app {version} pushed at {pushed_at}')
```

### 🚀 **[Run test](#-Run-test)**
```
⚡ $ python getImageVersion.py 
app master-12163 pushed at 2020-12-31 10:10:53+07:00
```

### 🚀 **[A practice to use this script](#-A-practice-to-use-this-script)**
- Using slackbot

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/r240froj3r7xgdl86j91.png)


**Read More**
- [Pelican-resume with docker-compose and AWS + CDK](https://dev.to/vumdao/pelican-resume-with-docker-compose-and-aws-cdk-33e5)
- [Using Helm Install Botkube Integrate With Slack On EKS](https://dev.to/vumdao/using-helm-install-botkube-integrate-with-slack-on-eks-gmn)
- [Ansible AWS EC2 Dynamic Inventory Plugin](https://dev.to/vumdao/ansible-aws-ec2-dynamic-inventory-plugin-3bme)
- [How To List All Enabled Regions Within An AWS account](https://dev.to/vumdao/list-all-enabled-regions-within-an-aws-account-4oo7)
- [Using AWS KMS In AWS Lambda](https://dev.to/vumdao/using-aws-kms-in-aws-lambda-2jm2)
- [Create AWS Backup Plan](https://dev.to/vumdao/create-aws-backup-plan-a0f)
- [Techniques For Writing Least Privilege IAM Policies](https://dev.to/vumdao/techniques-for-writing-least-privilege-iam-policies-4fc7)
- [EKS Persistent Storage With EFS Amazon Service](https://dev.to/vumdao/eks-persistent-storage-with-efs-amazon-service-14ei)
- [Create k8s Cronjob To Schedule Delete Expired Files](https://dev.to/vumdao/create-k8s-cronjob-to-schedule-delete-expired-files-1i41)
- [Amazon ECR - Lifecycle Policy Rules](https://dev.to/vumdao/amazon-ecr-lifecycle-policy-rules-1l59)
- [Connect Postgres Database Using Lambda Function](https://dev.to/vumdao/connect-postgres-database-using-lambda-function-1mca)
- [Using SourceIp in ALB Listener Rule](https://dev.to/vumdao/using-sourceip-in-alb-listener-rule-377b)
- [Amazon Simple Systems Manager (SSM)](https://dev.to/vumdao/amazon-simple-systems-manager-ssm-2pb0)
- [Invalidation AWS CDN Using Boto3](https://dev.to/vumdao/invalidation-aws-cdn-using-boto3-2k9g)
- [Create AWS Lambda Function Triggered By S3 Notification Event](https://dev.to/vumdao/create-aws-lambda-function-triggered-by-s3-notification-event-9p0)
- [CI/CD Of Invalidation AWS CDN Using Gitlab Pipeline](https://dev.to/vumdao/ci-cd-of-invalidation-aws-cdn-using-gitlab-pipeline-34op)
- [Create CodeDeploy](https://dev.to/vumdao/create-codedeploy-4425)
- [Gitlab Pipeline With AWS Codedeploy](https://dev.to/vumdao/gitlab-pipeline-with-aws-codedeploy-30cl)
- [Create AWS-CDK image container](https://dev.to/vumdao/create-aws-cdk-image-container-43ei)
- [Deploy Python Lambda Functions With Container Image](https://dev.to/vumdao/deploy-python-lambda-functions-with-container-image-5hgj)
- [Custom CloudWatch Events](https://dev.to/vumdao/custom-cloudwatch-events-4j3j)

<h3 align="center">
  <a href="https://dev.to/vumdao">:stars: Blog</a>
  <span> · </span>
  <a href="https://vumdao.hashnode.dev/">Web</a>
  <span> · </span>
  <a href="https://www.linkedin.com/in/vu-dao-9280ab43/">Linkedin</a>
  <span> · </span>
  <a href="https://www.linkedin.com/groups/12488649/">Group</a>
  <span> · </span>
  <a href="https://www.facebook.com/CloudOpz-104917804863956">Page</a>
  <span> · </span>
  <a href="https://twitter.com/VuDao81124667">Twitter :stars:</a>
</h3>

