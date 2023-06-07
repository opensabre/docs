# 更新日志

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