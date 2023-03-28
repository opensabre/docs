# 多级缓存

## 简介

<span>
在日常项目开发中，为了提高服务的并发和性能，缓存是非常常见且重要的手段。缓存是一种在内存中暂存数据的技术，以加快对这些数据的访问速度。通过缓存，可以把经常用到的数据暂存在高速缓存中，以便下次访问时可以更快地获取。</br></br>
opensabre-framework默认集成了caffeine，分别提供基于jvm和redis的多级缓存。并且opensabre-framework还内置了local(JVM)的1分钟、5分钟(默认)、2小时和remote(redis)5分钟、2小时(默认)、12小时供用户直接开箱即用。若有其它特殊需求，可通过自定义配置进行配置和扩展。
</span>

项目地址：`https://github.com/opensabre/examples/sample-cache`

## 前置

| 依赖软件                    | 要求     | 备注                                             |
| ------------------------- | -------- | ------------------------------------------------|
| java                      | 11+      | 必须                                             |
| redis                     | 6.0+     | 必须                                             |
| opensabre-starter-boot    | 0.0.5    | web项目用于测试                                   |
| opensabre-starter-cache   | 0.0.5    | 必须                                             |

## 开发

### 引入starter包

gradle依赖引入`implementation 'io.github.opensabre:opensabre-starter-cache:0.0.5'`

```groovy
dependencies {
    implementation 'io.github.opensabre:opensabre-starter-cache:0.0.5'
}
```

maven引入

```xml
<!-- opensabre cache starter -->
<dependency>
    <groupId>io.github.opensabre</groupId>
    <artifactId>opensabre-starter-cache</artifactId>
    <version>0.0.5</version>
</dependency>
```

### 缓存使用

1. `RemoteProvider.java` ，模拟远程调用。

```java
package io.github.opensabre.sample.cache.provider;

import lombok.extern.slf4j.Slf4j;
import org.apache.commons.lang3.RandomStringUtils;
import org.springframework.stereotype.Service;

@Slf4j
@Service
public class RemoteProvider {

    public String getRemote(String key) {
        log.info("load from remote : {}", key);
        try {
            Thread.sleep(2000L);
        } catch (InterruptedException e) {
            log.error("remote invoke error", e);
        }
        return key + ":" + RandomStringUtils.randomGraph(5);
    }
}
```

2. `IConfigService.java` ，service接口，利用注解进行缓存。

```java
package io.github.opensabre.sample.cache.service;

import com.alicp.jetcache.anno.CacheInvalidate;
import com.alicp.jetcache.anno.CacheType;
import com.alicp.jetcache.anno.CacheUpdate;
import com.alicp.jetcache.anno.Cached;

public interface IConfigService {

    String getConfig(String key);

    //获取并缓存，使用shortTime(local缓存1分钟，remote缓存5分钟)进行user缓存，userId为key，缓存名称为 userCache:${userId}
    @Cached(name = "userCache:", area = "shortTime", key = "#userId", cacheType = CacheType.BOTH)
    String getUser(String userId);
    //更新缓存，使用当前值更新缓存名称为 userCache:${userId}
    @CacheUpdate(name = "userCache:", area = "shortTime", key = "#userId", value = "#user")
    void updateUser(String userId, String user);
    //清除缓存名称为 userCache:${userId} 的缓存
    @CacheInvalidate(name = "userCache:", area = "shortTime", key = "#userId")
    void deleteUser(String userId);
}
```

`ConfigService.java` service接口实现

```java
package io.github.opensabre.sample.cache.service;

import io.github.opensabre.sample.cache.provider.RemoteProvider;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;

@Service
public class ConfigService implements IConfigService {
    @Resource
    RemoteProvider remoteProvider;

    @Override
    public String getConfig(String key) {
        return remoteProvider.getRemote(key);
    }

    @Override
    public String getUser(String userId) {
        return remoteProvider.getRemote(userId);
    }

    @Override
    public void updateUser(String userId, String user) {
    }

    @Override
    public void deleteUser(String userId) {
    }
}
```

3. `CacheController.java` , controller提供接口

```java
package io.github.opensabre.sample.cache.rest;

import io.github.opensabre.sample.cache.service.IConfigService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.*;

import javax.annotation.Resource;

@Slf4j
@RestController
@RequestMapping("/cache")
public class CacheController {

    @Resource
    private IConfigService configService;

    @GetMapping("/config")
    public String hello(@RequestParam String key) {
        String value = configService.getConfig(key);
        return key + ":" + value;
    }

    @GetMapping("/user")
    public String getUser(@RequestParam String userId) {
        String user = configService.getUser(userId);
        return userId + ":" + user;
    }

    @PostMapping("/user")
    public String updateUser(@RequestParam String userId, String user) {
        configService.updateUser(userId, user);
        return userId + ":" + "update";
    }

    @DeleteMapping("/user")
    public String deleteUser(@RequestParam String userId) {
        configService.deleteUser(userId);
        return userId + ":" + "delete";
    }
}
```

## 测试

```shell
root@xxxxx # curl http://localhost:8080/cache/user?userId=zhangsan

{   
    "code":"000000",
    "mesg":"处理成功",
    "time":"2023-03-22T14:46:58.826435Z",
    "data":"zhangsan:zhangsan:G0aci"
}
```

## 文档

swagger文档地址：`http://localhost:8080/swagger-ui/index.html`

### 默认缓存配置项

**可通过环境变量注入自己redis地址和端口，亦可在springboot配置文件中添加自己的缓存配置**

```yaml
spring:
  jackson:
    time-zone: GMT+8

jetcache:
  statIntervalMinutes: 15
  areaInCacheName: false
  hidePackages: io.github.opensabre
  local:
    # 默认5分钟本地缓存
    default:
      type: caffeine
      keyConvertor: fastjson
      expireAfterWriteInMillis: 300000
      expireAfterAccessInMillis: 180000
    # 長時本地緩存2小时，主要用于要求时效一般
    longTime:
      type: caffeine
      keyConvertor: fastjson
      expireAfterWriteInMillis: 3600000
      expireAfterAccessInMillis: 1800000
    # 短時本地緩存1分钟，主要用于要求时效较高的配置
    shortTime:
      type: caffeine
      keyConvertor: fastjson
      expireAfterWriteInMillis: 60000
      expireAfterAccessInMillis: 40000
  remote:
    # 默认2小时的远程缓存
    default:
      type: redis.lettuce
      expireAfterWriteInMillis: 7200000
      keyConvertor: fastjson
      valueEncoder: java
      valueDecoder: java
      poolConfig:
        minIdle: 5
        maxIdle: 20
        maxTotal: 50
      uri: redis://${REDIS_HOST:localhost}:${REDIS_PORT:6379}
    # 长时远程緩存12小时，主要用于要求时效要求一般的集中式缓存
    longTime:
      type: redis.lettuce
      expireAfterWriteInMillis: 43200000
      keyConvertor: fastjson
      valueEncoder: java
      valueDecoder: java
      poolConfig:
        minIdle: 5
        maxIdle: 20
        maxTotal: 50
      uri: redis://${REDIS_HOST:localhost}:${REDIS_PORT:6379}
    # 短時远程緩存5分钟，主要用于要求时效较高的集中式缓存
    shortTime:
      type: redis.lettuce
      expireAfterWriteInMillis: 300000
      keyConvertor: fastjson
      valueEncoder: java
      valueDecoder: java
      poolConfig:
        minIdle: 5
        maxIdle: 20
        maxTotal: 50
      uri: redis://${REDIS_HOST:localhost}:${REDIS_PORT:6379}
```

{docsify-updated}
