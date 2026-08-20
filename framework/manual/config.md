# 配置中心

Framework 1.1.1 使用 Spring Boot Config Data API 导入 Nacos 配置，不再依赖 bootstrap context。

```yaml
spring:
  config:
    import: optional:nacos:${OPENSABRE_COMMON_CONFIG_DATA_ID:opensabre-common.yml}?group=${OPENSABRE_COMMON_CONFIG_GROUP:DEFAULT_GROUP}&refreshEnabled=true
```

`optional:` 只覆盖公共配置在首次启动时不可取得的情形：Nacos 暂不可用、Data ID 不存在或内容为空时，未启用内部 Token 的应用仍可启动。它不应被用来掩盖生产配置错误。

若设置 `opensabre.security.internal-token.enabled=true`，上线前必须确认公共配置已发布并包含有效密钥。内部 Token 热更新会校验密钥长度、版本递增和 TTL 等约束；空或非法内容被拒绝后，应用继续使用最后一个有效快照，不会回退到固定共享密钥。

可用环境变量：

- `OPENSABRE_COMMON_CONFIG_DATA_ID`：默认 `opensabre-common.yml`。
- `OPENSABRE_COMMON_CONFIG_GROUP`：默认 `DEFAULT_GROUP`。

升级到 1.1.1 后，应删除旧的 bootstrap 配置，并在目标环境验证 Config Data 订阅、服务注册和配置刷新。
