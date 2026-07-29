# 字典组件

字典组件在 0.7.0 统一“应用声明、Sysadmin 管理、应用读取”链路。应用通过 `DictionaryService` 读取本地缓存，避免每次展示或校验都访问管理服务。

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
