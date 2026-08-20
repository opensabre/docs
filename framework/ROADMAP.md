# 产品路线

## 1.1.1

- Spring Boot 4.1.0、Java 21 编译基线，以及 JDK 21/JDK 25 CI 验证。
- Java 25 Jib 容器运行时、堆/线程栈/Metaspace 参数优化，并保持 Sentinel 为可选能力。
- Nacos 公共配置的可选启动语义与安全的内部 Token 热更新。
- 后续继续完善 Linux GraalVM Native Image 与 OCI 双模式构建；Native 验证使用一次性 CI Runner，不在共享业务服务器构建。


## 系统架构
