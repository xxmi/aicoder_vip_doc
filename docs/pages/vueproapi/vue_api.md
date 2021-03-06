# 宝洁SFA后台接口说明文档

## 综述

只有登录相关的接口不需要添加认证的token,其余所有的接口请求都必须在请求头中添加 `Authorization`.

所有的请求对应模型都默认提供了: 添加、修改、查询、复杂查询和删除操作的api。

### 接口统一地址模型

接口地址符合`RestFul`风格。

+ 添加

| 标题  | 说明               | 备注                                        |
|-----|------------------|-------------------------------------------|
| 地址  | `POST /api/模型名字` | `header`中必须添加 `Authrization`对应的jwt的token. |

例如:

```js
$.ajax({
  "async": true,
  "crossDomain": true,
  "url": "http://localhost:3002/api/user",
  "method": "POST",
  "headers": {
    "Authorization": "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1MzcxODU3OTk5NzgsIm5hbWUiOiJzZGZmZmYifQ.WLVdU-GESQR6kJfUdLCBpNWUZMGAW6VsTk6lfAXC1xM"
  },
  "data": {
    "Name": "laomsds",
    "Passwd": "aaafc44445555",
    "isDel": "false"
  }
}).done(function (response) {
  console.log(response);
});
```

+ 修改

| 标题  | 说明                  | 备注                                        |
|-----|---------------------|-------------------------------------------|
| 地址  | `PUT /api/模型名字/:id` | `header`中必须添加 `Authrization`对应的jwt的token. |
支持`PATCH协议`

```js
$.ajax({
  "async": true,
  "crossDomain": true,
  "url": "http://%E4%BF%AE%E6%94%B9",
  "method": "PUT",
  "headers": {
    "Authorization": "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1MzcxODU3OTk5NzgsIm5hbWUiOiJzZGZmZmYifQ.WLVdU-GESQR6kJfUdLCBpNWUZMGAW6VsTk6lfAXC1xM",
    "Content-Type": "application/x-www-form-urlencoded"
  },
  "data": {
    "Name": "laoma2333"
  }
}).done(function (response) {
  console.log(response);
});
```

+ 删除

| 标题  | 说明                     | 备注                                        |
|-----|------------------------|-------------------------------------------|
| 地址  | `DELETE /api/模型名字/:id` | `header`中必须添加 `Authrization`对应的jwt的token. |

```js
$.ajax({
  "async": true,
  "crossDomain": true,
  "url": "http://localhost:3002/api/user/5b9621e7cad9bf1c60e1d51f",
  "method": "DELETE",
  "headers": {
    "Authorization": "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1MzcxODU3OTk5NzgsIm5hbWUiOiJzZGZmZmYifQ.WLVdU-GESQR6kJfUdLCBpNWUZMGAW6VsTk6lfAXC1xM"
  }
}).done(function (response) {
  console.log(response);
});
```

+ 查询id

| 标题  | 说明                  | 备注                                        |
|-----|---------------------|-------------------------------------------|
| 地址  | `GET /api/模型名字/:id` | `header`中必须添加 `Authrization`对应的jwt的token. |

```js
$.ajax({
  "async": true,
  "crossDomain": true,
  "url": "http://localhost:3002/api/user/5b9621e7cad9bf1c60e1d51f",
  "method": "GET",
  "headers": {
    "Authorization": "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1MzcxODU3OTk5NzgsIm5hbWUiOiJzZGZmZmYifQ.WLVdU-GESQR6kJfUdLCBpNWUZMGAW6VsTk6lfAXC1xM"
  }
}).done(function (response) {
  console.log(response);
});
```

### 复合查询

