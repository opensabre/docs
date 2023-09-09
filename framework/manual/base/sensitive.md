# Rest报文敏感信息脱敏

## 简介

⽇常项⽬开发中，对于返回给前端的敏感数据，如身份证号、手机号、银行卡号等，通常会根据需求进行关键信息的隐藏。

opensabre-framework默认包含了数据脱敏的模块，开发者给通过注解方便的给返回的内容加上掩码，防止敏感信息的泄漏。

本例使用opensabre-starter-boot构建一个springboot应用，对rest返回的带有敏感信息的报文进行脱敏处理。

项目地址：`https://github.com/opensabre/examples/sample-boot`

示例文件：`https://github.com/opensabre/examples/sample-boot/src/java/io/github/opensabre/sample/rest/SensitiveController.java`

## 前置

| 依赖软件                    | 要求     | 备注                                             |
| ------------------------- | -------- | ------------------------------------------------|
| java                      | 11+      | 必须                                             |
| opensabre-starter-boot    | 0.0.7+   |                                                 |

## 开发

### 引入starter包

gradle依赖引入`implementation 'io.github.opensabre:opensabre-starter-boot:0.0.7'`

```groovy
dependencies {
    implementation 'io.github.opensabre:opensabre-starter-boot:0.0.7'
}
```

maven引入

```xml
<!-- opensabre boot starter -->
<dependency>
    <groupId>io.github.opensabre</groupId>
    <artifactId>opensabre-starter-boot</artifactId>
    <version>0.0.7</version>
</dependency>
```

### 使用方法

#### 1. VO类添加注解，标识脱敏类型

```java
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
@EqualsAndHashCode(callSuper = true)
public class UserVo extends BaseVo {
    private long userId;
    @Desensitization(type = MOBILE)
    private String mobile;
    @Desensitization(type = PHONE)
    private String phone;
    @Desensitization(type = NAME)
    private String name;
    @Desensitization(type = EMAIL)
    private String email;
    @Desensitization(type = ADDRESS)
    private String address;
    @Desensitization(type = PASSWORD)
    private String password;
    @Desensitization(type = CUSTOM, retainPrefixCount = 2, retainSuffixCount = 4)   // 自定义脱敏策略
    private String orderId;
}
```

#### 2. Controller将VO对象返回

```java
@Tag(name = "sensitive")
@ApiResponse(responseCode = "200", description = "处理成功", content = @Content(schema = @Schema(implementation = Result.class)))
@Slf4j
@RestController
@RequestMapping("/sensitive")
public class SensitiveController {

    @Operation(summary = "脱敏测试接口", description = "脱敏测试")
    @GetMapping("/user")
    public UserVo user() throws JsonProcessingException {
        UserVo userVo = UserVo.builder()
                .userId(1234567890)
                .name("张三一")
                .phone("0571-1234567")
                .mobile("13711220098")
                .email("abc123@123.com")
                .password("1qazXSW@")
                .orderId("A76883636636")
                .address("上海市浦东新区上丰路1234号1栋2509室")
                .build();
        log.info("userVo:{}", new ObjectMapper().writeValueAsString(userVo));
        log.info("userVo email:{}, mobile={} ,phone={}", userVo.getEmail(), userVo.getMobile(), userVo.getPhone());
        log.info("userVo password:{} , userVo passwd:{} key is {}", userVo.getPassword(), "1qaz@WSX", "key123");
        return userVo;
    }
}
```

测试

```shell
curl -X 'GET' 'http://localhost:8080/sensitive/user'

```

响应

```json
{
  "code": "000000",
  "mesg": "处理成功",
  "time": "2023-08-17T20:57:49.037Z",
  "data": {
    "id": null,
    "userId": 1234567890,
    "mobile": "137****0098",
    "phone": "057*****4567",
    "name": "张**",
    "email": "ab*****123.com",
    "address": "上海市***************509室",
    "password": "********",
    "orderId": "A7******6636"
  }
}
```

### 内置脱敏策略

|  标识         |  类型      |  默认规则    |  例子                         |
|--------------|------------|------------|------------------------------|
|  ACCOUNT_NO  |  用户账号类  |  前3后2    |  `guest123` -> `gue***23`        |
|  NAME        |  姓名       |  前1       |  `张三强` -> `张**`              |
|  ID_CARD     |  身份证号    |  前6后2    |  `31012320100430123X` -> `310123**********3X` |
|  BANK_CARD   |  银行卡号    |  前4后3    |  `622212345678119` -> `6222********119` |
|  CAR_LICENSE |  车牌号      |  前2后2    |  `沪A1234` -> `沪A**34`             |
|  EMAIL       |  电子邮件    |  前2后7    |  `admin@123.com` -> `ad****123.com` |
|  MOBILE      |  中国手机号   |  前3后4    | `13212345678` -> `123****5678`     |
|  PHONE       |  固定电话    |  前3后2    |  `057112345678` -> `057*******78`   |
|  PASSWORD    |  密码凭证    |  全*       |  `password123` -> `***********`     |
|  ADDRESS     |  中国地址    |  前3后4     |  `上海市浦东新区上车路5号102室` -> `上海市*********102室`  |

### 自定义脱敏策略

```java
@Desensitization(type = CUSTOM, retainPrefixCount = 2, retainSuffixCount = 4, replaceChar = "#")
```

- retainPrefixCount ： 前缀显示的字符数，默认为0

- retainSuffixCount ： 后缀显示的字符数，默认为0

- replaceChar ： 掩码字符，默认为`*`

### 脱敏规则扩展

待补充

## 文档

swagger文档地址：`http://localhost:8080/swagger-ui/index.html#/sensitive/user`