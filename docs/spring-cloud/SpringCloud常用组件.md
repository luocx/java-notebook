## 服务注册与发现
一个分布式架构系统通常由多个独立服务应用组成，应用与应用间**相互提供服务**，为保证服务的高可用性，应用通常会采用集群部署的方式。
随着业务不断拆分，独立的应用越来越多，随之而来的问题是，**应用之间该如何管理依赖应用的服务地址**。
  
当某台服务器不可用时（比如：宕机、网络故障），如何保证服务请求能够发到可用的机器？  
服务器扩容时，如何在不重启情况下将流量接入新扩容机器？
  
为了解决此类服务治理问题，**注册中心**出现了
  
* spring-cloud-alibaba-nacos
* spring-cloud-netflix-eureka
* spring-cloud-consul
* spring-cloud-zookeeper

## 配置中心
* spring-cloud-config
* spring-cloud-alibaba-nacos
* apollo

## 网关/路由
* spring-cloud-gateway
* spring-cloud-netflix-zuul

## 服务与服务间的调用
* spring-cloud-openfeign

## 负载均衡
* spring-cloud-loadbalancer

## 断路器