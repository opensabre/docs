# 治理 SDK

`opensabre-starter-governance` 从 `0.4.0` 开始提供审计日志和限次能力，`0.7.0` 增加错误码目录与字典组件，`0.7.1` 为启动注册任务增加重试、退避和可观测性。1.1.1 继续提供这些能力，业务应用只需要引入 starter 并添加注解，具体数据入库、规则管理和计数逻辑统一由 sysadmin 承担。

## 引入依赖

```xml
<dependency>
    <groupId>io.github.opensabre</groupId>
    <artifactId>opensabre-starter-governance</artifactId>
    <version>1.1.1</version>
</dependency>
```

Gradle：

```groovy
implementation 'io.github.opensabre:opensabre-starter-governance:1.1.1'
```

## 配置项

默认配置由 starter 提供：

```yaml
opensabre:
  governance:
    enabled: true
    sysadmin:
      service-id: base-sysadmin
    audit:
      enabled: true
    ratelimit:
      enabled: true
      fail-open: true
    usage:
      transport: EDA
```

| 配置项 | 说明 |
| --- | --- |
| `opensabre.governance.enabled` | governance 总开关 |
| `opensabre.governance.sysadmin.service-id` | sysadmin 服务名，默认 `base-sysadmin` |
| `opensabre.governance.audit.enabled` | 审计能力开关 |
| `opensabre.governance.ratelimit.enabled` | 限次能力开关 |
| `opensabre.governance.ratelimit.fail-open` | sysadmin 调用异常时是否放行 |
| `opensabre.governance.usage.transport` | 使用量上报通道：默认 `EDA`，可选 `HTTP` |

## 审计日志

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

审计切面会记录操作类型、模块、操作人、客户端 IP、请求地址、请求参数、响应结果、异常信息和耗时。0.5.1 起它发布本地 EDA 事件，由默认处理器调用 sysadmin 的审计接口统一入库；事件处理不会阻塞业务请求。

## 限次

```java
import io.github.opensabre.governance.ratelimit.annotations.RateLimit;
import io.github.opensabre.governance.ratelimit.enums.RateLimitAlgorithmType;
import io.github.opensabre.governance.ratelimit.enums.RateLimitDimension;

@RateLimit(
    sceneCode = "send_sms",
    maxCount = 5,
    period = 60,
    algorithm = RateLimitAlgorithmType.SLIDING_WINDOW,
    dimensions = {RateLimitDimension.IP},
    key = "#mobile"
)
@PostMapping("/sms")
public boolean sendSms(@RequestParam String mobile) {
    return smsService.send(mobile);
}
```

限次切面会解析注解和 SpEL key，调用 sysadmin 的 `/ratelimit/check` 接口完成限次判断。sysadmin 侧负责动态场景、规则、算法和计数存储。

## 使用量统计

验证码、限次和通知可通过类型化 recorder 记录尝试、成功和失败；默认使用 EDA 远程 transport，业务应用需提供对应 transport 实现。若暂不使用消息通道，可配置 `opensabre.governance.usage.transport=HTTP`，由 Sysadmin 的 `/usage-counters/records` 接口异步聚合。

```java
captchaUsageRecorder.generateSuccess("login");
rateLimitUsageRecorder.allowed("api-login");
notificationUsageRecorder.templateSendFailure("password-reset");
```

使用量事件只用于观测，不能替代同步的限次放行判断；记录中不得包含验证码、通知正文、手机号等敏感数据。

## 使用建议

- 关键写操作、登录、导出、上传等接口建议加 `@Audit`。
- 查询接口按需审计，避免审计数据过大。
- 短信、验证码、登录、导出等高风险接口建议加 `@RateLimit`。
- 限次场景优先在 sysadmin 后台创建，注解中的 `sceneCode` 与后台场景编码保持一致。
- 生产环境建议确认 `base-sysadmin` 服务可用，再按业务容忍度设置 `fail-open`。
