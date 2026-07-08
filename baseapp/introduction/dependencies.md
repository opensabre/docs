# 依赖说明

## 1. 项目依赖概述
本工程基于Spring Boot 3.4.x构建，主要依赖分为以下三类：
- **基础框架依赖**：Spring Cloud Alibaba 2023.0.4
- **数据存储依赖**：MyBatis-Plus 3.5.3, MySQL Driver 8.0.x
- **安全认证依赖**：Spring Security OAuth2 5.7.6

## 2. 核心依赖列表
| 组件名称 | 版本 | 作用 |
|----------|------|------|
| spring-boot-starter-web | 3.4.1 | Web MVC支持 |
| spring-cloud-starter-gateway | 2023.0.4 | API网关核心 |
| mybatis-plus-boot-starter | 3.5.3.1 | ORM框架 |
| spring-boot-starter-data-redis | 3.4.1 | 缓存支持 |

## 3. 模块依赖关系
- gateway模块依赖：
  - spring-cloud-starter-loadbalancer
  - spring-cloud-starter-circuitbreaker-reactor-resilience4j
- organization-service模块依赖：
  - spring-boot-starter-data-jpa
  - spring-security-oauth2-resource-server

## 4. 版本管理策略
所有依赖版本通过父pom的dependencyManagement统一管理，禁止子模块单独指定版本号。