| 标题     | 说明                                                                                             |
|--------|------------------------------------------------------------------------------------------------|
| 地址     | `GET /api/模型名字`                                                                                |
| 注意事项   | `header`中必须添加 `Authrization`对应的jwt的token.                                                      |
| 分页     | 当前页请求参数中,添加 `page`, 默认不分页.一页大小,请在请求参数中添加`limit`或`pageSize`. 例如:   `page=9&limit=10`,一页10条,第9页. |
| 排序     | 升序: `sort_asc=排序字段名`, 降序:`sort_desc=排序字段名`                                                     |
| 等于过滤   | `字段名_eq=等于的值`, 查询某个字段必须等于什么...                                                                 |
| 模糊查询过滤 | `字段名_like=模糊查询的值`                                                                              |

```js
$.ajax({
  "async": true,
  "crossDomain": true,
  "url": "http://localhost:3002/api/user?sort_asc=Passwd&limit=5&page=1",
  "method": "GET",
  "headers": {
    "Authorization": "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1MzcxODU3OTk5NzgsIm5hbWUiOiJzZGZmZmYifQ.WLVdU-GESQR6kJfUdLCBpNWUZMGAW6VsTk6lfAXC1xM"
  }
}).done(function (response) {
  console.log(response);
});
```

## 登陆相关

### 登陆获取token

用户登录WebApp时，首先对用户进行鉴权。

| 类型   | 说明                                                   |
|------|------------------------------------------------------|
| 接口地址 | `http://域名/api/login`  <br>例如：`http://aicoder.com/api/login` |
| 请求方式 | `POST`                                               |
| 数据类型 | `application/json`                                   |
| 特殊要求 | 后台限制同一指纹浏览器,在1分钟内只能请求5次,超过次数认为是攻击,则禁止登录.             |

#### 请求参数

| 序号  | 字段     | 类型     | 性质  | 说明   |
|-----|--------|--------|-----|------|
| 1   | CNO    | String | 必填  | 公司编号 |
| 2   | Passwd | String | 必填  | 密 码   |
| 3   | PNO    | String | 必填  | 员工编号 |

#### 返回值

| 序号  | 字段    | 类型     | 说明                |
|-----|-------|--------|-------------------|
| 1   | user  | Object | 登陆成功的用户对象信息       |
| 2   | code  | Number | 登陆成功的编码，1成功， 0失败。 |
| 3   | token | String | token密钥。          |
| 4   | msg   | String | 消息内容。             |

> 登录成功后续请求都需要添加token密钥到header的Authorization中。

用户对象类型

| 属性            | 类型       | 参考值                      | 说明     |
|---------------|----------|--------------------------|--------|
| SubTitle      | String   | 区社就没状几行重马定展标里技代。         | 个人心情标题 |
| isDel         | Boolean  | false                    | 是否删除   |
| _id           | ObjectID | 5bc40eaa53918a0e7096167f | 主键     |
| Name          | String   | vyk                      | 用户名    |
| PNO           | String   | 80004                    | 员工编号   |
| Passwd        | String   | 123123                   | 密码     |
| CNO           | String   | 6666                     | 公司编号   |
| Avatar        | String   | /a/b.png                 | 头像     |
| CName         | String   | 郑霞                       | 中文名    |
| Phone         | String   | 1555511215151            | 电话     |
| LastLoginDate | Date     | 2018-10-15               | 最后登录日期 |
| Department    | ObjectID | 5bc40eaa53918a0e7096166d | 部门id   |

#### 返回实例

```js
// 登录成功消息
{
    "user": {
        "SubTitle": "区社就没状几行重马定展标里技代。",
        "isDel": false,
        "_id": "5bc40eaa53918a0e7096167f",
        "Name": "vyk",
        "PNO": "80004",
        "Passwd": "123123",
        "CNO": "6666",
        "Avatar": "",
        "CName": "郑霞",
        "Phone": "",
        "LastLoginDate": "2018-10-15T03:51:06.000Z",
        "Department": "5bc40eaa53918a0e7096166d",
    },
    "code": 1,
    "msg": "登录成功",
    "token": "eyJ0eXAiOiJKV1..."
}

// 登录失败消息
{
  code: 0,
  msg: '登录验证失败'
}
```

## 用户接口相关

### 用户ID查询

