# 框架模块设计

## 模块分层

opensabre-framework 是 Opensabre 项目群的框架层，主要提供公共模型、依赖版本管理和一组 Spring Boot Starter。应用项目按能力组合 starter，而不是直接依赖框架内部实现类。

| 层级 | 模块 | 定位 |
| --- | --- | --- |
| 依赖管理 | `opensabre-base-dependencies` | 统一管理 Spring Boot、Spring Cloud、Spring Cloud Alibaba 和 Opensabre 自身模块版本 |
| 公共基础 | `opensabre-web` | 公共返回模型、异常模型、校验注解、用户上下文、实体转换等轻量基础能力 |
| 测试支撑 | `opensabre-test` | 测试工具类，供框架模块和业务模块复用 |
| 通用启动 | `opensabre-starter-boot` | 应用基础配置、配置加密、监控、链路追踪、参数校验、脱敏；不传递 Servlet/OpenAPI UI |
| Web 技术栈 | `opensabre-starter-webmvc` | Spring MVC 应用入口，提供 WebMVC、Undertow、统一响应/异常、Springdoc 与 Knife4j |
| Web 技术栈 | `opensabre-starter-webflux` | Spring WebFlux 应用入口，提供响应式 Web 异常处理等能力 |
| 微服务能力 | `opensabre-starter-config` | Nacos 配置中心和 Opensabre 配置默认值 |
| 微服务能力 | `opensabre-starter-register` | Nacos 注册发现、实例元数据注册 |
| 微服务能力 | `opensabre-starter-rpc` | OpenFeign、LoadBalancer 和调用链路相关配置；Sentinel 可选 |
| 数据能力 | `opensabre-starter-persistence` | MyBatis-Plus、分页、防全表更新、非法 SQL 拦截；MVC 异常映射仅在 Servlet 应用装配 |
| 数据能力 | `opensabre-starter-cache` | JetCache Redis 多级缓存默认配置 |
| 事件能力 | `opensabre-starter-eda` | 进程内异步事件分发，MQ transport 由应用实现 |
| 治理能力 | `opensabre-starter-governance` | 审计日志和限次注解 SDK；审计以 EDA 事件异步交给 sysadmin 入库 |

## 推荐依赖组合

### 普通 WebMVC 服务

```groovy
implementation "io.github.opensabre:opensabre-starter-boot:0.7.1"
implementation "io.github.opensabre:opensabre-starter-webmvc:0.7.1"
```

`opensabre-starter-webmvc` 已经包含 `spring-boot-starter-web`，默认排除 Tomcat 并使用 Undertow。应用项目不需要再手工引入 `spring-boot-starter-web`。

### WebFlux 服务

```groovy
implementation "io.github.opensabre:opensabre-starter-webflux:0.7.1"
implementation "org.springframework.boot:spring-boot-starter-validation"
```

`opensabre-starter-webflux` 已经包含 `spring-boot-starter-webflux`。如果应用需要参数校验，再按需引入 `spring-boot-starter-validation`。

### 微服务应用

```groovy
implementation "io.github.opensabre:opensabre-starter-boot:0.7.1"
implementation "io.github.opensabre:opensabre-starter-webmvc:0.7.1"
implementation "io.github.opensabre:opensabre-starter-config:0.7.1"
implementation "io.github.opensabre:opensabre-starter-register:0.7.1"
implementation "io.github.opensabre:opensabre-starter-rpc:0.7.1"
```

`opensabre-starter-rpc` 已依赖 `opensabre-starter-register`，如果只需要远程调用能力，通常不必重复声明 register starter。

### 带数据库和缓存的服务

```groovy
implementation "io.github.opensabre:opensabre-starter-persistence:0.7.1"
implementation "io.github.opensabre:opensabre-starter-cache:0.7.1"
```

`opensabre-starter-persistence` 已依赖 `opensabre-starter-boot`。如果应用还要提供 HTTP 接口，仍需要引入 `opensabre-starter-webmvc` 或 `opensabre-starter-webflux`。

### 审计和限次治理

```groovy
implementation "io.github.opensabre:opensabre-starter-governance:0.7.1"
```

`opensabre-starter-governance` 面向业务应用提供 `@Audit`、`@RateLimit`、AOP 和自动装配。审计日志入库、限次场景、规则和计数统一由 sysadmin 管理。

## 当前设计评价

0.5.1 后，公共基础、通用启动、Web 技术栈、微服务、数据、事件和治理边界进一步明确。`opensabre-starter-webmvc` 接管 Servlet/OpenAPI/Knife4j，boot 保持技术栈中立；审计通过 EDA 解耦采集与入库；persistence 只在 Servlet 场景提供 MVC 异常映射。

后续可考虑新增场景 starter（例如 `opensabre-starter-service-webmvc`），组合 boot、webmvc、config、register、rpc、governance 等常见能力；底层 starter 继续保持细粒度。

## 依赖边界建议

| 建议 | 说明 |
| --- | --- |
| boot 不绑定具体 Web 技术栈 | 保留配置、监控、链路追踪、脱敏等通用能力，避免非 Web 服务或 WebFlux 服务被 MVC 依赖污染 |
| WebMVC/WebFlux 承担 Web 运行时 | 容器、统一异常、统一响应、Controller 增强、API 文档 UI 更适合放在 Web 技术栈 starter |
| persistence 避免强绑定 Web 异常处理 | 当前持久化异常 Advice 对 Web 项目友好，但非 Web 任务也可能依赖 persistence，后续可考虑用条件装配或移动到 Web starter |
| governance 只保留 SDK 职责 | 业务应用只使用注解和 AOP，审计数据、限次规则、计数存储统一交给 sysadmin |
| starter 自动配置增加条件保护 | 对第三方 Bean 使用 `@ConditionalOnClass`、`@ConditionalOnMissingBean`、`@ConditionalOnProperty`，降低应用自定义配置时的冲突 |
| 版本只在 BOM 管理 | 新增依赖优先进入 `opensabre-base-dependencies`，子模块尽量不写硬编码版本 |
