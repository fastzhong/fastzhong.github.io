+++
title = "Serverless 101"
date = 2018-01-30T10:18:02+08:00
readingTime = true
categories = ["云 & 云原生"]
tags = ["serverless"]
toc = true
+++

serverless what, why, how

<!--more-->

## Serverless 是什么

Serverless 就是让开发近最大可能的关注在代码上，而不是 infra，代码以 **function** 为基本单元进行部署和运行:

![more code, less infra](/images/serverless/more-code-less-infra.gif)

简单讲 Serverless 就是：

> `Serverless = FaaS + BaaS`

Serverless 和数据库的存储过程很像，把存储过程搬到了云端，由数据库触发事件变成云端触发事件，由数据库支持变成云平台支持，简洁直接，但能发挥的功效依赖所属平台的支持。

FaaS 具体和 IaaS，PaaS 的比较如下：
![FaaS](/images/serverless/fass.jpg)

## Serverless 的特点

-   编程模式（典型分布式特点）
    -   event-driven 事件响应
    -   sharing nothing
    -   stateless
-   运维模式
    -   无需开通，没有服务器，没有虚机，也没有容器，基础设施方面是完全 0 代价维护
    -   按需自动扩展，内置扩展性（云服务商支持）
    -   全球到达，内置分布式架构（云服务商支持）
-   收费模式
    -   完全按使用多少收费，闲置状态不收费

## Serverless 的挑战

-   启动延时
-   监控
-   调试和错误诊断
-   本地开发，并可靠的发布到云端
-   和已有的应用集成（云端和非云端）
-   厂商绑定

