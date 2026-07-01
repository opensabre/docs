# Opensabre一站式微服务框架

</br>

💪Opensabre是基于SpringCloud2024的微服务开发平台，整合了Spring Security、Spring Cloud Alibaba等组件。包含了基础的

RBAC权限管理、授权认证、网关管理、服务治理、审计日志、限次管理等系统管理基础应用。定义了相关开发规范、风格并落地在框架层，开箱即

用，支持Docker、Kubenetes的部署。让项目开发快速进入业务开发，而不需过多时间花费在架构搭建和编码风格规范上。

## 当前版本

当前 framework 主线版本为 `0.4.0`，主要模块包括：

- `opensabre-base-dependencies`：统一依赖版本管理。
- `opensabre-web`：公共返回模型、异常、校验、上下文等基础能力。
- `opensabre-starter-boot`：启动、配置加密、监控、链路追踪、脱敏、OpenAPI 等基础能力。
- `opensabre-starter-webmvc` / `opensabre-starter-webflux`：MVC 与响应式 Web 技术栈入口。
- `opensabre-starter-config` / `opensabre-starter-register` / `opensabre-starter-rpc`：配置中心、注册发现、远程调用等微服务能力。
- `opensabre-starter-persistence` / `opensabre-starter-cache` / `opensabre-starter-eda`：持久化、缓存、事件能力。
- `opensabre-starter-governance`：审计日志和限次的注解 SDK，统一调用 sysadmin 进行数据入库和规则管理。

</br>

[![Stargazers over time](https://starchart.cc/zhoutaoo/SpringCloud.svg)](https://starchart.cc/zhoutaoo/SpringCloud)