| 类型   | 说明                                                      |
|------|---------------------------------------------------------|
| 接口地址 | `http://域名/api/user/:id`<br>例如：`http://aicoder.com/api/user/29` |
| 请求方式 | `GET`                                                   |
| 数据类型 | `application/json`                                      |

#### 请求参数

| 序号  | 字段    | 类型     | 性质  | 说明                                        |
|-----|-------|--------|-----|-------------------------------------------|
| 1   | id    | String | 必填  | 这个是路由参数，必须放入地址中                           |
| 2   | token | String | 必填  | 这个是登陆后，用户的登陆token。数据请放到请求的`Authorization` |

#### 返回值

用户对象类型

| 属性            | 类型       | 参考值                      | 说明     |
|---------------|----------|--------------------------|--------|
| SubTitle      | String   | 区社就没状几行重马定展标里技代。         | 个人心情标题 |
| isDel         | Boolean  | false                    | 是否删除   |
| _id           | ObjectID | 5bc40eaa53918a0e7096167f | 主键     |
| Name          | String   | vyk                      | 用户名    |
| PNO           | String   | 80004                    | 员工编号   |
| Passwd        | String   | 123123                   | 密码     |
| CNO           | String   | 6666                     | 公司编号   |
| Avatar        | String   | /a/b.png                 | 头像     |
| CName         | String   | 郑霞                       | 中文名    |
| Phone         | String   | 1555511215151            | 电话     |
| LastLoginDate | Date     | 2018-10-15               | 最后登录日期 |
| Department    | ObjectID | 5bc40eaa53918a0e7096166d | 部门id   |

#### 返回实例

```js
{
  "SubTitle": "区社就没状几行重马定展标里技代。",
  "isDel": false,
  "_id": "5bc40eaa53918a0e7096167f",
  "Name": "vyk",
  "PNO": "80004",
  "Passwd": "123123",
  "CNO": "6666",
  "Avatar": "",
  "CName": "郑霞",
  "Phone": "",
  "LastLoginDate": "2018-10-15T03:51:06.000Z",
  "Department": "5bc40eaa53918a0e7096166d",
}
```

## 公告接口

### 获取公司公告

| 类型   | 说明                                                      |
|------|---------------------------------------------------------|
| 接口地址 | `http://域名/api/message`<br>例如：`http://aicoder.com/api/message` |
| 请求方式 | `GET`                                                   |
| 数据类型 | `application/json`                                      |

#### 请求参数

| 序号  | 字段    | 类型     | 性质  | 说明                                        |
|-----|-------|--------|-----|-------------------------------------------|
| 1   | token | String | 必填  | 这个是登陆后，用户的登陆token。数据请放到请求的`Authorization` |

#### 返回值

接口返回值为:公司通告数据对象的集合.

公司通告对象类型:

| 属性            | 类型       | 参考值                      | 说明     |
|---------------|----------|--------------------------|--------|
| title      | String   | 标题文字         | 通告标题 |
| isDel         | Boolean  | false                    | 是否删除   |
| _id           | ObjectID | 610000200212299156 | 主键     |
| main          | String   | 通告主体                      | 通告主体    |
| SubDate | Date     | 2018-10-15               | 发布日期 |
| isReaded    | Boolean |false |   是否已读  |
| level    | number | 1 | 紧急级别   |

#### 返回实例

```js
[
  {
    "title": "又特共委压具",
    "isDel": false,
    "_id": "610000200212299156",
    "main": "快打角划能不南太很状体名生农。府运组间派装任总式术层因重和土。深系义知见革身将往质状然海革济光。装低和内即位验知及天象志话气始。",
    "level": "magna Duis esse",
    "isReaded": true,
    "SubDate": "id adipisicing"
  },
  {
    "title": "的被风带维革",
    "isDel": true,
    "_id": "12000019741128014X",
    "main": "个发道党各儿代土每声精还度。长西场前水再变造最它收共将。无气来眼共候音月员变感理长龙。着热却战路收来组公重装毛群统加问。",
    "level": "sunt anim amet",
    "isReaded": true,
    "SubDate": "occaecat tempor exercitation non"
  },
  ...
]
```

