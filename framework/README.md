# Opensabre一站式微服务框架

</br>

💪Opensabre是基于 Spring Boot 4.1、Spring Cloud 2025.1 的微服务开发平台，整合了 Spring Security、Spring Cloud Alibaba 等组件。包含了基础的

RBAC权限管理、授权认证、网关管理、服务治理、审计日志、限次管理等系统管理基础应用。定义了相关开发规范、风格并落地在框架层，开箱即

用，支持Docker、Kubenetes的部署。让项目开发快速进入业务开发，而不需过多时间花费在架构搭建和编码风格规范上。

## 当前版本

当前 framework 版本为 `1.1.1`，Java 21 为编译基线、Java 25 为默认容器运行时，并在 JDK 21、JDK 25 上持续验证。主要模块包括：

- `opensabre-base-dependencies`：统一依赖版本管理。
- `opensabre-web`：公共返回模型、异常、校验、上下文等基础能力。
- `opensabre-starter-boot`：启动、配置加密、监控、链路追踪、脱敏等通用能力，不绑定 Servlet Web 技术栈。
- `opensabre-starter-webmvc` / `opensabre-starter-webflux`：MVC 与响应式 Web 技术栈入口；OpenAPI 与 Knife4j 归属 MVC starter。
- `opensabre-starter-config` / `opensabre-starter-register` / `opensabre-starter-rpc`：配置中心、注册发现、远程调用等微服务能力。
- `opensabre-starter-persistence` / `opensabre-starter-cache` / `opensabre-starter-eda`：持久化、缓存、进程内异步事件能力。
- `opensabre-starter-governance`：审计日志、限次、错误码目录和字典组件，统一调用 sysadmin 完成治理。
- `opensabre-starter-security`：内部短 Token、逐跳重签、可信用户上下文和双密钥轮换。

升级说明见[版本说明](VERSONS.md)和[配置中心](manual/config.md)。使用 Spring Cloud Alibaba 组件时，请按应用完成 Nacos、服务注册与业务链路回归；其 `2025.1.0.0` 上游 GA 版本仍仅正式声明 Spring Boot 4.0.x。

</br>

[![Stargazers over time](https://starchart.cc/zhoutaoo/SpringCloud.svg)](https://starchart.cc/zhoutaoo/SpringCloud)
