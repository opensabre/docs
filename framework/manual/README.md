# 用户手册

本章节主要介绍 opensabre-framework 主要功能和各功能使用方法。

## 功能索引

| 能力 | 说明 |
| --- | --- |
| 基础能力 | 日志、校验、异常、统一响应、配置加密、脱敏、API 文档 |
| Web 技术栈 | WebMVC、WebFlux 应用入口和运行时能力 |
| 微服务能力 | 注册中心、配置中心、远程调用、事件总线 |
| 数据能力 | MyBatis-Plus 持久化、分页、非法 SQL 防护、多级缓存 |
| 治理 SDK | 审计日志、限次、错误码目录与字典组件，通过 sysadmin 统一管理 |
| 服务间安全 | 内部短 Token、逐跳重签、可信用户上下文与双密钥轮换 |

## 1.1.1 升级

- [升级到 1.1.1](migration-1.1.md)
- [配置中心与 Nacos 公共配置](config.md)

当前版本为 `1.1.1`：Spring Boot 4.1.0、Java 21 编译基线、Java 25 容器运行时，并在 JDK 21 与 JDK 25 验证。使用 Spring Cloud Alibaba 的应用须完成自身集成回归，详见升级指南。

## 0.7.x 新增能力

- [错误码目录](framework/manual/error-catalog.md)
- [字典组件](framework/manual/dictionary.md)
- [内部 Token 认证](framework/manual/internal-token.md)

0.7.x 补齐治理注册重试与可观测性、错误码归属协议，以及内部 Token 的可信 Claims、Authority 语义和双凭据防护。
