# 升级到 1.1.1

Framework 1.1.1 的编译与最低运行基线为 Java 21，默认容器运行时为 Java 25，并已在 JDK 21、JDK 25 完成 CI 验证。核心依赖版本为 Spring Boot 4.1.0、Spring Cloud 2025.1.2 与 Spring Cloud Alibaba 2025.1.0.0。

## 升级步骤

1. 将 Framework BOM、父 POM 或 Gradle 依赖升级为 `1.1.1`，保持 Java 编译 release 为 `21`。
2. 删除旧 `bootstrap.yml` / `bootstrap.properties`；将配置中心设置迁移到 `application.yml` 的 `spring.config.import`。
3. 使用 [配置中心](config.md) 的 `optional:nacos:` 模板，按环境设置公共配置 Data ID 与 Group。
4. 在 JDK 21 与 JDK 25 分别执行应用测试；使用 Nacos、注册发现、Feign、鉴权或数据库的应用必须完成真实环境回归。
5. 使用内部 Token 时，先发布并校验公共配置，再启用 `opensabre.security.internal-token.enabled=true`；不要以 optional 导入掩盖生产密钥缺失。

## Spring Cloud Alibaba 兼容性说明

OpenSabre 1.1.0 使用已发布的 `2025.1.0.0`。该版本的上游 GA 文档仅声明 Spring Boot 4.0.x；本版本基于 Framework JDK 21/25 CI 与示例回归在维护者批准的兼容性例外下发布。

因此，使用 Nacos、Sentinel、RocketMQ 或 Seata 的应用升级后必须至少验证：公共配置订阅与刷新、服务注册发现、核心远程调用、鉴权和健康检查。遇到上游兼容问题时，应先保留可复现的应用回归用例，再评估上游 GA 升级或临时规避方案。

## Native Image

Native Image 与 JVM 打包保持独立。需要原生镜像的应用应在 Linux GraalVM CI Runner 中执行构建和集成验证；不要在共享业务服务器上下载构建镜像或进行长时间 AOT 编译。

## 容器运行时优化

1. Framework 1.1.1 的 Jib 默认使用 `eclipse-temurin:25-jre-alpine`，并设置初始堆 10%、最大堆 60%、线程栈 `256k`、Metaspace/Direct Memory 各 `128m`。
2. 应用可按线程数、Netty buffer、动态代理和实际负载覆盖这些参数；不要在未压测前进一步降低上限。
3. Sentinel 依赖改为可选依赖；不使用 Sentinel 的应用不再将其带入镜像。需要 Sentinel 时显式声明依赖，并启用部署层的 Sentinel profile。
4. AppCDS 暂不默认启用。Spring Boot fat jar 生成的归档不能直接复用于 Jib exploded classpath，必须按应用单独生成和验证。
