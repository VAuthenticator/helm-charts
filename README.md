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

| Name         | Description   | Value       |
|--------------|---------------|-------------|
| aws.region   | AWS region    | xxxxxxxxx   |
| ------------ | ------------- | ----------- |

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