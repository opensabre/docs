# 快速入门

## 简介

​		本例快速使用opensabre starter构建一个基础应用。

## 前置

| 依赖软件      | 要求     | 备注                                                    |
| ------------- | -------- | ------------------------------------------------------- |
| java          | 11+      | 必须                                                    |
| mq            | rabbitmq | 默认账号密码：guest/guest，如没有，启动报错，不影响使用 |
| 注册/配置中心 | nacos    | 非必须，如没有，启动报错，不影响使用                    |

## 开发

### 1. 引入starter包

gradle依赖引入`implementation 'io.github.opensabre:opensabre-base-starter:0.0.3'`

```ASN.1
plugins {
    id 'java'
    id 'org.springframework.boot' version '2.7.5'
    id 'io.spring.dependency-management' version '1.0.15.RELEASE'
}

group = 'io.github.opensabre'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '11'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'io.github.opensabre:opensabre-base-starter:0.0.3'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

maven引入

```xml
<!-- opensabre starter -->
<dependency>
	<groupId>io.github.opensabre</groupId>
	<artifactId>opensabre-base-starter</artifactId>
	<version>0.0.3</version>
</dependency>
```

### 2. 增加配置项

配置文件`application.properties`中设置端口和应用名配置项

```
server.port=8080                    #应用端口
spring.application.name=base-sample #应用名
```

### 3. 添加RestController

```java
package io.github.opensabre.sample.rest;

import io.github.opensabre.common.core.entity.vo.Result;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.tags.Tag;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/test")
@Tag(name = "test")
@Slf4j
public class HelloController {

    @Operation(summary = "测试接口", description = "hello xxx")
    @GetMapping("/hello")
    public Result hello(@RequestParam String name) {
        return Result.success("Hello:" + name);
    }
}
```

## 测试

```shell
root@xxxxx # curl http://localhost:8080/test/hello?name=zhangsan
{"code":"000000","mesg":"处理成功","time":"2022-11-22T14:46:58.826435Z","data":"Hello:zhangsan"}
```

## 文档

swagger文档地址：`http://localhost:8080/swagger-ui/index.html`

项目地址：`https://github.com/opensabre/examples/sample`

