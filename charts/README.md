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
#### Properties description

| Name                               | Description                                                                                                                                                                | Value                                      |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| in-namespace.redis.enabled         | AWS Region like eu-central-1                                                                                                                                               | true                                       |

VAuthenticator is designed with AWS and kubernetes in mind. In order to configure AWS credentials have a look below:

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

#### Properties description

| Name                               | Description                                                                                                                                                                | Value                                      |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| aws.region                         | AWS Region like eu-central-1                                                                                                                                               | xxxxxxxxx                                  |
| aws.iamUser.accessKey              | AWS IAM User Access Key Id                                                                                                                                                 | xxxxxxxxx                                  |
| aws.iamUser.secretKey              | AWS IAM User Access Key secret                                                                                                                                             | xxxxxxxxx                                  |
| aws.iamUser.enabled                | Identify that we want use a dedicated AWS IAM user. This strategy is required if VAuthenticator is deployed on premise                                                     | false                                      |
| aws.eks.serviceAccount.enabled     | Identify that we want use a dedicated Service Account linked to an IAM identity Provider. <br/>This strategy is the best practice if VAuthenticator is deployed on AWS EKS | false                                      |
| aws.eks.serviceAccount.iamRole.arn | It is the arn of the role used from STS to gain permissions                                                                                                                | arn:aws:iam::ACCOUNT_ID:role/IAM_ROLE_NAME |


## VAuthenticator Authorization Server

Here there are all the properties to program ingress, services deployment details together with application configurations 
```yaml


application:
  keda:
    enabled: false
    spec:
      minReplicaCount: 1
      maxReplicaCount: 1
      pollingInterval: 1
      cooldownPeriod: 300
    prometheus:
      serverAddress: ""
      metricName: ""
      threshold: ""
      query: ""


  pod:
    probes:
      liveness:
        initialDelaySeconds: 10
        periodSeconds: 30
      rediness:
        initialDelaySeconds: 10
        periodSeconds: 30

  service:
    type: ClusterIP

  ingress:
    host: "*"
    annotations: { }
    tls: { }
    enabled: true
    class: nginx


  resources:
    requests:
      cpu: "256m"
      memory: "256Mi"
    limits:
      cpu: "512m"
      memory: "512Mi"
  replicaCount: 1

  image:
    repository: mrflick72/vauthenticator-k8s
    pullPolicy: Always
    tag: "latest"

  lables: { }

  selectorLabels:
    app: vauthenticator

  podAnnotations: { }

  masterKey: ACCOUNT_KMS_KEY

  redis:
    database: 0
    host: vauthenticator-redis-master.auth.svc.cluster.local

  server:
    port: 8080

  baseUrl: http://application-example-host.com
  backChannelBaseUrl: http://vauthenticator:8080


  mailProvider:
    enabled: false
    host: localhost
    port: 587
    username: ""
    password: ""
    properties: { }

  mail:
    from: ""
    welcomeMailSubject: ""
    verificationMailSubject: ""
    resetPasswordMailSubject: ""
    mfaMailSubject: ""

  dynamoDb:
    account:
      tableName: your_VAuthenticator_Account_table_name
      role:
        tableName: your_VAuthenticator_Account_Role_table_name
    role:
      tableName: your_VAuthenticator_Role_table_name
    clientApplication:
      tableName: your_VAuthenticator_ClientApplication_table_name
    mfaAccountMethods:
      tableName: your_VAuthenticator_mfaAccountMethods_table_name
    keys:
      mfa:
        tableName: your_VAuthenticator_Mfa-Keys_table_name
      signature:
        tableName: your_VAuthenticator_Signature_Keys_table_name
      tableName: your_VAuthenticator_Keys_table_name
    ticket:
      tableName: your_VAuthenticator_tICKET_table_name

  documentRepository:
    bucketName: test

  mfa:
    otp:
      otpLength: 6
      otpTimeToLiveInSeconds: 30

  assetServer:
    baseUrl: http://localhost:3000/asset

```

#### Properties description


## VAuthenticator Authorization Server Management UI