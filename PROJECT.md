## 工程类型

| 分类                | 用途                                                       | 备注                                  |
| ------------------ | ---------------------------------------------------------- | ------------------------------------ |
| opensabre-framework| 微服务框架工程，主要封装服务框架的starter，进行装配，简化使用服务框架  |                                      |
| base               | 基础应用类项目，一般为web应用，通过接口提供基础的服务，如人员组织管理服务，权限认证服务等。|                       |
| docs               | 文档类工程                                                    | 逐渐更新中                            |
| examples           | 微服务框架使用的工程例子                                         | 逐渐完善中                            |

## 工程项目

| 工程名称                | 地址                                               | 主要功能及用途                                                          |
| ------------------- | ------------------------------------------------ | ---------------------------------------------------------------- |
| opensabre-framework | https://github.com/opensabre/opensabre-framework | opensabre服务框架工程多moudle，框架包、starter包等，详见[服务框架](/framework/README) |
| base-organization   | https://github.com/opensabre/base-organization   | 基础应用，如人员、组织、角色、权限等的管理应用。                                         |
| base-authorization  | https://github.com/opensabre/base-authorization  | 授权应用，负责发放、核验、回收等Token、Client的管理应用。                               |
| base-sysadmin       | https://github.com/opensabre/base-sysadmin       | 基础应用，如验证码发送、验证，审计信息管理、字典管理等                                      |
| base-k8s            | https://github.com/opensabre/base-k8s            | Docker Compose 基础设施、数据库初始化、Nacos 公共配置和应用编排。                |
| base-gateway        | https://github.com/opensabre/base-gateway        | 运行时网关，负责路由转发、过滤器和接口文档聚合。                              |
| base-gateway-admin  | https://github.com/opensabre/base-gateway-admin  | 网关唯一控制面，负责路由、策略、发布、审计和回滚。                              |
| opensabre-admin     | https://github.com/opensabre/opensabre-admin     | Vue 管理端，承载基础应用和网关控制面操作入口。                                |
| docs                | https://github.com/opensabre/docs                | opensabre的文档工程。                                                  |
| examples            | https://github.com/opensabre/examples            | examples工程，使用opensabre的一些案例工程。                                   |

</br>

## 1.1.1 发布核对

- Framework 1.1.1 已发布，base 应用已按当前 POM 升级并完成部署（发布状态以项目群 CI 和服务器运行状态为准）。
- docs 本次同步统一版本、依赖基线、入口链接和部署职责；逐项核对记录见[发布核对计划](RELEASE-1.1.1.md)。
- 新环境部署优先参考 [base-k8s 部署文档](https://github.com/opensabre/base-k8s/blob/main/README.md)，不要继续使用旧的单体 `baseapp` 启动命令。

> [!tip|label:提示]
> </br>
> 以上地址亦可使用gitee访问项目，如 https://gitee.com/opensabre/examples
