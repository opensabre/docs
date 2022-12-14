# 开发规范

## 数据库设计规范

### 表设计规范

1、表名全部小写，单词间通过'_'间隔

2、主键命名为'id'，varchar(20)，不使用数据库的序列，应用生成全局序列。

3、必须包含4个审计字段且不能为空。created_time、updated_time、created_by、updated_by。

4、关键词要求大写，使用IDE如idea进行格式化

5、常量枚举全部用大写

### 外键及索引命名规范

1、唯一索引：ux_表名_索引字段。如：ux_resource_code

2、普通索引：ix_表名_索引字段。如：ix_role_name

3、外键命名：fk_表名_字段名。如：fk_orders_product_id

### 字段长度规则

| 名称类   | 类型     | 长度 | 备注               |
| -------- | -------- | ---- | ------------------ |
| 编码类   | varchar  | 100  |                    |
| 账号类   | varchar  | 100  | 如email，username  |
| 状态类   | varchar  | 5    | 如订单状态等       |
| 名称类   | varchar  | 200  | 中文名称，如产品名 |
| 手机电话 | varchar  | 20   |                    |
| 描述简介 | varchar  | 500  |                    |
| 网址类   | varchar  | 500  | 如url              |
| 时间类   | datetime |      |                    |


## URL和方法命名规范

### RESTFUL URL命名规范

API URI design
API URI 设计最重要的一个原则： nouns (not verbs!) ，名词（而不是动词）。

CRUD 简单 URI：

| 方法   | URL     | 功能                |
| ------ | ------- | ------------------- |
| GET    | /user   | 获取用户列表        |
| GET    | /user/1 | 获取 id 为 1 的用户 |
| POST   | /user   | 创建一个用户        |
| PUT    | /user/1 | 替换 id 为 1 的用户 |
| PATCH  | /user/1 | 修改 id 为 1 的用户 |
| DELETE | /user/1 | 删除 id 为 1 的用户 |

上面是对某一种资源进行操作的 URI，那如果是有关联的资源，或者称为级联的资源，该如何设计 URI 呢？比如某一用户下的产品：

| 方法   | URL               | 功能                                   |
| ------ | ----------------- | -------------------------------------- |
| GET    | /user/1/product   | 获取 Id 为 1 用户下的产品列表          |
| GET    | /user/1/product/2 | 获取 Id 为 1 用户下 Id 为 2 的产品     |
| POST   | /user/1/product   | 在 Id 为 1 用户下，创建一个产品        |
| PUT    | /user/1/product/2 | 在 Id 为 1 用户下，替换 Id 为 2 的产品 |
| PATCH  | /user/1/product/2 | 修改 Id 为 1 的用户下 Id 为 2 的产品   |
| DELETE | /user/1/product/2 | 删除 Id 为 1 的用户下 Id 为 2 的产品   |

### 方法命名规范

#### Mapper

简单的CRUD请按如下规则命名

| 操作 | 例子       | 备注 |
| ---- | ---------- | ---- |
| 增加 | insert/add |      |
| 删除 | delete     |      |
| 修改 | update     |      |
| 查询 | query      |      |
| 搜索 | search     |      |

#### Service

简单的CRUD请按如下规则命名，其它操作请按业务动作命名，使用动词

| 操作 | 例子          | 备注                   |
| ---- | ------------- | ---------------------- |
| 增加 | add           |                        |
| 获取 | get           | 获取到单条记录         |
| 删除 | remove/delete |                        |
| 更新 | update        | 更新存在的记录         |
| 保存 | save          | 更新，不存在则新增     |
| 查询 | query         | 根据id等简单条件查询   |
| 搜索 | search        | 根据时间范围或模糊搜索 |

#### Rest

简单的CRUD请按如下规则命名，其它操作请按业务动作命名，使用动词

| 操作 | 例子          | 备注                   |
| ---- | ------------- | ---------------------- |
| 增加 | add           |                        |
| 保存 | save          | 更新，不存在则新增     |
| 删除 | remove/delete |                        |
| 获取 | get           | 获取到单条记录         |
| 更新 | update        | 更新存在的记录         |
| 查询 | query         | 根据id等简单条件查询   |
| 搜索 | search        | 根据时间范围或模糊搜索 |