针对最后一点，随着各厂商支持 k8s，k8s 上的 Knaive 就提供了统一的 serverless 基础框架，同时 CNCF 着手知道统一的 [cloudevent](https://cloudevents.io/)和 [workdlow](https://github.com/cncf/wg-serverless/blob/master/workflow/spec/spec.md)。

## Serverless 的应用场景

-   web 应用：静态网站
-   后端自动化服务：移动应用后台，IOT/edge computing
-   后台应用：会话机器人（chatbot），语音处理
-   数据流处理：实时数据处理，批处理，MapReduce
-   基础设施自动化服务

## Serverless 的响应模式

-   同步/推模式: 响应 http 的 url
-   异步/事件模式：响应来自某个服务的事件，如 S3
-   基于数据流：数据流来自数据库，如 Amazon DynamoDB，或者数据流中心，如 Amazon Kinesis

![execution model](/images/serverless/execution-models.png)

## Serverless 的托管平台

| 厂商      | AWS Lambda                                                                          | GCP Functions                                              | Azure Functions                                                                                     | Alibaba Function                                                       | IBM Cloud Functions                                                                 |
| --------- | ----------------------------------------------------------------------------------- | ---------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| 免费      | 1M reqs<br/>400K Gb/s                                                               | 2M reqs<br/>400K Gb/s                                      | 1M reqs<br/>400K G/s                                                                                | 1M reqs<br/>400K Gb/s                                                  | Unlimited<br/>400K Gb/s                                                             |
| 价格      | $0\.20/1m req<br/>$0\.0000025 G/s                                                   | $0\.40/1m req<br/>$0\.00001667 G/s                         | $0\.20/1m req<br/>$0\.000016 G/s                                                                    | $0\.40/1m req<br/>$0\.00001668 b/s                                     | \$0\.000017 G/s                                                                     |
| 语言      | JS,Go,Python,Ruby,Java,C\#                                                          | JS,Go,Python                                               | JS,C\#                                                                                              | JS,Python,Java,PHP                                                     | JS,Go,Python,Ruby Java,C\#                                                          |
| 内存      | 128\-3008                                                                           | 128\-2048                                                  | ?\-1536                                                                                             | 128\-2048                                                              |                                                                                     |
| 响应速度  | 15m                                                                                 | 9m                                                         | 10m                                                                                                 | 10m                                                                    | 10m                                                                                 |
| 请求大小  | 6M                                                                                  | 10M                                                        | Unlimited?                                                                                          | 6M                                                                     | 5M                                                                                  |
| 响应大小  | 6M                                                                                  | 10M                                                        | Unlimited?                                                                                          | 6M                                                                     | 5M                                                                                  |
| 数量      | Unlimited                                                                           | 1000 functions/project                                     | Unlimited                                                                                           |                                                                        |                                                                                     |
| 执行次数  | 1000 parallel exec<br/>/account<br/>/region                                         | 1000 parallel executions                                   | Unlimited                                                                                           |                                                                        |                                                                                     |
| 执行时长  | 900 secs                                                                            | 540 secs                                                   | 600 secs                                                                                            |                                                                        |                                                                                     |
| 部署      | zip or jar upload, AWS SAM                                                          | zip upload, Google Cloud storage, repositories, GCloud CLI | Git, Dropbox, VS, Kudu console, One drive, zip deployment, Azure CLI                                |                                                                        |                                                                                     |
| 扩展方式  | Automatic                                                                           | Automatic                                                  | Manual or metered scaling                                                                           |                                                                        |                                                                                     |
| 事件源    | S3, SNS, SQS, SES, DynamoDB Kinesis, CloudWatch, HTTP                               | Cloud Pub/Sub, Cloud Storage, Firestore/Firebase, HTTP     | IoT, Hub Service Bus, HTTP Event grid/hub, Cosmos DB Storage                                        |                                                                        |                                                                                     |
| 编排      | AWS Step                                                                            |                                                            | Azure Logic \+ Durable                                                                              |                                                                        |                                                                                     |
| 安全      | AWS IAM, VPC support                                                                | Cloud IAM                                                  | OAuth providers \(Azure Active Directory, Facebook, Google, Microsoft Account, etc\.\)              |                                                                        | IBM Cloud IAM, OAuth providers \(Google, Facebook, GitHub, etc\.\)                  |
| 日志/监控 | AWS CloudWatch                                                                      | Stackdriver Monitoring                                     | Azure Application Insights                                                                          |                                                                        | IBM Cloud Functions Dashboard                                                       |
| 审计      | AWS CloudTrail                                                                      | Cloud Audit Logging                                        | Azure Audit Logs                                                                                    |                                                                        |                                                                                     |
| 报警      | AWS CloudWatch Alarms                                                               | Stackdriver Monitoring                                     | Azure Application Insights, Log Analytics, and Azure Monitor                                        |                                                                        | IBM Cloud Functions Dashboard                                                       |
| 开发工具  | AWS CodePipeline, AWS CodeBuild                                                     | gcloud CLI for functions                                   | Azure Portal, Azure Powershell, Azure CLI, Azure SDK                                                |                                                                        | IBM Cloud Functions UI, IBM Cloud Functions CLI, OpenWhisk Shell                    |
| 调试      | AWS X\-Ray                                                                          | Stackdriver Debugger                                       | Azure CLI(local), Azure App Service(remote)                                                         |                                                                        | wskdb: The OpenWhisk Debugger openwhisk\-light                                      |
| 其它限制  | [AWS 链接](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html) | [GCP 链接](https://cloud.google.com/functions/quotas)      | [Azure 链接](https://docs.microsoft.com/en-us/azure/azure-functions/functions-scale#service-limits) | [Alibaba 链接](https://www.alibabacloud.com/help/doc-detail/51907.htm) | [OpenWhisk 链接](https://github.com/ibm-cloud-docs/openwhisk/blob/master/limits.md) |

## Serverless 的开源框架

CNCF serverless 框架一览：

![CNCF serverless](/images/cncf/serverless.png)

对比如下：

| 框架                        | IronFunctions       | Fission       | Fn                                                               | Knative        | Kubeless      | Nuclio      | OpenFaaS                               | OpehWhisk                                              |
| --------------------------- | ------------------- | ------------- | ---------------------------------------------------------------- | -------------- | ------------- | ----------- | -------------------------------------- | ------------------------------------------------------ |
| license                     | Apache 2\.0         | Apache 2\.0   | Apache 2\.0                                                      | Apache 2\.0    | Apache 2\.0   | Apache 2\.0 | MIT                                    | Apache 2\.0                                            |
| 始于                        | 2016\.2             | 2016\.8       | 2016\.5                                                          | 2018\.1        | 2016\.11      | 2018\.7     | 2016\.12                               | 2016\.2                                                |
| 厂商                        | iron                | Platform9     | Oracle                                                           | Google, Redhat | Bitnami       | Iguazio     | VMWare                                 | IBM, Adobe                                             |
| Github Star                 | 2\.8k               | 5\.1k         | 4\.6k                                                            | 3k             | 5\.7k         | 3\.3k       | 17\.7k                                 | 4\.7k                                                  |
| Github Commits              | 904                 | 1，151        | 3,393                                                            | 4,435          | 1,008         | 1,360       | 1,894                                  | 2,780                                                  |
| Github 贡献者               | 32                  | 95            | 85                                                               | 174            | 85            | 55          | 147                                    | 177                                                    |
| Google 热度                 | \-                  | \-            | \-                                                               | 73/100         | 27/100        | \-          | 31/100                                 | 27/100                                                 |
| StackOverflow 热度\(posts\) | 2                   | 7             | 26                                                               | 110            | 8             | 3           | 28                                     | 197                                                    |
| 当前版本                    | 0\.2\.72            | 1\.9\.0       | 0\.3\.749                                                        | 0\.15\.0       | 1\.0\.6       | 1\.3\.27    | 0\.18\.17                              | 0\.9\.0                                                |
| 开发语言                    | Go                  | Go            | Go                                                               | Go             | Go            | Go          | Go                                     | Scala                                                  |
| 代码行数                    |                     | ~25K          |                                                                  | ~87K+25K       | ~12K          |             |                                        | ~64K                                                   |
| 底层技术                    | Postgres,<br/>Redis | fluentd       | sqlite<br/>Postgres<br/>MySQL,<br/>Bolt<br/>Redis<br/>Prometheus |                |               |             | AlertManager,<br/>Prometheus,<br/>Nats | CouchDB,<br/>Kafka,<br/>Nginx<br/>Redis,<br/>Zookeeper |
| Serverless 部署             | ✗                   | ✗             | ✓                                                                | ✓              | ✓             |             | ✓                                      | ✓                                                      |
| 特性                        |                     | 原生/依赖 k8s | 支持多语言，多环境                                               | 原生/依赖 k8s  | 原生/依赖 k8s | 性能        | 简单、轻巧, 社区庞大                   | 庞大社区，成熟                                         |
