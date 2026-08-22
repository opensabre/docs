# 依赖说明

## 1. 项目依赖概述
本项目群当前基于 OpenSabre Framework 1.1.1，使用 Spring Boot 4.1.0、Spring Cloud 2025.1.2 和 Spring Cloud Alibaba 2025.1.0.0。各 base 应用通过父 POM 或 BOM 管理版本，具体以对应仓库当前 POM 为准。

## 2. 核心依赖列表
| 组件名称 | 版本 | 作用 |
|----------|------|------|
| spring-boot-dependencies | 4.1.0 | Spring Boot 依赖管理 |
| spring-cloud-dependencies | 2025.1.2 | Spring Cloud 依赖管理 |
| spring-cloud-alibaba-dependencies | 2025.1.0.0 | Nacos 等 Alibaba 组件；使用前完成应用级回归 |
| mybatis-plus-boot-starter | 3.5.17 | ORM 框架 |
| jetcache-starter-redis-lettuce | 2.8.0.RC | 多级缓存 |

## 3. 模块依赖关系
- gateway模块依赖：
  - spring-cloud-starter-loadbalancer
  - spring-cloud-starter-circuitbreaker-reactor-resilience4j
- organization-service模块依赖：
  - spring-boot-starter-data-jpa
  - spring-security-oauth2-resource-server

## 4. 版本管理策略
所有依赖版本通过父pom的dependencyManagement统一管理，禁止子模块单独指定版本号。
