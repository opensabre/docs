# 框架模块设计

## 模块分层

opensabre-framework 是 Opensabre 项目群的框架层，主要提供公共模型、依赖版本管理和一组 Spring Boot Starter。应用项目按能力组合 starter，而不是直接依赖框架内部实现类。

| 层级 | 模块 | 定位 |
| --- | --- | --- |
| 依赖管理 | `opensabre-base-dependencies` | 统一管理 Spring Boot、Spring Cloud、Spring Cloud Alibaba 和 Opensabre 自身模块版本 |
| 公共基础 | `opensabre-web` | 公共返回模型、异常模型、校验注解、用户上下文、实体转换等轻量基础能力 |
| 测试支撑 | `opensabre-test` | 测试工具类，供框架模块和业务模块复用 |
| 通用启动 | `opensabre-starter-boot` | 应用基础配置、配置加密、监控、链路追踪、参数校验、脱敏、OpenAPI 基础配置 |
| Web 技术栈 | `opensabre-starter-webmvc` | Spring MVC 应用入口，提供 WebMVC 依赖、Undertow 容器、统一响应和统一异常处理 |
| Web 技术栈 | `opensabre-starter-webflux` | Spring WebFlux 应用入口，提供响应式 Web 异常处理等能力 |
| 微服务能力 | `opensabre-starter-config` | Nacos 配置中心和 Opensabre 配置默认值 |
| 微服务能力 | `opensabre-starter-register` | Nacos 注册发现、实例元数据注册 |
| 微服务能力 | `opensabre-starter-rpc` | OpenFeign、LoadBalancer、Sentinel 和调用链路相关配置 |
| 数据能力 | `opensabre-starter-persistence` | MyBatis-Plus、分页、防全表更新、非法 SQL 拦截和持久化异常处理 |
| 数据能力 | `opensabre-starter-cache` | JetCache Redis 多级缓存默认配置 |
| 事件能力 | `opensabre-starter-eda` | Spring Cloud Bus + RabbitMQ 事件总线配置 |
| 治理能力 | `opensabre-starter-governance` | 审计日志和限次注解 SDK，通过 sysadmin 统一入库和规则管理 |

## 推荐依赖组合

### 普通 WebMVC 服务

```groovy
implementation "io.github.opensabre:opensabre-starter-boot:0.4.0"
implementation "io.github.opensabre:opensabre-starter-webmvc:0.4.0"
```

`opensabre-starter-webmvc` 已经包含 `spring-boot-starter-web`，默认排除 Tomcat 并使用 Undertow。应用项目不需要再手工引入 `spring-boot-starter-web`。

### WebFlux 服务

```groovy
implementation "io.github.opensabre:opensabre-starter-webflux:0.4.0"
implementation "org.springframework.boot:spring-boot-starter-validation"
```

`opensabre-starter-webflux` 已经包含 `spring-boot-starter-webflux`。如果应用需要参数校验，再按需引入 `spring-boot-starter-validation`。

### 微服务应用

```groovy
implementation "io.github.opensabre:opensabre-starter-boot:0.4.0"
implementation "io.github.opensabre:opensabre-starter-webmvc:0.4.0"
implementation "io.github.opensabre:opensabre-starter-config:0.4.0"
implementation "io.github.opensabre:opensabre-starter-register:0.4.0"
implementation "io.github.opensabre:opensabre-starter-rpc:0.4.0"
```

`opensabre-starter-rpc` 已依赖 `opensabre-starter-register`，如果只需要远程调用能力，通常不必重复声明 register starter。

### 带数据库和缓存的服务

```groovy
implementation "io.github.opensabre:opensabre-starter-persistence:0.4.0"
implementation "io.github.opensabre:opensabre-starter-cache:0.4.0"
```

`opensabre-starter-persistence` 已依赖 `opensabre-starter-boot`。如果应用还要提供 HTTP 接口，仍需要引入 `opensabre-starter-webmvc` 或 `opensabre-starter-webflux`。

### 审计和限次治理

```groovy
implementation "io.github.opensabre:opensabre-starter-governance:0.4.0"
```

`opensabre-starter-governance` 面向业务应用提供 `@Audit`、`@RateLimit`、AOP 和自动装配。审计日志入库、限次场景、规则和计数统一由 sysadmin 管理。

## 当前设计评价

整体模块拆分方向是合理的：公共基础能力、通用启动能力、Web 技术栈、微服务能力、数据能力、治理能力已经分开，应用可以按需组合。`opensabre-starter-webmvc` 接管 `spring-boot-starter-web` 和容器选择后，新建 MVC 项目的依赖边界也更清晰。审计和限次从 boot/sysadmin 内部实现拆成 governance starter 后，业务应用接入成本降低，sysadmin 继续承担集中管理职责。

需要继续收敛的地方主要有两类：

1. `opensabre-starter-boot` 仍包含部分 WebMVC 相关依赖和配置，例如 API 文档 UI、`MappingInfoHandler`。如果目标是让 boot 完全保持通用，应把 API 文档和 MVC 映射采集迁移到 WebMVC starter，或者新增 `opensabre-starter-doc`。
2. 若希望应用侧只声明一个业务入口依赖，可以新增更高层的场景 starter，例如 `opensabre-starter-service-webmvc`，组合 boot、webmvc、config、register、rpc、governance 等常见微服务能力；底层 starter 保持细粒度。

## 依赖边界建议

| 建议 | 说明 |
| --- | --- |
| boot 不绑定具体 Web 技术栈 | 保留配置、监控、链路追踪、脱敏等通用能力，避免非 Web 服务或 WebFlux 服务被 MVC 依赖污染 |
| WebMVC/WebFlux 承担 Web 运行时 | 容器、统一异常、统一响应、Controller 增强、API 文档 UI 更适合放在 Web 技术栈 starter |
| persistence 避免强绑定 Web 异常处理 | 当前持久化异常 Advice 对 Web 项目友好，但非 Web 任务也可能依赖 persistence，后续可考虑用条件装配或移动到 Web starter |
| governance 只保留 SDK 职责 | 业务应用只使用注解和 AOP，审计数据、限次规则、计数存储统一交给 sysadmin |
| starter 自动配置增加条件保护 | 对第三方 Bean 使用 `@ConditionalOnClass`、`@ConditionalOnMissingBean`、`@ConditionalOnProperty`，降低应用自定义配置时的冲突 |
| 版本只在 BOM 管理 | 新增依赖优先进入 `opensabre-base-dependencies`，子模块尽量不写硬编码版本 |
