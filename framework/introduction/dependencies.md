# 依赖说明

## 主要开源组件版本

| 组件                               | 版本         | 备注        |
| --------------------------------- | ------------ | ---------- |
| spring-boot-dependencies          | 3.4.1        |            |
| spring-cloud-dependencies         | 2024.0.0     |            |
| spring-cloud-alibaba-dependencies | 2023.0.3.2   |            |
| mysql-connector-j                 | 8.2.0        | mysql驱动   |
| mybatis-plus-boot-starter         | 3.5.5        | 数据持久化   |
| jetcache-starter-redis-lettuce    | 2.7.7        | 多级缓存    |
| knife4j-openapi3                  | 4.5.0        | springdoc swagger3.0 |
| springdoc-openapi-starter         | 2.7.0        | OpenAPI UI |
| lombok                            | 1.18.42      |            |
| hutool                            | 5.8.35       | 工具类      |
| guava                             | 33.4.0-jre   | 工具类      |
| jasypt-spring-boot-starter        | 3.0.5        | 配置加密     |

## Opensabre 模块版本

当前文档对应 framework `0.4.0`。业务项目建议通过 `opensabre-base-dependencies` 统一管理 Opensabre starter 版本。

| 模块 | 说明 |
| --- | --- |
| `opensabre-starter-boot` | 基础启动、监控、链路追踪、校验、配置加密、脱敏、API 文档 |
| `opensabre-starter-webmvc` | Spring MVC、Undertow、统一响应、统一异常 |
| `opensabre-starter-webflux` | Spring WebFlux 响应式应用入口 |
| `opensabre-starter-config` | Nacos 配置中心 |
| `opensabre-starter-register` | Nacos 注册发现 |
| `opensabre-starter-rpc` | OpenFeign、LoadBalancer、Sentinel |
| `opensabre-starter-persistence` | MyBatis-Plus、分页、SQL 拦截 |
| `opensabre-starter-cache` | JetCache 多级缓存 |
| `opensabre-starter-eda` | Spring Cloud Bus + RabbitMQ |
| `opensabre-starter-governance` | 审计日志、限次注解 SDK |

## 组件功能实现

|  服务     | 使用技术                 |   进度        |    备注   |
|----------|-------------------------|--------------|-----------|
|  注册中心 | Nacos                   |   ✅          |           |
|  配置中心 | Nacos                   |   ✅          |           |
|  消息总线 | SpringCloud Bus+Rabbitmq|   🏗          |           |
|  应用网关 | SpringCloud Gateway     |   ✅          |  多种维度的流量控制（服务、IP、用户等），后端可配置化🏗          |
|  授权认证 | Spring Security OAuth2  |   ✅          |  Jwt模式   |
|  服务容错 | SpringCloud Sentinel    |   ✅          |           |
|  服务调用 | SpringCloud OpenFeign   |   ✅          |           |
|  对象存储 | Minio                   |   🏗          |           |
|  多级缓存 | Jetcache                |   ✅          |           |
|  任务调度 |                         |   🏗          |           |
|  API文档 | swagger3.0              |   ✅          |           |
|  审计日志 | governance starter + sysadmin | ✅    | 业务应用使用 `@Audit`，sysadmin 统一入库 |
|  限次治理 | governance starter + sysadmin | ✅    | 业务应用使用 `@RateLimit`，sysadmin 统一管理场景和计数 |
|  数据权限 |                         |   🏗          |  使用mybatis对原查询做增强，业务代码不用控制，即可实现。         |

## 组件中涉及的中间件

| 组件            | 版本       | 备注                    |
| -------------- | --------- | ----------------------- |
| mysql          | 8.0+      | 主要用于数据持久化         |
| redis          | 6.0+      | 主要用于数据缓存           |
| rabbitmq       | 3.8+      | 主要用于事件通知等          |
| nacos          | 2.0+      | 主要用于应用实例注册        |
