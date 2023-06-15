# 快速入门

## 简介

本例快速使用opensabre-starter-boot构建一个springboot新应用，默认集成统一异常、统一报文、文档等基础组件和规范。

项目地址：`https://github.com/opensabre/examples/sample-boot`

## 前置

| 依赖软件        | 要求     | 备注                                                  |
| ------------- | -------- | ---------------------------------------------------- |
| java          | 11+      | 必须                                                  |

## 开发

### 1. 引入starter包

gradle依赖引入`implementation 'io.github.opensabre:opensabre-starter-boot:0.0.6'`

```gradle
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
    implementation 'io.github.opensabre:opensabre-starter-boot:0.0.6'
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
	<artifactId>opensabre-starter-boot</artifactId>
	<version>0.0.6</version>
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

    @Operation(summary = "测试接口1", description = "hello xxx")
    @GetMapping("/echo")
    public String echo(@RequestParam String name) {
        return "Hello:" + name;
    }
}
```

## 测试

```shell
root@xxxxx # curl http://localhost:8080/test/echo?name=zhangsan

{   
    "code":"000000",
    "mesg":"处理成功",
    "time":"2022-11-22T14:46:58.643Z",
    "data":"Hello:zhangsan"
}
```

## 文档

swagger文档地址：`http://localhost:8080/swagger-ui/index.html`
