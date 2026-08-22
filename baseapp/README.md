# 基础应用与部署

</br>

本页是项目群基础应用和部署入口。OpenSabre Framework 当前稳定版本为 `1.1.1`，Java 21 为编译基线、Java 25 为默认容器运行时。

基础应用按职责拆分为授权、组织、系统管理、网关运行时、网关控制面和管理前端；不要将历史 `baseapp` 目录理解为当前可部署的单体应用。

## 项目入口

- [Framework 1.1.1](https://github.com/opensabre/opensabre-framework)
- [base-authorization](https://github.com/opensabre/base-authorization)
- [base-organization](https://github.com/opensabre/base-organization)
- [base-sysadmin](https://github.com/opensabre/base-sysadmin)
- [base-gateway](https://github.com/opensabre/base-gateway)
- [base-gateway-admin](https://github.com/opensabre/base-gateway-admin)
- [opensabre-admin](https://github.com/opensabre/opensabre-admin)
- [base-k8s](https://github.com/opensabre/base-k8s)

## 当前技术基线

- Spring Boot `4.1.0`
- Spring Cloud `2025.1.2`
- Spring Cloud Alibaba `2025.1.0.0`（应用级 Nacos、注册发现和业务链路需单独回归）
- Framework `1.1.1`

各应用的数据库迁移、环境变量、健康检查和 CI 发布细节，以对应仓库 `README.md`、`docs/`、POM 和部署脚本为准。

## 发布与部署

Framework 发布后，应用按仓库 CI 构建镜像并部署到目标环境；基础设施和初始化配置由 `base-k8s` 管理。部署前执行数据库迁移，部署后检查容器状态、Actuator 健康端点、Nacos 注册和关键 API/UI 链路。

历史说明：本目录原有的单体示例文档仅保留作迁移参考。

RBAC权限管理、授权认证、网关管理、服务治理、审计日志等系统管理基础应用。定义了相关开发规范、风格并落地在框架层，开箱即

用，支持Docker、Kubenetes的部署。让项目开发快速进入业务开发，而不需过多时间花费在架构搭建和编码风格规范上。
