# VAuthenticator Helm Chart

It is the official helm chart for VAuthenticator ecosystem.

# Components

It is possible to enable the installation via helm of a namespace scoped redis.
In order to enable it have a look to the yaml snippet below and refers to the
official [bitnami details](https://github.com/bitnami/charts/tree/main/bitnami/redis/)

```yaml
in-namespace:
  redis:
    enabled: true

```

VAuthenticator is designed with AWS and kubernetes in mind. In order to configure AWS credentials have a look below:

| Name                               | Description                                                                                                                                                                | Value                                      |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| aws.region                         | AWS Region like eu-central-1                                                                                                                                               | xxxxxxxxx                                  |
| aws.iamUser.accessKey              | AWS IAM User Access Key Id                                                                                                                                                 | xxxxxxxxx                                  |
| aws.iamUser.secretKey              | AWS IAM User Access Key secret                                                                                                                                             | xxxxxxxxx                                  |
| aws.iamUser.enabled                | Identify that we want use a dedicated AWS IAM user. This strategy is required if VAuthenticator is deployed on premise                                                     | false                                      |
| aws.eks.serviceAccount.enabled     | Identify that we want use a dedicated Service Account linked to an IAM identity Provider. <br/>This strategy is the best practice if VAuthenticator is deployed on AWS EKS | false                                      |
| aws.eks.serviceAccount.iamRole.arn | It is the arn of the role used from STS to gain permissions                                                                                                                | arn:aws:iam::ACCOUNT_ID:role/IAM_ROLE_NAME |

```yaml
aws:
  region: xxxxxxxxx
  iamUser:
    accessKey: xxxxxxxxx
    secretKey: xxxxxxxxx
    enabled: false
  eks:
    serviceAccount:
      enabled: false
      iamRole:
        arn: arn:aws:iam::ACCOUNT_ID:role/IAM_ROLE_NAME

```

## VAuthenticator Authorization Server

## VAuthenticator Authorization Server Management UI