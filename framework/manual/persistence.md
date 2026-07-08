# 数据持久化规范

## 1. MyBatis-Plus 配置
```yaml
mybatis-plus:
  configuration:
    map-underscore-to-camel-case: true
  global-config:
    db-config:
      id-type: auto
```

## 2. 事务管理
- 使用@Transactional注解声明式事务
- 事务传播级别配置
- 只读事务优化

## 3. 分页查询
```java
Page<User> page = new Page<>(1, 10);
userMapper.selectPage(page, Wrappers.emptyWrapper());
```

## 4. 多数据源配置
- 主从数据库配置
- 动态数据源切换
- 读写分离策略

## 5. 数据库迁移
- Flyway迁移脚本管理
- 版本控制规范
- 回滚脚本机制

## 6. 性能优化
- 二级缓存配置
- 慢SQL监控
- 连接池调优