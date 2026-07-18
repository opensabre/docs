# 事件中心

`opensabre-starter-eda` 在 0.5.0 提供进程内异步事件分发。它定义稳定的事件信封、发布器和处理器接口，不绑定 RabbitMQ、Kafka 或 Spring Cloud Bus；需要跨进程投递时，由应用实现 transport 并复用本地处理器。

## 引入依赖

```groovy
implementation 'io.github.opensabre:opensabre-starter-eda:0.5.0'
```

## 发布与订阅

```java
import io.github.opensabre.eda.api.EdaEvent;
import io.github.opensabre.eda.api.EdaEventHandler;
import io.github.opensabre.eda.api.EdaEventPublisher;

@Service
class OrderService {
    private final EdaEventPublisher publisher;

    void created(OrderCreated payload) {
        publisher.publishLocal(EdaEvent.of("order.created", "order-service", payload));
    }
}

@Component
class OrderCreatedHandler implements EdaEventHandler<OrderCreated> {
    @Override
    public String eventType() {
        return "order.created";
    }

    @Override
    public void handle(EdaEvent<OrderCreated> event) {
        // 异步执行本地业务处理
    }
}
```

`eventType` 是处理器路由和跨服务协议的稳定标识；`source` 用于记录事件来源，`payload` 为业务负载。处理器应自行保证幂等，并处理失败重试或补偿。

## 对接消息队列

应用可按消息系统实现 `EventTransport` Bean，并通过 `publisher.publishRemote(event)` 触发远程投递；消费端反序列化后调用 `publisher.publishLocal(event)`，即可复用相同处理器。消息确认、重试、死信、持久化和 JSON/Avro/Protobuf 协议均属于 transport 的职责。

默认异步执行器可通过 `opensabre.eda.*` 配置调整；业务对顺序、线程池隔离或可靠投递有要求时，应显式配置并补充集成测试。
