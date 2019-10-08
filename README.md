# Static Jekyll Website on AWS with CI/CD pipeline

## Jekyll Website

```
                    [CloudFormation Template]
|---------------------------------------------------------------------|
|           (Resolve Domain name)                                     |
|         |-->[Amazon Route53]                                        |
|         |                               (Get contents)              |
|         |                     |-->[Amazon S3 contents Bucket]       |
| Users---|-->[AWS CloudFront]--|                                     |
|         |                     |-->[Amazon S3 Logging Bucket]        |
|         |                               (Save Logs)                 |
|         |-->[AWS Certificate Manager]                               |
|---------------------------------------------------------------------|
```

## AWS CI/CD Pipeline

```
                    [CloudFormation Template]
|---------------------------------------------------------------------------|
|[NewCodeChange] --> [Pushed to master branch] --> [AWS CodePipeline]       |     
|                                                            |              |
|                                                            |              |
|          Published to S3 <-- Build Jekyll Website <-- [AWS CodeBuild]     |
|                    |                                                      |
|                    |                                                      |
|     Lambda function triggered to invalidate CloudFront Distribution       |
|  (Old files removed from CloudFront edge before the actual expiration)    |
|---------------------------------------------------------------------------|
```
