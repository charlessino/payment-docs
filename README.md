# 富国支付网关文档

### <span id="1-----">1 概要</span>

#### <span id="11-----">1.1 接口用途</span>

本接口用于接入富国支付系统。本接口纯Restful风格，传入参数及返回参数全部为JSON格式。

#### <span id="12-----">1.2 接口申请</span>

请联系客服申请接口，申请通过之后您可获得以下信息：

    1. 专属接口请求地址(apiAddress)
    2. 应用ID(appId)
    3. 密钥(key)

#### <span id="13-----">1.3 Header 参数</span>

请求方式：POST

| 参数名       | 必填 | 类型/参数值      | 说明         |
| ------------ | ---- | ---------------- | ------------ |
| Content-Type | 是   | application/json | 请求参数类型 |

#### <span id="14-----">1.4 签名</span>

  1. 将全部传入参数（除了sign）的参数名按照字典序排列，请注意值为空的参数无需传入
  2. 构建为链接参数格式
  3. 在最后加上key（密钥请联系客服）
  4. 将字符串进行md5加密
  5. 转换为小写

##### <span id="141-----">1.4.1 签名示例</span>

 - 1.传入参数

```json
{
    "appId": "B32D954CC4E25491F99EFE42DF1CCBBF",
    "channelId": 1,
    "currency": "CNY",
    "actionValue": 1200.00,
    "outOrderId": "ESP837647136232",
    "outTips": "测试订单"
}
```

 - 2.参数名按字典序排列

```json
{
    "actionValue": 1200.00,
    "appId": "B32D954CC4E25491F99EFE42DF1CCBBF",
    "channelId": 1,
    "currency": "CNY",
    "outOrderId": "ESP837647136232",
    "outTips": "测试订单"
}
```

 - 3.构建为链接参数格式

actionValue=1200.00&appId=B32D954CC4E25491F99EFE42DF1CCBBF&channelId=1&currency=CNY&outOrderId=ESP837647136232&outTips=测试订单

 - 4.在最后加上key（假设key为aaabbbccc）

actionValue=1200.00&appId=B32D954CC4E25491F99EFE42DF1CCBBF&channelId=1&currency=CNY&outOrderId=ESP837647136232&outTips=测试订单&key=aaabbbccc

 - 5.转为小写的md5，即为sign

61695d4fb053b3c769205820170b9dea

​    

### <span id="2-----">2 接口列表</span>

​    

#### <span id="21-----">2.1 代收</span>

请求地址：`{apiAddress}/recharge`

##### <span id="211-----">2.1.1 传入参数</span>

| 参数名      | 必填 | 类型    | 字段长度 | 例子     | 说明                     |
| ----------- | ---- | ------- | -------- | -------- | ------------------------ |
| appId       | 是   | string  | 32       |          | 应用ID                   |
| channelId   | 是   | int     | 5        | 1        | 支付渠道ID，参考渠道章节 |
| currency     | 是   | string | 10    | CNY  | 货币，参考货币列表章节       |
| actionValue | 是   | decimal | 18, 2    | 2100.10  | 申请代收的金额 (就算是没有小数的货币，也需格式化为2位小数)       |
| accountName | 是   | string | 100    | 张三  | 付款人姓名       |
| callbackUrl  |      | string  | 512      |          | 第三方回调地址             |
| returnUrl  |      | string  | 512      |          | 支付完成后，第三方页面返回地址             |
| outOrderId  |      | string  | 100      |          | 第三方订单号             |
| outTips     |      | string  | 100      | 测试订单 | 第三方备注               |
| sign        | 是   | string  | 32       |          | 参考签名章节             |

##### <span id="212-----">2.1.2 返回参数</span>

| 参数名 | 类型   | 字段长度 | 例子    | 说明                                         |
| ------ | ------ | -------- | ------- | -------------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| transactionId    | string | 100      |    RC_10086     | 交易流水号 |
| url    | string | 255      |         | 用于跳转至支付页面的链接，请在应用中直接打开 |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |

##### <span id="213-----">2.1.3 异步回调通知参数</span>

| 参数名 | 类型   | 字段长度 | 例子    | 说明                                         |
| ------ | ------ | -------- | ------- | -------------------------------------------- |
| transactionId    | string | 100      |    RC_10086     | 交易流水号 |
| outOrderId | string    | 100        |        | 第三方订单号                      |
| outTips    | string  | 100      | 测试订单 | 第三方备注 |
| currency    | string | 10    | CNY  | 货币，参考货币列表章节 |
| actionValue    | decimal | 18, 2    | 2100.10  | 实际代收金额 (就算是没有小数的货币，也会被格式化为2位小数)      |
| status    | int | 1      | 1 | 1=代收成功 0=代收失败      |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |

