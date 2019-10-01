# Static Jekyll Website on AWS with CI/CD pipeline

## Jekyll Website

```
          (Resolve Domain name)
        |-->[Amazon Route53]
        |                               (Get contents)
        |                     |-->[Amazon S3 contents Bucket]
Users---|-->[AWS CloudFront]--|
        |                     |-->[Amazon S3 Logging Bucket]
        |                               (Save Logs)
        |-->[AWS Certificate Manager]
```

## AWS CI/CD Pipeline
