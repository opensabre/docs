# 工程简介

## 工程类型

| 分类                | 用途                                                       | 备注                                  |
| ------------------ | ---------------------------------------------------------- | ------------------------------------ |
| opensabre-framework| 微服务框架工程，主要封装服务框架的starter，进行装配，简化使用服务框架  |                                      |
| base               | 基础应用类项目，一般为web应用，通过接口提供基础的服务，如人员组织管理服务，权限认证服务等。|                       |
| docs               | 文档类工程                                                    | 逐渐更新中                            |
| examples           | 微服务框架使用的工程例子                                         | 逐渐完善中                            |

## 工程项目介绍

| 项目名称                 | 地址                                                | 主要功能及用途                                               |
| ---------------------- | --------------------------------------------------- | --------------------------------------------------------- |
| opensabre-framework    | https://github.com/opensabre/opensabre-framework    | opensabre服务框架工程starter包，详见[]() |
| base-organization      | https://github.com/opensabre/base-organization      | 基础应用，如人员、组织、角色、权限等的管理应用。             |
| base-authorization     | https://github.com/opensabre/base-authorization     | 授权应用，负责发放、核验、回收等Token、Client的管理应用。    |
| base-system            | https://github.com/opensabre/base-system            |     |
| base-k8s               | https://github.com/opensabre/base-k8s               | opensabre基础设施依赖中间件创建ymal，如nacos，redis，mq等中间件的初使化。 |
| docs                   | https://github.com/opensabre/docs                   | opensabre的文档工程。                                        |
| examples               | https://github.com/opensabre/examples               | examples工程，使用opensabre的一些案例工程。                  |

## opensabre-framework服务框架子工程介绍
| moudle名称                   | 子目录                           | 主要功能及用途                                                 |
| ----------------------------| ------------------------------- | ------------------------------------------------------------ |
| opensabre-base-release      | opensabre-base-release          |  opensabre服务框架父工程，定义通用配置，如打包、仓库、发布等各模块配置  |
| opensabre-base-dependencies | opensabre-base-dependencies     |  opensabre服务框架工程的依赖管理，如版本、配置等定义  |
| opensabre-base-starter      | opensabre-base-starter          |  |
| opensabre-starter-register  | opensabre-starter-register      |  注册中心相关starter模块，用于封装简化注册中心使用，约定使用规范等|
| opensabre-starter-config    | opensabre-starter-config        |  配置中心相关starter模块，用于封装简化配置中心使用，约定使用规范等|
| opensabre-starter-boot      | opensabre-starter-boot          |  springboot web相关starter模块，用于封装统一异常处理、统一报文、swagger文档等相关规范约定等|
| opensabre-starter-rpc       | opensabre-starter-rpc           |  远程调用相关starter模块，用于封装应用间相互调用、路由、超时等通用配置模块|
| opensabre-starter-cache     | opensabre-starter-cache         |  缓存相关starter模块，用于封装多级缓存，如内存、redis等缓存的规范和约定     |
| opensabre-starter-persistence| opensabre-starter-persistence  |  数据持久化相关starter模块，用于封装数据库操作、实体转换等数据持久化规范和约定|
| opensabre-starter-eda       | opensabre-starter-eda           |  事件驱动starter模块，用于封装和规范消息、事件等操作|
| opensabre-test              | opensabre-test                  |  通用测试工具包|
| opensabre-web               | opensabre-web                   |  web操作通用工具、配置等|
