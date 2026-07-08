# 快速入门

# 快速入门指南

## 环境要求
- JDK 17+
- MySQL 8.0+
- Redis 6.0+

## 1. 获取代码
```bash
git clone https://github.com/opensabre/baseapp.git
cd baseapp
```

## 2. 数据库初始化
```sql
CREATE DATABASE baseapp DEFAULT CHARACTER SET utf8mb4;
-- 执行/db/ddl下的初始化脚本
```

## 3. 配置修改
```yaml
# application.yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/baseapp
    username: root
    password: your_password

redis:
  host: localhost
  port: 6379
```

## 4. 启动服务
```bash
mvn clean install
cd baseapp-gateway
mvn spring-boot:run
```

## 5. 验证部署
访问 http://localhost:8080/swagger-ui.html 查看API文档