# 注册中心使用

## 简介

本例快速使用opensabre-starter-boot构建一个springboot新应用，默认集成统一异常、统一报文、文档等基础组件和规范。

项目地址：`https://github.com/opensabre/examples/sample`

## 前置

| 依赖软件        | 要求     | 备注                                                  |
| ------------- | -------- | ---------------------------------------------------- |
| java          | 11+      | 必须                                                  |

## 开发

### 1. 引入starter包


## 测试

```shell
root@xxxxx # curl http://localhost:8080/test/echo?name=zhangsan
{"code":"000000","mesg":"处理成功","time":"2022-11-22T14:46:58.826435Z","data":"Hello:zhangsan"}
```

## 文档

swagger文档地址：`http://localhost:8080/swagger-ui/index.html`