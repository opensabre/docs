# 表单校验使用

## 简介

本例使用opensabre-starter-boot构建一个springboot新应用，对rest提交表单进行校验。

项目地址：`https://github.com/opensabre/examples/sample-boot`

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