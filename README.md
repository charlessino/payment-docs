# 恒盈四方支付网关文档

### <span id="1-----">1 概要</span>

#### <span id="11-----">1.1 接口用途</span>

本接口用于接入恒盈四方支付系统。本接口纯Restful风格，传入参数及返回参数全部为JSON格式。

#### <span id="12-----">1.2 接口申请</span>

请联系客服申请接口，申请通过之后您可获得以下信息：

    1. 专属接口请求地址(apiAddress)
    2. 应用ID(appId)
    3. 密钥(key)

#### <span id="13-----">1.3 Header 参数</span>

请求方式：POST

| 参数名       | 必选 | 类型/参数值      | 说明         |
| ------------ | ---- | ---------------- | ------------ |
| Content-Type | 是   | application/json | 请求参数类型 |

#### <span id="14-----">1.4 签名</span>

  1. 将全部传入参数（除了sign）的参数名按照字典序排列，请注意值为空的参数无需传入
  2. 构建为链接参数格式
  3. 在最后加上key（密钥请联系客服）
  4. 将字符串进行md5加密
  5. 转换为小写

##### <span id="141-----">1.4.1 签名示例</span>

 - 1.原始传入参数

```json
{
    "appId": "B32D954CC4E25491F99EFE42DF1CCBBF",
    "channelId": 1,
    "actionValue": 1200.00,
    "outOrderId": "ESP837647136232",
    "outTips": "测试订单"
}
```

 - 2.参数名先按字典序排列

```json
{
    "actionValue": 1200.00,
    "appId": "B32D954CC4E25491F99EFE42DF1CCBBF",
    "channelId": 1,
    "outOrderId": "ESP837647136232",
    "outTips": "测试订单"
}
```

 - 3.构建为链接参数格式

actionValue=1200.00&appId=B32D954CC4E25491F99EFE42DF1CCBBF&channelId=1&outOrderId=ESP837647136232&outTips=测试订单

 - 4.在最后加上key（假设key为aaabbbccc）

actionValue=1200.00&appId=B32D954CC4E25491F99EFE42DF1CCBBF&channelId=1&outOrderId=ESP837647136232&outTips=测试订单&key=aaabbbccc

 - 5.转为小写的md5，即为sign

6cf32dc0412ff0614d3f0091b556883f

​    

### <span id="2-----">2 接口列表</span>

​    

#### <span id="21-----">2.1 代收</span>

请求地址：`{apiAddress}/payment`

##### <span id="211-----">2.1.1 传入参数</span>

