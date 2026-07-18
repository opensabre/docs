# 更新日志

## **2026-07-18**

### 0.5.0

1. `opensabre-starter-eda` 调整为进程内异步事件分发基础设施，提供 `EdaEvent`、`EdaEventPublisher` 与 `EdaEventHandler`；消息队列传输由业务应用按需实现，不再由 starter 强绑定 RabbitMQ。

2. governance 审计改为发布 `governance.audit.created` EDA 事件，再由默认处理器调用 sysadmin 入库。需要自定义处理时，实现同一事件类型的 `EdaEventHandler<AuditInfo>`。

3. Servlet、Springdoc、Knife4j、OpenAPI 配置和 REST 映射通知从 `opensabre-starter-boot` 收敛到 `opensabre-starter-webmvc`。MVC 服务必须同时声明 boot 与 webmvc starter；WebFlux 服务不再被 Servlet 依赖污染。

4. persistence 的 MVC 持久化异常映射改为仅在 Servlet Web 应用中自动装配，非 Web 或 WebFlux 应用可安全复用持久化 starter。

5. 统一依赖 BOM 与所有框架模块发布为 `0.5.0`。

## **2026-07-01**

### 0.4.0

1. 新增 `opensabre-starter-governance`，统一承载治理类 SDK 能力。

2. 审计日志从 `opensabre-starter-boot` 拆分到 governance starter，业务应用引入 starter 后通过 `@Audit` + AOP 自动记录审计日志，并统一调用 sysadmin 入库。

3. 新增限次 SDK 能力，提供 `@RateLimit` + AOP + 自动装配，由 starter 调用 sysadmin 的限次检查接口统一管理规则和计数。

4. governance starter 新增 `opensabre.governance.*` 配置项，支持配置 sysadmin 服务名和治理功能开关。

5. `opensabre-base-dependencies` 增加 governance starter 版本管理，业务应用可直接按 starter 方式引入。

6. `opensabre-starter-boot` 移除审计相关代码，职责收敛到启动、日志、脱敏等基础能力。

### 0.3.0

1. 新增 `opensabre-starter-webflux`，补齐响应式 Web 应用的 starter 支持。

2. 重构 Web 模块边界，拆分并整理 `opensabre-web`、`opensabre-starter-webmvc`、`opensabre-starter-webflux` 的依赖职责。

3. 调整 WebMVC starter 依赖定位，补齐运行所需的 Servlet、Validation、Lombok 等依赖声明。

4. 聚合 WebFlux starter 运行时依赖，降低业务应用手工补依赖成本。

5. 修复 boot 模块编译缺少 Servlet API 的问题。

6. 升级 Lombok 版本以适配更新的 JDK 编译环境。

## **2025-10-25**

### 0.2.0

1. 新增审计日志能力，在 `opensabre-starter-boot` 中提供 `@Audit`、`@EnabledAudit`、`OperationType`、审计切面、审计事件和默认事件处理器。

2. `opensabre-base-dependencies` 增加审计相关依赖管理，框架版本升级至 `0.2.0`。

3. 优化 Jib Docker 打包参数，支持容器镜像构建发布。

4. 升级 HttpClient 5 相关依赖，修复 Feign HC5 调用场景中的兼容问题。

5. 调整 Servlet API 依赖范围，避免运行期缺少 Servlet 相关类。

## **2025-10-10**

### 0.1.1

1. 框架主版本升级至 `0.1.1`，并升级 Spring Boot 至 `3.4.1`、Spring Cloud 至 `2024.0.0`、Spring Cloud Alibaba 至 `2023.0.3.2`。

2. 优化依赖边界，新增 `opensabre-starter-webmvc`，为 Web 能力从 `opensabre-web` 中拆分做准备。

3. Maven 发布链路迁移到 `central-publishing-maven-plugin`，调整 Sonatype Central 发布地址和 Maven 插件版本。

4. API 文档能力升级，修复 Knife4j 文档显示问题，并增加 Bearer Authorization 鉴权参数。

5. 升级 MySQL 驱动、MyBatis-Plus、Guava 等第三方依赖版本。

6. 增加 Jib Docker 打包插件，补齐后续镜像构建能力。

## **2024-08-06**

1. JDK升级至17+，服务框架springboot升级到3.2.3，springcloud版本升级到2023.0.0

2. Api文档集成knife4j，方便接口文档查看、传递、调试。

## **2023-09-06**

1. springboot版本升级至2.7.14，内置tomcat替换为undertow。

2. Rest统一报文 Result time精确到毫秒 SSS。

3. opensabre framework框架配置项添加IDE提示功能功能。

4. 统一响应报文添加数据脱敏注解，添加注解可对敏感信息进行脱敏处理，防止外泄。

5. 集成jasypt-spring-boot-starter，敏感配置项如密码等，可加密后放置配置文件中。

6. 增加日志脱敏功能，防止密码、手机号、身份证号等敏感信息通过日志泄漏。

## **2023-06-06**

1. springboot应用启动完成后，收集所有rest接口并发布spring接口注册事件，便于后续做接口治理工作。

2. starter-boot模块扩展增加Mobile校验注解，扩展字段枚举类型选项EnumString校验工具。

3. 全局异常处理模块，增加MethodNotSupportedException、HttpMessageNotReadableException报文转换类异常。

4. 新增框架版本号常量与环境变更，banner中增加spring和框架的版本号打印，方面用户快速识别版本信息。

5. starter-cache模块重构，保留jetcache，去掉cache redis。

6. 优化统一响应对象的封装，Rest直接返回原始类型即可，无需要包装为Result对象。处理swagger rest接口被result包装的问题。

7. 重构entity转换方式，删除无用转换类。

8. 移除base-starter模块，使用starter-boot统一封装使用springboot。

9. config配置类均改为@Import进行初使化，避免手工初使化。

10. 应用启动注册到nacos时，将框架版本等元数据信息注册到注册中心，方面后续版本管理与路由扩展。

11. 引入hutool工具类。

## **2022-11-18**

1. 服务框架单独拆出工程维护，服务框架各模块分包，通过opensabre-starter形式分包加载

2. 服务框架springboot升级到2.7.5，springcloud版本升级到2021.0.4

3. 增加examples项目展示框架的使用，增加docs项目文档库

4. 完善opensabre-starter-boot功能


## **2019-10-18**

1. 使用nacos替代eureka为服务的注册中心

2. 使用nacos替代apollo为服务的配置中心

3. 引入使用sentinel替换掉hystrix，引入sentinel-dashboard

4. 使用jetcache作两级缓存，优化缓存性能

5. 网关启动时加载数据库中的路由到redis缓存

6. 其它已知bug修复
