# 配置项加密

## 简介

为了安全的需要，一些重要的信息比如数据库密码不能明文保存在配置文件中，需要进行加密之后再保存。

框架引入了`jasypt-spring-boot-starter`组件来为配置属性提供加密的支持。

本例使用opensabre-starter-boot构建一个springboot应用，对其中的配置项进行加密保存，应用在运行时可取到明文信息进行使用。


项目地址：`https://github.com/opensabre/examples/sample-boot`

示例文件：`https://github.com/opensabre/examples/sample-boot/src/java/io/github/opensabre/sample/rest/EncryptConfigController.java`

## 前置

| 依赖软件                    | 要求     | 备注                                             |
| ------------------------- | -------- | ------------------------------------------------|
| java                      | 11+      | 必须                                             |
| opensabre-starter-boot    | 0.0.7    |                                                 |

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

### 使用教程

#### 配置使用

##### 1. 配置好应用的加密密钥

```yaml
# 配置项加密密钥默认配置
jasypt:
  encryptor:
    password: Pa55w0rd@opensabre

```

##### 2. 将明文信息加密后的密文存放在配置中文件，如下格式

```properties
test.password=ENC(3iIE/7wbUTBB0g9WuUHIFbduqiLHafNVnB+Oo+SP6uFGntypXzyZsd1SPlwJchgv)
```

##### 3. 程序中通过@Value可直接获取到配置的明文，进行使用

```java
@Tag(name = "config")
@ApiResponse(responseCode = "200", description = "处理成功", content = @Content(schema = @Schema(implementation = Result.class)))
@Slf4j
@RestController
@RequestMapping("/config")
public class EncryptConfigController {

    @Value("${test.password}")
    private String passwd;

    @Resource
    private StringEncryptor stringEncryptor;

    @Operation(summary = "配置项加密验证", description = "配置项加密")
    @GetMapping("/get")
    public String config() {
        log.info("plain:{}", passwd);
        return passwd;
    }
}
```

测试

```shell
curl -X 'GET' 'http://localhost:8080/config/get'

```

响应

```json
{
  "code": "000000",
  "mesg": "请求成功",
  "time": "2023-02-26 17:37:40.964",
  "data": "123456"
}
```

### 密文生成

#### 1. 通过调用API进行加密、解密

```java
    public static void main(String[] args) {
        String username = encrypt("root");
        String password = encrypt("123456");
        System.out.println(username);
        System.out.println(password);
    }

    /**
     * 加密
     * @param plaintext 明文密码     
     * @return
     */
    public static String encrypt(String plaintext) {
        //加密工具
        StandardPBEStringEncryptor encryptor = new StandardPBEStringEncryptor();
        //加密配置
        EnvironmentStringPBEConfig config = new EnvironmentStringPBEConfig();
        // 算法类型
        config.setAlgorithm("PBEWithMD5AndDES");
        //生成秘钥的公钥
        config.setPassword("PEB123@321BEP");
        //应用配置
        encryptor.setConfig(config);
        //加密
        String ciphertext = encryptor.encrypt(plaintext);
        return ciphertext;
    }

    /**
     * 解密
     *
     * @param ciphertext 待解密秘钥
     * @return
     */
    public static String decrypt(String ciphertext) {
        //加密工具
        StandardPBEStringEncryptor encryptor = new StandardPBEStringEncryptor();
        //加密配置
        EnvironmentStringPBEConfig config = new EnvironmentStringPBEConfig();
        config.setAlgorithm("PBEWithMD5AndDES");
        //生成秘钥的公钥
        config.setPassword("PEB123@321BEP");
        //应用配置
        encryptor.setConfig(config);
        //解密
        String pText = encryptor.decrypt(ciphertext);
        return pText;
    }
```

#### 2. 通过命令行调用

```shell
// 加密命令
java -cp jasypt-1.9.3.jar org.jasypt.intf.cli.JasyptPBEStringEncryptionCLI input='root' password=abcdef algorithm=PBEWithMD5AndDES

// 解密命令
java -cp jasypt-1.9.3.jar org.jasypt.intf.cli.JasyptPBEStringDecryptionCLI input='z4xP29fuY4wF2AJqp1NnoGJxj' password=abcdef algorithm=PBEWithMD5AndDES
```

#### 3. 通过在应用中引入maven插件

```
// 1、在Pom中添加maven插件依赖
<plugin>
    <groupId>com.github.ulisesbocchio</groupId>
    <artifactId>jasypt-maven-plugin</artifactId>
    <version>3.0.4</version>
</plugin>
```

```shell
// 2、加密命令
mvn jasypt:encrypt-value -Djasypt.encryptor.password="秘钥的值" -Djasypt.plugin.value="需要加密的敏感信息"

// 解密命令
mvn jasypt:decrypt-value -Djasypt.encryptor.password="秘钥的值" -Djasypt.plugin.value="需要解密的密文"
```

## 文档

swagger文档地址：`http://localhost:8080/swagger-ui/index.html#/config/get`