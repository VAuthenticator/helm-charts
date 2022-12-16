# VAuthenticator Helm Chart

It is the official helm chart for VAuthenticator ecosystem.

# Global Components

We discuss how configure global and general available properties

## Redis

It is possible to enable the installation via helm of a namespace scoped redis.
In order to enable it have a look to the yaml snippet below and refers to the
official [bitnami details](https://github.com/bitnami/charts/tree/main/bitnami/redis/)

#### yaml section

```yaml
in-namespace:
  redis:
    enabled: true

```

#### Properties description

| Name                       | Description                  | Value |
|----------------------------|------------------------------|-------|
| in-namespace.redis.enabled | AWS Region like eu-central-1 | true  |

## AWS

VAuthenticator is designed with AWS and kubernetes in mind. In order to configure AWS credentials have a look below:

#### yaml section

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

## KEDA

In order to apply autoscaling in VAuthenticator authorization server is possible configure [keda](https://keda.sh/) with
prometheus metrics.

KEDA is applied on `application` and `managementUi` root field for simplicity we show as root field application but
the properties are valid for `managementUi` too

#### yaml section

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
```

#### Properties description

| Name                                      | Description                                                                                                           | Value |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------|-------|
| application.keda.enabled                  | define if the keda autoscaling is enabled                                                                             | false |
| application.keda.spec.minReplicaCount     | minReplicaCount autoscaling parameter                                                                                 | 1     |
| application.keda.spec.maxReplicaCount     | maxReplicaCount autoscaling parameter                                                                                 | 1     |
| application.keda.spec.pollingInterval     | polling Interval of metric evaluation in order to decide if apply autoscaling or not                                  | 1     |
| application.keda.spec.cooldownPeriod      | specifies how long any alarm-triggered scaling action will be disallowed after a previous scaling action is complete. | 300   |
| application.keda.prometheus.serverAddress | prometheus address                                                                                                    | ""    |
| application.keda.prometheus.metricName    | prometheus metric name                                                                                                | ""    |
| application.keda.prometheus.threshold     | prometheus metric threshold to apply for autoscaling                                                                  | ""    |
| application.keda.prometheus.query         | prometheus query to evaluate autoscaling                                                                              | ""    |

## POD

Pod section is applied on `application`, `applicationAssets`, `managementUi` and `managementUiAssets` root field for
simplicity we show as root field application but the properties are valid for `applicationAssets`, `managementUi`
and `managementUiAssets` too

#### yaml section

```yaml

pod:
  probes:
    liveness:
      initialDelaySeconds: 10
      periodSeconds: 30
    rediness:
      initialDelaySeconds: 10
      periodSeconds: 30
```

#### Properties description

| Name                                    | Description                                                                                    | Value |
|-----------------------------------------|------------------------------------------------------------------------------------------------|-------|
| pod.probes.liveness.initialDelaySeconds | define for liveness probe the initial delay in seconds to wait to start to evaluate the probes | 10    |
| pod.probes.liveness.periodSeconds       | define for liveness probe period in seconds to wait for the next evaluation                    | 30    |
| pod.probes.rediness.initialDelaySeconds | define for rediness probe the initial delay in seconds to wait to start to evaluate the probes | 10    |
| pod.probes.rediness.periodSeconds       | define for rediness probe period in seconds to wait for the next evaluation                    | 30    |

## Ingress

Ingress section is applied on `application`, `applicationAssets`, `managementUi` and `managementUiAssets` root field for
simplicity we show as root field application but the properties are valid for `applicationAssets`, `managementUi`
and `managementUiAssets` too

#### yaml section

```yaml
ingress:
  host: "*"
  annotations: { }
  tls: { }
  enabled: true
  class: nginx
```

#### Properties description

| Name                | Description                                                                     | Value |
|---------------------|---------------------------------------------------------------------------------|-------|
| ingress.host        | define on what host the ingress resource should be configured to answer         | *     |
| ingress.annotations | define annotations for the ingress resource                                     | { }   |
| ingress.tls         | define tls configuration for the ingress resource                               | { }   |
| ingress.enabled     | define if the ingress should be included in the resources                       | true  |
| ingress.class       | define what kind of ingress class should be configured for teh ingress resource | nginx |

#### yaml section

```yaml

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

# VAuthenticator Authorization Server

# VAuthenticator Authorization Server Management UI