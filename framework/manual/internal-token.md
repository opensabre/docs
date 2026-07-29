# 内部 Token 认证

`opensabre-starter-security` 是 0.7.0 新增的服务间认证组件。它将已校验身份转换为短期、面向单个目标服务的 HS256 Token，并在每一跳重新签发。

![内部 Token 逐跳认证](framework/assets/internal-token-flow.svg)

## 安全边界

- 浏览器到网关继续使用外部 OAuth JWT；网关不签发内部 Token。
- 首应用完成 JWT 校验和接口授权后签发第一跳 Token。
- 后续每跳面向目标服务重签，禁止原样转发。
- Token 固定放在 `x-client-token`，重签前清除旧身份 Header。
- 0.7.0 支持 Servlet、Feign、受控 RestClient；暂不支持 WebFlux/WebClient。

## 配置

```yaml
opensabre:
  security:
    internal-token:
      enabled: true
      required: false
      key-config-version: 1
      active-key-id: key-202607
      active-key: ${OPENSABRE_INTERNAL_TOKEN_ACTIVE_KEY}
      previous-key-id: key-202606
      previous-key: ${OPENSABRE_INTERNAL_TOKEN_PREVIOUS_KEY:}
      ttl: 60s
      max-ttl: 120s
      clock-skew: 5s
      max-hop: 8
      allowed-issuers: [base-organization, base-order]
      allowed-extension-keys: [tenant, locale]
```

密钥须 Base64 编码且解码后至少 32 字节。`required=true` 只适用于纯内部应用；同时接收网关 JWT 的应用保持 false。

## 逐跳验证

接收方验证签名、`kid`、`iss/src`、`aud/dst`、时间、Token 大小和 `hop`。`jti/parent_jti` 串联调用链，`roles/scope` 承载授权快照，`ext` 只能包含白名单键。

Spring Security 应用需将 `InternalTokenAuthenticationFilter` 放在 `BearerTokenAuthenticationFilter` 之前。验证成功后，身份同步绑定到 `UserContextHolder`，请求结束时清理。

Feign 自动使用当前应用名作为签发方、目标服务名作为 audience，并生成新 `jti`、增加 `hop`。RestClient 必须显式开启 `rest-client-enabled`，且目标在 `rest-client-allowed-targets` 白名单中；只使用 Spring Boot 注入的 `RestClient.Builder` 才会应用拦截器。

## 双密钥轮换

1. 新密钥成为 active，旧 active 移到 previous，并递增配置版本。
2. 所有应用加载新版本后，新请求使用 active，验证同时接受 active/previous。
3. previous 至少保留 `max-ttl + clock-skew` 后再清除。

共享 HMAC 需要配合网络隔离、配置读取最小权限、短 TTL、快速轮换和审计。日志、审计和管理页面严禁输出密钥或完整 Token。
