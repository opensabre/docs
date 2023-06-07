# 表单校验使用

## 简介

⽇常项⽬开发中，对于前端提交的表单，后台接⼝接收到表单数据后，通常会加⼊业务参数的合法校验操作来增加安全性。

springboot中使用spring-boot-starter-validation进行了数据校验的⼯作。

opensabre-framework默认包含了数据校验的模块并对其进行主扩展，基础的校验与springboot使用方法一样。


本例使用opensabre-starter-boot构建一个springboot应用，对rest提交表单进行校验。

项目地址：`https://github.com/opensabre/examples/sample-boot`

示例文件：`https://github.com/opensabre/examples/sample-boot/src/java/io/github/opensabre/sample/rest/ValidController.java`

## 前置

| 依赖软件                    | 要求     | 备注                                             |
| ------------------------- | -------- | -----------------------------------------------------------|
| java                      | 11+      | 必须                                             |
| opensabre-starter-boot    | 0.0.5    | opensabre-starter-boot默认引入spring-boot-starter-validation |

## 开发

### 引入starter包

gradle依赖引入`implementation 'io.github.opensabre:opensabre-starter-boot:0.0.5'`

```groovy
dependencies {
    implementation 'io.github.opensabre:opensabre-starter-boot:0.0.5'
}
```

maven引入

```xml
<!-- opensabre boot starter -->
<dependency>
    <groupId>io.github.opensabre</groupId>
    <artifactId>opensabre-starter-boot</artifactId>
    <version>0.0.5</version>
</dependency>
```

### 基础校验规则

#### 校验相关注解

| 注解           | 功能        |
|---------------|-------------|
|@AssertFalse   | 可以为null,如果不为null的话必须为false|
|@AssertTrue    | 可以为null,如果不为null的话必须为true |
|@DecimalMax    | 设置不能超过最⼤值 |
|@DecimalMin    | 设置不能超过最⼩值 |
|@Digits        | 设置必须是数字且数字整数的位数和⼩数的位数必须在指定范围内|
|@Future        | ⽇期必须在当前⽇期的未来|
|@Past          | ⽇期必须在当前⽇期的过去|
|@Max           | 最⼤不得超过此最⼤值|
|@Min           | 最⼤不得⼩于此最⼩值|
|@NotNull       | 不能为null，可以是空|
|@Size          | 集合、数组、map等的size()值必须在指定范围内|
|@Email         | 必须是email格式|
|@Length        | ⻓度必须在指定范围内|
|@NotBlank      | 字符串不能为null,字符串trim()后也不能等于“”|
|@NotEmpty      | 不能为null，集合、数组、map等size()不能为0；字符串trim()后可以等于“”|
|@Range         | 值必须在指定范围内|
|@URL           | 必须是⼀个URL|
|@Pattern       | 必须满⾜指定的正则表达式|

#### 表单校验示例

Form表单

```java
public class ValidForm extends BaseForm {

    @Schema(title = "用户ID")
    @NotNull(message = "id不能为空", groups = {Add.class, Save.class})
    private int id;

    @Schema(title = "用户名")
    @NotBlank(message = "⽤户不能为空!", groups = {Add.class, Save.class})
    private String name;

    @Schema(title = "用户密码")
    @NotBlank(message = "⽤户密码不能为空!", groups = Add.class)
    @Size(min = 3, max = 20, message = "密码为3~20字母数字", groups = Add.class)
    private String password;

    @Schema(title = "用户邮箱")
    @NotBlank(message = "⽤户邮箱不能为空!", groups = {Add.class, Save.class})
    @Email(message = "请输入正确的邮箱地址，如 abc123@qq.com", groups = {Add.class, Save.class})
    private String email;

    @Schema(title = "年龄")
    @Min(value = 18, message = "年龄不能小于18周岁", groups = {Add.class})
    @Max(value = 120, message = "年龄不能大于120周岁", groups = {Add.class})
    private int age;

    @Schema(title = "时间", example = "2023-02-22 14:56:10")
    @Past(groups = {Add.class, Save.class}, message = "时间格式为yyyy-MM-dd HH:mm:ss，并且不能为将来的时间")
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    @DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    private Date time;

    @Schema(title = "手机号1")
    @Pattern(regexp = "^1[3456789]\\d{9}$", groups = {Add.class}, message = "请输入正确的手机号")
    private String mobile1;

    @Schema(title = "类型")
    @EnumString(message = "类型只能为Last、Second、First", value = {"Last", "Second", "First"}, groups = {Add.class, Save.class})
    private String type;

    /**
     * 新增校验分组
     */
    public interface Add {
    }

    /**
     * 修改校验分组
     */
    public interface Save {
    }
}
```

Controller

```java
    @Operation(summary = "数据校验接口1", description = "Form表单校验")
    @PostMapping("/form")
    public String formAdd(@Parameter(description = "表单校验", required = true) @Validated(ValidForm.Add.class) @RequestBody ValidForm validForm) {
        log.info("form is {}", validForm.getName());
        return validForm.toString();
    }
```

测试

```shell
curl -X 'POST' \
  'http://localhost:8080/valid/form' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
  "username": "zhangsan",
  "id": 1234,
  "name": "abc",
  "password": "12",
  "email": "string@12.com",
  "age": 60,
  "time": "2023-02-26T09:17:08.529Z",
  "mobile": "15600002000"
}'

```

响应

```json
{
  "code": "020000",
  "mesg": "请求参数校验不通过",
  "time": "2023-02-26 17:37:40.964",
  "data": "密码为3~20字母数字"
}
```

### 扩展校验规则

#### 扩展校验相关注解

| 注解           | 功能     |
|---------------|----------|
|@Mobile        | 手机号校验 |
|@EnumString    | 枚举字符串校验，被校验字段值须包含在在value数组中 |

#### 枚举值

Form写法如下：

```java
    @Schema(title = "类型")
    @EnumString(message = "类型只能为Last、Second、First", value = {"Last", "Second", "First"}, groups = {Add.class, Save.class})
    private String type;
```

Controller写法同上，无变化

测试

```shell
curl -X 'POST' \
  'http://localhost:8080/valid/form' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
  "username": "zhangsan",
  "id": 1234,
  "name": "abc",
  "password": "123456",
  "email": "string@12.com",
  "age": 60,
  "time": "2023-02-26T09:17:08.529Z",
  "mobile": "15619841000",
  "type": "test"
}'
```

响应

```json
{
    "code":"000000",
    "mesg":"处理成功",
    "time":"2022-11-22 14:46:58.826",
    "data": "类型只能为Last、Second、First"
}
```

## 文档

swagger文档地址：`http://localhost:8080/swagger-ui/index.html#/valid/formAdd`