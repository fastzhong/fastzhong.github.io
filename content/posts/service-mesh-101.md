+++
title = "Service Mesh 101"
date = 1999-07-01T13:08:47+08:00
readingTime = true
categories = ["系统设计 & 架构"]
tags = ["microservices"]
toc = true
draft = true
+++

<!--more-->


3-tier        
microservices 
service discovery 
container/k8s - service 
service mesh 
    connectivity  
    security   
    monitoring   

mTLS 
latency-aware load balancing  
retries           
traffic shifting 
circuit breaking   
observability  
## 服务网格（Service Mesh ）是什么

s


Service Mesh 是在 networking 层和 application 层之间的系统基础设施，实现如下功能：

-   服务发现 service discovery
-   服务配置 configuration
-   服务访问安全 authentication & authorization
-   负载均衡，限流，熔断，重试/降级 network traffic control
-   通讯加密/解密 network traffic encryption
-   服务追踪 traceability
-   服务状态和性能 observability
-   通过 policy 统一管理上述功能 control policy

咋一看，技术特点和 J2EE/EJB 很像啊，其实目标是很像的，只是实现的方式不同，Service Mesh 把这些功能，从 application 关注中剥离出来，既不是通过库，也不是通过中间件，而是做成了基础设施，降至 OS 网络层面，这和近几年容器、k8s 以及微服务的技术发展息息相关。

## Service Mesh 技术生态一览

Service Mesh 最早的技术实现者有 Linkd，Envoy，到现在著名的 Istio，其相关的生态



dapr 