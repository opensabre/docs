# 工程简介

## 工程地址

Github地址：https://github.com/opensabre/opensabre-framework

Gitee地址：https://gitee.com/opensabre/opensabre-framework

## 子工程介绍

当前 framework 稳定版本为 `0.7.1`，工程按依赖管理、公共基础能力、Web 技术栈、微服务能力、数据能力和治理能力拆分。业务应用建议通过 `opensabre-base-dependencies` 统一管理版本，并按场景组合 starter。

| module名称 | 子目录 | 主要功能及用途 |
| --- | --- | --- |
| opensabre-framework | opensabre-framework | opensabre 服务框架父工程，聚合各子模块，统一编译、打包、发布和版本号配置 |
| opensabre-base-dependencies | opensabre-base-dependencies | Opensabre BOM，统一管理 Spring Boot、Spring Cloud、Spring Cloud Alibaba、第三方组件和 Opensabre 模块版本 |
| opensabre-web | opensabre-web | 公共返回模型、异常模型、校验注解、用户上下文、实体转换等轻量 Web 基础能力 |
| opensabre-starter-boot | opensabre-starter-boot | 通用启动 starter，提供配置加密、监控、链路追踪、参数校验和脱敏；不传递 Servlet 或 OpenAPI UI |
| opensabre-starter-webmvc | opensabre-starter-webmvc | Spring MVC 应用入口，封装 WebMVC、Undertow、统一响应/异常、Springdoc 与 Knife4j |
| opensabre-starter-webflux | opensabre-starter-webflux | Spring WebFlux 响应式应用入口，封装 WebFlux 运行时依赖和响应式异常处理 |
| opensabre-starter-register | opensabre-starter-register | 注册中心 starter，封装 Nacos 注册发现、实例元数据注册和默认配置 |
| opensabre-starter-config | opensabre-starter-config | 配置中心 starter，封装 Nacos 配置中心接入和 Opensabre 默认配置 |
| opensabre-starter-rpc | opensabre-starter-rpc | 远程调用 starter，封装 OpenFeign、LoadBalancer、Sentinel 和调用链路相关配置 |
| opensabre-starter-cache | opensabre-starter-cache | 缓存 starter，封装 JetCache Redis 多级缓存默认配置和使用规范 |
| opensabre-starter-persistence | opensabre-starter-persistence | 数据持久化 starter，封装 MyBatis-Plus、分页、防全表更新、非法 SQL 拦截和持久化异常处理 |
| opensabre-starter-eda | opensabre-starter-eda | 进程内异步事件分发 starter；MQ 发布、消费和可靠投递由应用 transport 实现 |
| opensabre-starter-governance | opensabre-starter-governance | 治理 SDK starter，提供审计日志、限次注解、AOP 和自动装配，并统一调用 sysadmin 管理数据与规则 |
| opensabre-test | opensabre-test | 通用测试工具包，供框架模块和业务模块复用 |
