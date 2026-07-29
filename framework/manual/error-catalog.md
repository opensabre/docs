# 错误码目录

错误码目录是 0.7.0 新增的声明式治理能力。应用仍按原方式返回错误码；Starter 只把元数据汇总到 `base-sysadmin`，用于统一检索归属、文案和废弃状态。

![错误码目录流程](framework/assets/error-catalog-flow.svg)

## 配置

```yaml
opensabre:
  governance:
    error-catalog:
      enabled: true
      registration-token: ${ERROR_CATALOG_REGISTRATION_TOKEN}
```

所有应用与 Sysadmin 使用同一注册凭据，凭据只从环境变量或配置中心注入。

## 声明错误码

```java
@Bean
ErrorCatalogProvider orderErrorCatalogProvider() {
    return ErrorCatalogProvider.of("order", OrderErrorType.values());
}
```

需要完整元数据时直接返回 `ErrorCatalogEntry`：字段包括 `code`、`message`、`module`、`httpStatus`、`publicVisible`、`deprecated` 和 `description`。`code` 与 `message` 必填；同一应用内相同 code 定义冲突时，本次快照会跳过。

## 注册流程

1. `ApplicationReadyEvent` 后异步汇总全部 Provider。
2. 生成包含应用名、Framework 版本和条目的完整快照。
3. 携带 `X-Opensabre-Error-Catalog-Token` 注册到 Sysadmin。
4. Sysadmin 校验凭据和错误码归属后保存。

注册失败只记录告警，不阻塞启动，也不影响运行时错误返回。旧错误码建议先标记 `deprecated=true`，不要复用给其他语义。
