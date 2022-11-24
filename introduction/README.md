# 工程简介

## 工程分类

| 分类     | 用途                                            | 备注                                                       |
| -------- | ----------------------------------------------- | ---------------------------------------------------------- |
| common   | 通用依赖包，一般打包为jar或pom，通过pom依赖引入 | 项目发布地址：https://mvnrepository.com/search?q=opensabre |
| base     | 基础设施应用，一般为web应用，通过接口提供服务。 |                                                            |
| docs     | 文档类工程                                      | 逐渐更新中                                                 |
| examples | 工程例子                                        | 逐渐增加中                                                 |

## 工程项目介绍

| 项目名称               | 地址                                                | 主要功能及用途                                               |
| ---------------------- | --------------------------------------------------- | ------------------------------------------------------------ |
| common-core            | https://github.com/opensabre/common-core            | 项目核心依赖，如一些基类，各项目都会使用的工具类等。该工程仅可依赖于其它工具类。 |
| common-web             | https://github.com/opensabre/common-web             | 项目的web依赖，如spring、web等通用的基类和配置类。该工程可依赖web相关包。 |
| common-test            | https://github.com/opensabre/common-test            | 项目测试工具类相关的封装，如测试校验、数据生成等。           |
| opensabre-base-starter | https://github.com/opensabre/opensabre-base-starter | opensabre starter包，包括opensabre-base-dependencies（springcloud依赖模块）和opensabre-base-starter（配置等初使化） |
| base-organization      | https://github.com/opensabre/base-organization      | 基础应用，如人员、组织、角色、权限等的管理应用。             |
| base-authorization     | https://github.com/opensabre/base-authorization     | 授权应用，负责发放、核验、回收等Token、Client的管理应用。    |
| base-k8s               | https://github.com/opensabre/base-k8s               | opensabre基础设施依赖中间件创建ymal，如nacos，redis，mq等中间件的初使化。 |
| docs                   | https://github.com/opensabre/docs                   | opensabre的文档工程。                                        |
| examples               | https://github.com/opensabre/examples               | examples工程，使用opensabre的一些案例工程。                  |
|                        |                                                     |                                                              |
|                        |                                                     |                                                              |

