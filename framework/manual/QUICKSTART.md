# 快速入门

## 简介

本例快速使用 `opensabre-starter-boot` 和 `opensabre-starter-webmvc` 构建一个 Spring Boot WebMVC 新应用，默认集成统一异常、统一报文、文档等基础组件和规范。

项目地址：`https://github.com/opensabre/examples/sample-boot`

## 前置

| 依赖软件        | 要求     | 备注                                                  |
| ------------- | -------- | ---------------------------------------------------- |
| java          | 17+      | 必须                                                  |

## 开发

### 1. 引入 starter 包

WebMVC 项目需要引入 `opensabre-starter-boot` 和 `opensabre-starter-webmvc`。`opensabre-starter-webmvc` 已包含 `spring-boot-starter-web`，默认使用 Undertow 容器。

<!-- tabs:start -->

#### **maven**

```xml
<dependency>
    <groupId>io.github.opensabre</groupId>
    <artifactId>opensabre-starter-boot</artifactId>
    <version>0.7.1</version>
</dependency>
<dependency>
    <groupId>io.github.opensabre</groupId>
    <artifactId>opensabre-starter-webmvc</artifactId>
    <version>0.7.1</version>
</dependency>
```

#### **gradle**

```groovy
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.4.1'
    id 'io.spring.dependency-management' version '1.1.7'
}

group = 'io.github.opensabre'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '17'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'io.github.opensabre:opensabre-starter-boot:0.7.1'
    implementation 'io.github.opensabre:opensabre-starter-webmvc:0.7.1'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

<!-- tabs:end -->

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

knife4j文档地址：`http://localhost:8080/doc.html`

## 可选治理能力

如果业务应用需要审计日志或限次能力，引入 governance starter：

```groovy
implementation 'io.github.opensabre:opensabre-starter-governance:0.7.1'
```

然后在业务方法上使用 `@Audit` 或 `@RateLimit`，starter 会通过 sysadmin 的接口统一处理审计入库和限次检查。
