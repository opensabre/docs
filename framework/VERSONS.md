## 更新日志

### **2022-11-18**

1.服务框架单独拆出工程维护，服务框架各模块分包，通过opensabre-starter形式分包加载

2.服务框架springboot升级到2.7.5，springcloud版本升级到2021.0.4

3.增加examples项目展示框架的使用，增加docs项目文档库

4.完善opensabre-starter-boot功能


### **2019-10-18**

1.使用nacos替代eureka为服务的注册中心

2.使用nacos替代apollo为服务的配置中心

3.引入使用sentinel替换掉hystrix，引入sentinel-dashboard

4.使用jetcache作两级缓存，优化缓存性能

5.网关启动时加载数据库中的路由到redis缓存

6.其它已知bug修复