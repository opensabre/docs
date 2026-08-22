# OpenSabre 1.1.1 发布核对计划

## 已完成

1. Framework POM、发布记录和迁移指南已统一到 `1.1.1`。
2. Framework 文档中的 Maven/Gradle starter 示例已统一到 `1.1.1`，Java 基线统一为 21+。
3. 依赖说明已同步到 Spring Boot `4.1.0`、Spring Cloud `2025.1.2`、Spring Cloud Alibaba `2025.1.0.0`、MyBatis-Plus `3.5.17` 和 JetCache `2.8.0.RC`。
4. 项目群入口已补充 base-gateway、base-gateway-admin、opensabre-admin 和 base-k8s 的职责与链接。
5. 按开发者确认，Framework 1.1.1 已正式发布，各 base 应用已升级、构建并部署。

## 发布后核对顺序

1. 对照各仓库当前 POM，确认没有应用仍引用 Framework `0.x` 或 `1.1.0`。
2. 从 CI 记录确认各应用构建使用正确的工作流，并记录镜像标签；生产环境避免使用不可追踪的 `latest`。
3. 在目标服务器检查 Compose/容器状态、应用健康端点、Nacos 注册和关键日志。
4. 验证授权登录、组织/菜单、在线用户、网关路由发布和管理端页面等关键链路。
5. 使用 Spring Cloud Alibaba 的应用单独验证 Nacos 配置订阅、服务注册发现和业务调用；该组件上游 GA 兼容性仍需应用级回归。
6. 发现失败时保留上一镜像和配置版本，先回退再修复，不直接覆盖线上数据。

## 当前未纳入本次修改的内容

- 各应用仓库自身的业务模块文档仍由所属仓库维护，docs 只维护入口、版本基线和跨项目流程。
- 部署时间、镜像 digest、服务器运行状态等环境事实不写死在长期文档中，应从 CI、Compose 和服务器检查结果获取。
- Wiki 的 `raw/project-docs/gateway-web` 与 `gateway-admin` 软链接已修复，分别指向当前 `base-gateway/docs` 和 `base-gateway-admin/docs`。
