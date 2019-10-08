# Static Jekyll Website on AWS with CI/CD pipeline


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
