# 日志打印与追踪

## 简介

系统运行过程中，打印的日志可能涉及到敏感信息，如银行卡，姓名，身份证等等，通常会根据需求进行关键词的隐藏，防止信息泄漏。

比如身份证号、姓名、银行卡等，程序会掩码处理后在日志中打印，如` 张三 -> 张* `这样的过程叫做日志关键词脱敏。

框架实现了常见的敏感数据的日志脱敏，可通过一些简单的配置完成日志中敏感数据的脱敏输出。

本例使用opensabre-starter-boot构建一个springboot应用，对其中的配置项进行加密保存，应用在运行时可取到明文信息进行使用。

</br>

项目地址：`https://github.com/opensabre/examples/sample-boot`

示例文件：`https://github.com/opensabre/examples/sample-boot/src/java/io/github/opensabre/sample/rest/EncryptConfigController.java`

## 前置

| 依赖软件                    | 要求     | 备注                                             |
| ------------------------- | -------- | ------------------------------------------------|
| java                      | 17+      | 必须                                             |
| opensabre-starter-boot    | 0.4.0    |                                                 |

## 开发

### 引入starter包

引入依赖 `opensabre-starter-boot` 

<!-- tabs:start -->

#### **maven**

```xml
<!-- opensabre boot starter -->
<dependency>
    <groupId>io.github.opensabre</groupId>
    <artifactId>opensabre-starter-boot</artifactId>
    <version>0.4.0</version>
</dependency>
```

#### **gradle**

```groovy
dependencies {
    implementation 'io.github.opensabre:opensabre-starter-boot:0.4.0'
}
```

<!-- tabs:end -->

### 配置项

#### 正则匹配

输出的信息有固定模式可识别，如固定电话、身份证号、手机号、email、地址、车牌、银行卡号等，框架已提供相应的正则，可通过简单配置识别日志中的信息，并进行脱敏屏蔽。

```yaml
# opensabre framework配置
opensabre:
  sensitive:
    log:
      enabled: true # 日志脱敏开关，默认为关闭
      rules: mobile,idCard,phone,address  # 默认配置mobile,idCard,phone，可支持 mobile,idCard,phone,email,address,carLicense,bankCard选项

```

#### 固定格式匹配

诸如密码、姓名类的信息无法简单通过正则匹配，但在日志输出时会在信息前给出提示信息，如 passwrod: XXXXXXX、姓名：张三 等样式，通过配置bean实例化脱敏器，便可进行信息脱敏。

支持 `[password|pass|passwd|secret|key|credential|token][[is|:|：|=|<|>]123456`形式的密码信息脱敏。

> [!tip|label:提示]
> </br>
> 打开`opensabre.sensitive.log.enabled`开关后， `PasswordLogBackDesensitizer`密码脱敏器默认会加载，防止密码信息打印控制台。

姓名脱敏器，可通过实例化`NameLogBackDesensitizer`类进行配置，支持 `[name|姓名|][is|:|：|=|<|>]张三李四` 形式的信息脱敏。

```java
@Configuration
public class SensitiveConfig {

    @Bean
    LogBackDesensitizer nameLogBackDesensitizer() {
        return new NameLogBackDesensitizer();
    }
}
```

### 输出日志

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
        log.info("name is 张四一，姓名：李四六，住址：{}", "中国台湾省台北市新竹县桃源村5号天下花园1幢103室");
        return userVo;
    }
}
```

测试

```shell
curl -X 'GET' 'http://localhost:8080/sensitive/user'

```

查看日志

 1. 日志中匹配手机号、固定电话、身份证号格式的信息均按默认规则进行报脱敏处理。
 
 2. 日志中匹配 `[password|pass|passwd|secret|key|credential|token][：=<>:]` 字符串的均被认为是凭证，进行了掩码处理。

具体脱敏的规则可查看 [脱敏规则](/framework/manual/base/sensitive?id=内置脱敏策略)

```java
2023-09-09 12:57:10.332  INFO [,c5cacaa69bc09055,c5cacaa69bc09055] 72106 --- [  XNIO-1 task-1] c.o.sample.rest.SensitiveController      
: userVo:{"id":null,"userId":1234567890,"mobile":"137****0098","phone":"057*****4567",
"name":"张**","email":"ab*****123.com","address":"上海市***************509室",
"password":"********","orderId":"A7******6636"}
2023-09-09 12:57:10.335  INFO [,c5cacaa69bc09055,c5cacaa69bc09055] 72106 --- [  XNIO-1 task-1] c.o.sample.rest.SensitiveController      : userVo email:abc123@123.com, mobile=137****0098 ,phone=057*******67
2023-09-09 12:57:10.341  INFO [,c5cacaa69bc09055,c5cacaa69bc09055] 72106 --- [  XNIO-1 task-1] c.o.sample.rest.SensitiveController      : userVo password:******** , userVo passwd:******** key is ******
2023-09-13 00:09:53.387  INFO [,1dac9417bc287cc1,1dac9417bc287cc1] 90603 --- [  XNIO-1 task-1] c.o.sample.rest.SensitiveController      : name is 张**，姓名：李**，住址：中国台湾省台北市新竹县桃源村5号天下花园1幢103室
```

## 文档

swagger文档地址：`http://localhost:8080/swagger-ui/index.html#/sensitive/user`