| 参数名      | 必选 | 类型    | 字段长度 | 例子     | 说明                     |
| ----------- | ---- | ------- | -------- | -------- | ------------------------ |
| appId       | 是   | string  | 32       |          | 应用ID                   |
| channelId   | 是   | int     | 5        | 2        | 支付渠道ID，参考[渠道列表](#31-----) |
| actionValue | 是   | decimal | 18, 2    | 2100.10  | 申请代付的金额(元)       |
| callbackUrl |      | text    |          |          | 异步通知地址             |
| returnUrl   |      | text    |          |          | 网页跳转地址             |
| outOrderId  |      | string  | 100      | RB383727 | 第三方订单号             |
| outTips     |      | string  | 100      | 测试订单 | 第三方备注               |
| sign        | 是   | string  | 32       |          | 参考签名章节             |

##### <span id="212-----">2.1.2 返回参数</span>

| 参数名 | 类型   | 字段长度 | 例子    | 说明                                         |
| ------ | ------ | -------- | ------- | -------------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| url    | string | 255      |         | 用于跳转至支付页面的链接，请在应用中直接打开 |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |

##### <span id="213-----">2.1.3 返回示例</span>

 - 成功

```json
{
    "result": 1,
    "url": "https://aaa.bbb.ccc/kdshfaksjdfkwhejfs",
    "msg": "success"
}
```

 - 失败

```json
{
    "result": 0,
    "url": null,
    "msg": "签名不正确"
}
```

##### <span id="214-----">2.1.4 代收回调参数</span>

回调信息将会以JSON格式发送至您请求时定义的**`callbackUrl`**，收到信息后请返回纯文本**`success`**进行确认，如果未得到确认，系统将以每分钟为频率重复回调，至多10次。

| 参数名        | 类型    | 字段长度 | 例子     | 说明                             |
| ------------- | ------- | -------- | -------- | -------------------------------- |
| transactionId | int     | 11       | 13877272 | 系统流水号                       |
| outOrderId    | string  | 100      | RB383727 | 第三方订单号（原样返回）         |
| outTips       | string  | 100      | 测试订单 | 第三方备注（原样返回）           |
| actionValue   | decimal | 18, 2    | 1000.00  | 代收金额                         |
| status        | int     | 1        | 1        | 1=成功 0=失败 （只有这两种可能） |
| reason        | string  | 100      | success  | 返回结果为失败时，显示失败原因   |

##### <span id="215-----">2.1.5 代收回调示例</span>

 - 成功

```json
{
    "transactionId": 13877272,
    "outOrderId": "RB383727",
    "outTips": NULL,
    "actionValue": 1000.00,
    "status": 1,
    "reason": "success"
}
```

 - 失败

```json
{
    "transactionId": 13877272,
    "outOrderId": "RB383727",
    "outTips": "测试订单",
    "actionValue": 1000.00,
    "status": 0,
    "reason": "账号状态异常"
}
```



#### <span id="22-----">2.2 代付</span>

请求地址：`{apiAddress}/withdraw`

##### <span id="211-----">2.2.1 传入参数</span>

| 参数名      | 必选 | 类型    | 字段长度 | 例子        | 说明                                |
| ----------- | ---- | ------- | -------- | ----------- | ----------------------------------- |
| appId       | 是   | string  | 32       |             | 应用ID                              |
| channelId   | 是   | int     | 5        | 2           | 支付渠道ID，参考[渠道列表](#31-----) |
| actionValue | 是   | decimal | 18, 2    | 2100.10     | 申请代付的金额(元)                  |
| callbackUrl |      | text    |          |             | 异步通知地址                        |
| bankName    |      | string  | 100      | 中国银行    | 银行名                              |
| branchName  |      | string  | 100      | 广州市分行  | 分行名                              |
| cardNumber  | 是   | string  | 100      | 33000000000 | 卡号（账号）                        |
| ownerName   |      | string  | 100      | 张三        | 所有者姓名                          |
| outOrderId  |      | string  | 100      | RB383727    | 第三方订单号                        |
| outTips     |      | string  | 100      | 测试订单    | 第三方备注                          |
| sign        | 是   | string  | 32       |             | 参考签名章节                        |

##### <span id="222-----">2.2.2 返回参数</span>

| 参数名 | 类型   | 字段长度 | 例子    | 说明                                         |
| ------ | ------ | -------- | ------- | -------------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |

##### <span id="223-----">2.2.3 返回示例</span>

 - 成功

```json
{
    "result": 1,
    "msg": "success"
}
```

 - 失败

```json
{
    "result": 0,
    "msg": "签名不正确"
}
```

##### <span id="224-----">2.2.4 代付回调参数</span>

回调信息将会以JSON格式发送至您请求时定义的**`callbackUrl`**，收到信息后请返回纯文本**`success`**进行确认，如果未得到确认，系统将以每分钟为频率重复回调，至多10次。

| 参数名        | 类型    | 字段长度 | 例子     | 说明                             |
| ------------- | ------- | -------- | -------- | -------------------------------- |
| transactionId | int     | 11       | 13877272 | 系统流水号                       |
| outOrderId    | string  | 100      | RB383727 | 第三方订单号（原样返回）         |
| outTips       | string  | 100      | 测试订单 | 第三方备注（原样返回）           |
| actionValue   | decimal | 18, 2    | 1000.00  | 代付金额                         |
| status        | int     | 1        | 1        | 1=成功 0=失败 （只有这两种可能） |
| reason        | string  | 100      | success  | 返回结果为失败时，显示失败原因   |

##### <span id="215-----">2.1.5 代收回调示例</span>

 - 成功

```json
{
    "transactionId": 13877272,
    "outOrderId": "RB383727",
    "outTips": NULL,
    "actionValue": 1000.00,
    "status": 1,
    "reason": "success"
}
```

 - 失败

```json
{
    "transactionId": 13877272,
    "outOrderId": "RB383727",
    "outTips": "测试订单",
    "actionValue": 1000.00,
    "status": 0,
    "reason": "商户余额不足"
}
```



### <span id="3-----">3 参考信息</span>

​    

#### <span id="31-----">3.1 渠道列表</span>

| ID   | 名称     |
| ---- | -------- |
| 1    | 微信支付 |
| 2    | Gcash    |
