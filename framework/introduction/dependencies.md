# 依赖说明

## 主要开源组件版本

| 组件                               | 版本         | 备注        |
| --------------------------------- | ------------ | ---------- |
| spring-boot-dependencies          | 3.4.1        |            |
| spring-cloud-dependencies         | 2024.0.0     |            |
| spring-cloud-alibaba-dependencies | 2023.0.3.2   |            |
| mysql-connector-j                 | 8.0.33       | mysql驱动   |
| jetcache-starter-redis-lettuce    | 2.7.5        | 多级缓存    |
| knife4j-openapi3                  | 4.3.0        | springdoc swagger3.0 |
| lombok                            | 1.18.30      |            |
| hutool-all                        | 5.8.25       | 工具类      |
| mapstruct                         | 1.5.5.Final  | 对象转换     |
| jasypt-spring-boot-starter        | 3.0.5        | 配置加密     |

## 组件功能实现

|  服务     | 使用技术                 |   进度        |    备注   |
|----------|-------------------------|--------------|-----------|
|  注册中心 | Nacos                   |   ✅          |           |
|  配置中心 | Nacos                   |   ✅          |           |
|  消息总线 | SpringCloud Bus+Rabbitmq|   🏗          |           |
|  应用网关 | SpringCloud Gateway     |   ✅          |  多种维度的流量控制（服务、IP、用户等），后端可配置化🏗          |
|  授权认证 | Spring Security OAuth2  |   ✅          |  Jwt模式   |
|  服务容错 | SpringCloud Sentinel    |   ✅          |           |
|  服务调用 | SpringCloud OpenFeign   |   ✅          |           |
|  对象存储 | Minio                   |   🏗          |           |
|  多级缓存 | Jetcache                |   ✅          |           |
|  任务调度 |                         |   🏗          |           |
|  API文档 | swagger3.0              |   ✅          |           |
|  数据权限 |                         |   🏗          |  使用mybatis对原查询做增强，业务代码不用控制，即可实现。         |

## 组件中涉及的中间件

| 组件            | 版本       | 备注                    |
| -------------- | --------- | ----------------------- |
| mysql          | 8.0+      | 主要用于数据持久化         |
| redis          | 6.0+      | 主要用于数据缓存           |
| rabbitmq       | 3.8+      | 主要用于事件通知等          |
| nacos          | 2.0+      | 主要用于应用实例注册        |