##### <span id="214-----">2.1.4 请求参数示例</span>

 - 传入参数

```json
{
    "appId": "B32D954CC4E25491F9UIETG3CCBBF",
    "channelId": 1,
    "currency": "CNY",
    "actionValue": 2000.00,
    "accountName": "张三",
    "callbackUrl": "https://aaa.bbb.ccc/port1/withdraw",
    "outOrderId": "WE8681762354832",
    "outTips": "测试订单",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```
##### <span id="215-----">2.1.5 返回参数示例</span>

 - 返回成功

```json
{
    "result": 1,
    "transactionId": "RC_10086",
    "url": "https://aaa.bbb.ccc/?token=lakshfksh2ui3y4723726",
    "msg": "success"
}
```

 - 返回失败

```json
{
    "result": 0,
    "url": null,
    "msg": "签名不正确"
}
```

##### <span id="216-----">2.1.6 异步回调通知参数示例</span>

 - 代收成功

```json
{
    "transactionId": "RC_10086",
    "outOrderId": "8986327638746",
    "outTips": "用户申请代收",
    "currency": "CNY",
    "actionValue": 2500.00,
    "status": 1,
    "msg": "success"
}
```

 - 代收失败

```json
{
    "transactionId": "RC_10086",
    "outOrderId": "8986327638746",
    "outTips": "用户申请代收",
    "currency": "CNY",
    "actionValue": 2500.00,
    "status": 0,
    "msg": "通道维护暂时关闭"
}
```

##### <span id="217-----">2.1.7 支付渠道列表</span>

请通过商户后台系统 > 信息概览 > 支付通道，获取渠道ID及对应名称。

##### <span id="218-----">2.1.8 货币列表</span>

| 参数   | 货币名称     |
| ---- | -------- |
| CNY    | 人民币 |
| USDT    | USDT |
| VND    | 越南盾 |
| INR    | 印度卢比 |
| THB    | 泰铢 |
| IDR    | 印度尼西亚卢比 |
| BRL    | 巴西雷亚尔 |
| MXP    | 墨西哥比索 |
| JPY    | 日元 |
| PHP    | 菲律宾比索 |

#### <span id="22-----">2.2 代付</span>

请求地址：`{apiAddress}/withdraw`

##### <span id="221-----">2.2.1 传入参数</span>

| 参数名    | 必填 | 类型     | 字段长度 | 例子 | 说明                                          |
| --------- | ---- | -------- | -------- | ---- | --------------------------------------------- |
| appId     | 是   | string   | 32       |      | 应用ID                                        |
| channelId | 是   | int | 5       |   1   | 支付渠道ID，参考渠道章节            |
| currency     | 是   | string | 10    | CNY  | 货币，参考货币列表章节       |
| actionValue | 是   | decimal | 18, 2    | 2100.10  | 申请代付的金额 (就算是没有小数的货币，也需格式化为2位小数)       |
| cardNumber      | 是   | string   | 100        | 982268716  | 卡号（账号）         |
| bankName    |  是  | string      | 100        | 中国建设银行   | 银行名称 |
| branchName      |    | string   | 100        | 上海分行  | 分支行名称         |
| ownerName      |  是  | string   | 100        | 张三  | 户主姓名         |
| callbackUrl  |      | string  | 512      |          | 第三方回调地址             |
| outOrderId  |      | string  | 100      |          | 第三方订单号             |
| outTips     |      | string  | 100      | 测试订单 | 第三方备注               |
| sign      | 是   | string   | 32       |      | 签名                                          |

##### <span id="222-----">2.2.2 返回参数</span>

| 参数名     | 类型   | 字段长度 | 例子           | 说明                                      |
| ---------- | ------ | -------- | -------------- | ----------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| transaction_id    | int | 10      |         | 交易流水号 |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |

##### <span id="223-----">2.2.3 调用示例</span>

 - 传入参数

```json
{
    "appId": "B32D954CC4E25491F9UIETG3CCBBF",
    "channelId": 1,
    "currency": "CNY",
    "actionValue": 2000.00,
    "cardNumber": "938265716",
    "callbackUrl": "https://aaa.bbb.ccc/port1/withdraw",
    "outOrderId": "WE8681762354832",
    "outTips": "测试订单",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

 - 返回参数（成功）

```json
{
    "result": 1,
    "transaction_id": "87262176",
    "msg": "success"
}
```

 - 返回参数（失败）

```json
{
    "result": 0,
    "transaction_id": NULL,
    "msg": "传入参数格式有误"
}
```
