## 更新日志

### **2019-10-18**

1.使用nacos替代eureka为服务的注册中心

2.使用nacos替代apollo为服务的配置中心

3.引入使用sentinel替换掉hystrix，引入sentinel-dashboard

4.使用jetcache作两级缓存，优化缓存性能

5.网关启动时加载数据库中的路由到redis缓存

6.其它已知bug修复