# 工程简介

## 工程地址

Github地址：https://github.com/opensabre/opensabre-framework

Gitee地址：https://gitee.com/opensabre/opensabre-framework

## 子工程介绍

| module名称                    | 子目录                           | 主要功能及用途                                                 |
| -----------------------------| ------------------------------- | ------------------------------------------------------------ |
| opensabre-framework          | opensabre-framework             |  opensabre服务框架父工程，定义通用配置，如打包、仓库、发布等各模块配置  |
| opensabre-base-dependencies  | opensabre-base-dependencies     |  opensabre服务框架工程的依赖管理，如版本、配置等定义  |
| opensabre-starter-register   | opensabre-starter-register      |  注册中心相关starter模块，用于封装简化注册中心使用，约定使用规范等|
| opensabre-starter-config     | opensabre-starter-config        |  配置中心相关starter模块，用于封装简化配置中心使用，约定使用规范等|
| opensabre-starter-boot       | opensabre-starter-boot          |  springboot web相关starter模块，用于封装统一异常处理、统一报文、swagger文档等相关规范约定等|
| opensabre-starter-rpc        | opensabre-starter-rpc           |  远程调用相关starter模块，用于封装应用间相互调用、路由、超时等通用配置模块|
| opensabre-starter-cache      | opensabre-starter-cache         |  缓存相关starter模块，用于封装多级缓存，如内存、redis等缓存的规范和约定     |
| opensabre-starter-persistence| opensabre-starter-persistence   |  数据持久化相关starter模块，用于封装数据库操作、实体转换等数据持久化规范和约定|
| opensabre-starter-eda        | opensabre-starter-eda           |  事件驱动starter模块，用于封装和规范消息、事件等操作|
| opensabre-test               | opensabre-test                  |  通用测试工具包|
| opensabre-web                | opensabre-web                   |  web操作通用工具、配置等|
