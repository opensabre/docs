# 审计日志

## 简介

`opensabre-starter-boot` 提供方法级审计日志能力。业务方法添加 `@Audit` 后，框架会在方法执行结束后发布 `AuditEvent`，默认监听器会记录事件日志。业务系统如需写入数据库、消息队列或审计平台，可以自定义 `ApplicationListener<AuditEvent>`。

## 启用审计

在启动类上添加 `@EnabledAudit`：

```java
@EnabledAudit
@SpringBootApplication
public class SampleApplication {
    public static void main(String[] args) {
        SpringApplication.run(SampleApplication.class, args);
    }
}
```

## 标记审计方法

```java
@Audit(
    operationType = OperationType.CREATE,
    description = "新增用户",
    module = "USER",
    response = true,
    key = "#userForm.username"
)
@PostMapping("/users")
public boolean add(@RequestBody UserForm userForm) {
    return userService.add(userForm);
}
```

## 注解参数

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `operationType` | `OperationType` | 是 | 操作类型 |
| `description` | `String` | 是 | 操作描述 |
| `module` | `String` | 否 | 业务模块 |
| `request` | `boolean` | 否 | 是否记录请求参数，默认 `true` |
| `response` | `boolean` | 否 | 是否记录响应结果，默认 `false` |
| `key` | `String` | 否 | 操作对象关键值，支持 SpEL |

## 操作类型

`OperationType` 当前包含：

```java
CREATE, UPDATE, DELETE, QUERY, LOGIN, LOGOUT, SCAN, EXPORT, IMPORT, DOWNLOAD, UPLOAD
```

## 自定义事件处理

```java
@Component
public class AuditEventHandler implements ApplicationListener<AuditEvent> {

    @Override
    public void onApplicationEvent(AuditEvent event) {
        AuditInfo auditInfo = (AuditInfo) event.getSource();
        // 写入数据库、消息队列或外部审计平台
    }
}
```

## 使用建议

- 重点记录新增、修改、删除、登录、导出等关键操作。
- 查询类接口默认不建议全部记录，避免审计数据过大。
- `response = true` 只用于必要接口，避免记录过大的响应内容。
- 敏感字段需要结合脱敏规则处理后再落库。

