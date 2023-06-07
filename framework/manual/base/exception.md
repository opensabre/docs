# 统一异常处理

## 简介

<span>
在日常项目开发中，异常是常见的，如何更高效的处理好异常信息，统一对异常进行响应，这样能迅速定位BUG和原因，因此我们需要对这些异常进行统一的捕获和处理。SpringBoot中，@ControllerAdvice即可开启全局异常处理，我们只需在自定义一个方法使用@ExceptionHandler注解然后定义捕获异常的类型即可对这些捕获的异常进行统一的处理。

opensabre-framework默认内置了部分常见异常处理，返回对应的响应码，也可再对其进行主扩展。
</span>

## 前置

| 依赖软件                    | 要求     | 备注                                             |
| ------------------------- | -------- | ------------------------------------------------|
| java                      | 11+      | 必须                                             |
| opensabre-starter-boot    | 0.0.5    |                                                 |

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

### 内置异常及响应码

| 异常                                    | 响应码 | 响应信息       |     备注            |
|----------------------------------------|-------|---------------|--------------------|
| MissingServletRequestParameterException|020000 |请求参数校验不通过| 缺少请求参数等 |
| MethodArgumentNotValidException        |020000 |请求参数校验不通过| 参数不合法，参数Form表单校验不通过       |
| HttpMessageNotReadableException        |020000 |请求参数校验不通过| 报文内容解析或转换异常，输入数据格式不匹配 |
| MultipartException                     |020010 |上传文件大小超过限制| 上传文件大小超过限制 |
| BaseException               | -1 |系统异常| 未知的异常，统一返回-1 |
| Exception、Throwable        | -1 |系统异常| 未知的异常，统一返回-1 |

### 响应示例

```json
{
  "code": "020000",
  "mesg": "请求参数校验不通过",
  "time": "2023-02-26 17:37:40.964",
  "data": "密码为3~20字母数字"
}
```