### 阅读公司公告

| 类型   | 说明                                                      |
|------|---------------------------------------------------------|
| 接口地址 | `http://域名/api/message/:id`<br>例如：`http://aicoder.com/api/message/2222` |
| 请求方式 | `POST`                                                   |
| 数据类型 | `application/json`                                      |

#### 请求参数

| 序号  | 字段    | 类型     | 性质  | 说明                                        |
|-----|-------|--------|-----|-------------------------------------------|
| 1   | token | String | 必填  | 这个是登陆后，用户的登陆token。数据请放到请求的`Authorization` |
| 2   | id | String | 必填  | 消息的id |

#### 返回值

| 序号  | 字段    | 类型     | 说明                |
|-----|-------|--------|-------------------|
| 2   | code  | Number | 登陆成功的编码，1成功， 0失败。 |
| 4   | msg   | String | 消息内容。             |

#### 返回实例

```js
{
  "code": 1,
  "msg": "success"
}
```

## 订单相关接口

### 获取用户的销售额度

| 类型   | 说明                                                      |
|------|---------------------------------------------------------|
| 接口地址 | `http://域名//api/getUserProgress` |
| 请求方式 | `GET`                                                   |
| 数据类型 | `application/json`                                      |

#### 请求参数

| 序号  | 字段    | 类型     | 性质  | 说明                                        |
|-----|-------|--------|-----|-------------------------------------------|
| 1   | token | String | 必填  | 这个是登陆后，用户的登陆token。数据请放到请求的`Authorization` |

#### 返回值

用户对象类型

| 属性            | 类型       | 参考值                      | 说明     |
|---------------|----------|--------------------------|--------|
| totalShops           | Number   | 10                     | 有效商店数   |
| monthPercent| Number | 0.35 | 销售进度,0-1之间的一个值  |

#### 返回实例

```js
{
  "monthPercent": 0.89,
  "totalShops": 838
}
```

请求实例

```js
$.ajax({
  "async": true,
  "crossDomain": true,
  "url": "http://localhost:3002/api/getUserProgress",
  "method": "GET",
  "headers": {
    "Authorization": "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1MzcxODU3OTk5NzgsIm5hbWUiOiJzZGZmZmYifQ.WLVdU-GESQR6kJfUdLCBpNWUZMGAW6VsTk6lfAXC1xM"
  }
}).done(function (response) {
  console.log(response);
});
```

### 获取店铺订单

| 类型   | 说明                                                      |
|------|---------------------------------------------------------|
| 接口地址 | `http://域名//api/showOrders/:shopId` |
| 请求方式 | `GET`                                                   |
| 数据类型 | `application/json`                                      |

#### 请求参数

| 序号  | 字段    | 类型     | 性质  | 说明                                        |
|-----|-------|--------|-----|-------------------------------------------|
| 1   | shopId    | String | 必填  | 这个是路由参数，必须放入地址中,商店的id                          |
| 2   | token | String | 必填  | 这个是登陆后，用户的登陆token。数据请放到请求的`Authorization` |

#### 返回值

用户对象类型

| 属性            | 类型       | 参考值                      | 说明     |
|---------------|----------|--------------------------|--------|
| ShopCount           | Number   | 10                     | 有效商店数   |
| SaleProgress| Number | 0.35 | 销售进度,0-1之间的一个值  |

### 提交订单

### 取消订单

### 查询订单

### 查询店铺订单

## 店铺相关接口

### 查询店铺信息

### 设置店铺信息（新增门店)

## 消息相关接口

### 发布消息

### 获取消息

### 阅读消息


## 商品相关接口

### 查询商品详情

### 同步库存

## 部门相关接口

### 获取部门数据

### 获取部门员工

## 签到接口

### 商店签到

### 商店上传图片

## 地图接口相关
