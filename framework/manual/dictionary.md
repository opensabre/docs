# 字典组件

字典组件在 0.7.0 统一“应用声明、Sysadmin 管理、应用读取”链路。应用通过 `DictionaryService` 读取本地缓存，避免每次展示或校验都访问管理服务。

> **兼容性要求：** Framework `0.7.1` 需配合已合并 [base-sysadmin #17](https://github.com/opensabre/base-sysadmin/pull/17) 与 [#18](https://github.com/opensabre/base-sysadmin/pull/18) 的后端使用。该后端已于 2026-07-29 部署验证；若使用更早的 Sysadmin 镜像，请保持 `registration-enabled=false`。

![字典组件架构](framework/assets/dictionary-component.svg)

## 配置

```yaml
opensabre:
  governance:
    dictionary:
      enabled: true
      registration-enabled: true
      registration-token: ${DICTIONARY_REGISTRATION_TOKEN}
      preload-codes: [order_status, payment_channel]
```

`registration-enabled` 默认关闭。注册凭据未设置时回退到错误码目录凭据。

## 后端协议

| 方法 | 路径 | 用途 |
| --- | --- | --- |
| `POST` | `/dicts/snapshots` | 携带 `X-Opensabre-Dictionary-Token` 注册应用完整快照 |
| `GET` | `/dicts/{dictCode}/items/all` | 返回包含停用项的完整列表，供缓存与历史回显使用 |

启用注册前应先验证：无效凭据返回 401、有效快照返回 200、`items/all` 包含停用项且 options 接口只包含启用项。后端实现与验证记录见 [base-sysadmin #10](https://github.com/opensabre/base-sysadmin/issues/10)。

## 声明字典

```java
@Bean
DictionaryProvider orderDictionaryProvider() {
    return DictionaryProvider.of(DictionaryDefinition.of(
        "order_status", "订单状态", OrderStatus.values(),
        OrderStatus::getCode, OrderStatus::getLabel));
}
```

应用就绪后异步携带 `X-Opensabre-Dictionary-Token` 注册快照；失败不阻塞启动。

## 读取与缓存

```java
List<DictionaryItem> items = dictionaryService.items("order_status");
String label = dictionaryService.labelOf("order_status", value).orElse("未知状态");
boolean valid = dictionaryService.contains("order_status", value);
dictionaryService.refresh("order_status");
```

- `items` 只返回启用项；缓存未命中时从 Sysadmin 加载。
- `labelOf` 包含停用项，保证历史数据回显。
- `contains` 只在启用项中校验。
- `refresh` 清除本地缓存，下次访问重新加载。

默认实现使用 JetCache `shortTime` 本地区域并开启穿透保护。Sysadmin 不可用时抛出 `DictionaryUnavailableException`，业务不能将其误判为“字典值非法”。0.7.0 不主动订阅变更通知，需要调用 `refresh` 或等待缓存过期。
