# 审计日志

## 简介

`opensabre-starter-governance` 提供方法级审计日志能力。业务方法添加 `@Audit` 后，starter 会通过 AOP 采集审计信息，并调用 sysadmin 的审计接口统一入库管理。

## 引入依赖

```xml
<dependency>
    <groupId>io.github.opensabre</groupId>
    <artifactId>opensabre-starter-governance</artifactId>
    <version>0.5.0</version>
</dependency>
```

## 启用审计

默认自动装配会启用审计能力。如需显式启用，也可以在启动类上添加 `@EnabledAudit`：

```java
import io.github.opensabre.governance.audit.annotations.EnabledAudit;

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
import io.github.opensabre.governance.audit.annotations.Audit;
import io.github.opensabre.governance.audit.annotations.OperationType;

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

## 统一入库

starter 默认把审计信息发布为本地 `governance.audit.created` EDA 事件，再由默认处理器交给 sysadmin：

```yaml
opensabre:
  governance:
    sysadmin:
      service-id: base-sysadmin
    audit:
      enabled: true
```

sysadmin 负责审计日志的统一入库、查询和后续管理。

## 自定义事件处理

```java
import io.github.opensabre.eda.api.EdaEvent;
import io.github.opensabre.eda.api.EdaEventHandler;
import io.github.opensabre.governance.audit.entity.AuditInfo;
import io.github.opensabre.governance.audit.event.DefaultAuditEventHandler;
import org.springframework.stereotype.Component;

@Component
public class AuditEventHandler implements EdaEventHandler<AuditInfo> {

    @Override
    public String eventType() {
        return DefaultAuditEventHandler.EVENT_TYPE;
    }

    @Override
    public void handle(EdaEvent<AuditInfo> event) {
        AuditInfo auditInfo = event.payload();
        // 写入数据库、消息队列或外部审计平台
    }
}
```

## 使用建议

- 重点记录新增、修改、删除、登录、导出等关键操作。
- 查询类接口默认不建议全部记录，避免审计数据过大。
- `response = true` 只用于必要接口，避免记录过大的响应内容。
- 敏感字段需要结合脱敏规则处理后再落库。
