<img width="66" height="2065" alt="image" src="https://github.com/user-attachments/assets/4f93ba2c-5e28-41b3-b485-20cadc6b8adf" /># 支付网关文档

- 目录
  + [1 概要](#1-----)
    - [1.1 接口用途](#11-----)
    - [1.2 接口申请](#12-----)
    - [1.3 Header 参数](#13-----)
    - [1.4 签名](#14-----)
      * [1.4.1 签名示例](#141-----)
      * [1.4.2 返回参数签名](#142-----)
    - [1.5 回调机制](#15-----)
  + [2 接口列表](#2-----)
    + [2.1 代收](#21-----)
      + [2.1.1 传入参数](#211-----)
      + [2.1.2 返回参数](#212-----)
      + [2.1.3 异步回调通知参数](#213-----)
      + [2.1.4 请求参数示例](#214-----)
      + [2.1.5 返回参数示例](#215-----)
      + [2.1.6 异步回调通知参数示例](#216-----)
    + [2.2 代付](#21-----)
      + [2.2.1 传入参数](#221-----)
      + [2.2.2 返回参数](#222-----)
      + [2.2.3 异步回调通知参数](#223-----)
      + [2.2.4 请求参数示例](#224-----)
      + [2.2.5 返回参数示例](#225-----)
      + [2.2.6 异步回调通知参数示例](#226-----)
    + [2.3 代收订单查询](#23-----)
      + [2.3.1 单一代收订单查询](#231-----)
        + [2.3.1.1 传入参数](#2311-----)
        + [2.3.1.2 返回参数](#2312-----)
        + [2.3.1.3 请求参数示例](#2313-----)
        + [2.3.1.4 返回参数示例](#2314-----)
      + [2.3.2 批量代收订单查询](#232-----)
        + [2.3.2.1 传入参数](#2321-----)
        + [2.3.2.2 返回参数](#2322-----)
        + [2.3.2.3 data格式示意](#2323-----)
        + [2.3.2.4 请求参数示例](#2324-----)
        + [2.3.2.5 返回参数示例](#2325-----)
    + [2.4 代付订单查询](#24-----)
      + [2.4.1 单一代付订单查询](#241-----)
        + [2.4.1.1 传入参数](#2411-----)
        + [2.4.1.2 返回参数](#2412-----)
        + [2.4.1.3 请求参数示例](#2413-----)
        + [2.4.1.4 返回参数示例](#2414-----)
      + [2.4.2 批量代付订单查询](#242-----)
        + [2.4.2.1 传入参数](#2421-----)
        + [2.4.2.2 返回参数](#2422-----)
        + [2.4.2.3 data格式示意](#2423-----)
        + [2.4.2.4 请求参数示例](#2424-----)
        + [2.4.2.5 返回参数示例](#2425-----)
    + [2.5 余额查询](#25-----)
      + [2.5.1 传入参数](#251-----)
      + [2.5.2 返回参数](#252-----)
      + [2.5.3 请求参数示例](#253-----)
      + [2.5.4 返回参数示例](#254-----)
  + [3 附件](#3-----)
    + [3.1 通道列表](#31-----)
    + [3.2 货币列表](#32-----)
    + [3.3 银行名称列表-代收](#33-----)
    	+ [3.3.1 银行名称列表-越南代收](#331-----)
    + [3.4 银行名称列表-代付](#34-----)
    	+ [3.4.1 银行名称列表-中国代付](#341-----)
    	+ [3.4.2 银行名称列表-USDT代付](#342-----)
    	+ [3.4.3 银行名称列表-越南代付](#343-----)
    	+ [3.4.4 银行名称列表-韩国代付](#344-----)
    	+ [3.4.5 银行名称列表-菲律宾代付](#345-----)
    	+ [3.4.6 银行名称列表-印度代付](#346-----)
    	+ [3.4.7 银行名称列表-日本代付](#347-----)
    	+ [3.4.8 银行名称列表-泰国代付](#348-----)
        + [3.4.9 银行名称列表-巴西代付](#349-----)
        + [3.4.10 银行名称列表-印度尼西亚代付](#3410-----)
        + [3.4.11 银行名称列表-孟加拉代付](#3411-----)

### <span id="1-----">1 概要</span>

#### <span id="11-----">1.1 接口用途</span>

本接口用于接入支付系统。本接口纯Restful风格，传入参数及返回参数全部为JSON格式。

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

##### <span id="142-----">1.4.2 返回参数签名</span>

每次请求接口，接口返回时会带着一个sign，这个sign的签名规则为：md5(nonceStr + key)

举例：<br>
nonceStr = 123456<br>
key = aaabbbccc<br>
md5("123456aaabbbccc") = 4118e6a1a1d43665ba1b77f49759b130<br>

4118e6a1a1d43665ba1b77f49759b130 就是返回参数的签名

请注意，如果不传nonceStr参数，则不会返回这个sign

#### <span id="15-----">1.5 回调机制</span>

  1. 代收和代付订单在收到状态更新之后，会立即回调商户指定的回调地址
  2. 如果收到商户返回success字样表示回调成功
  3. 如果未收到success字样，系统会每隔一分钟尝试再次回调，最多10次，直到收到success时停止
  4. 10次之后仍然未收到success字样，系统也不再发送回调信息
  5. 如您需要再次回调，欢迎随时联系客服



### <span id="2-----">2 接口列表</span>

​    

#### <span id="21-----">2.1 代收</span>

请求地址：`{apiAddress}/recharge`<br>
Header：Content-Type: application/json;charset=utf-8

##### <span id="211-----">2.1.1 传入参数</span>

| 参数名      | 必填 | 类型    | 字段长度 | 例子     | 说明                     |
| ----------- | ---- | ------- | -------- | -------- | ------------------------ |
| appId       | 是   | string  | 32       |          | 应用ID                   |
| channelId   | 是   | int     | 5        | 1        | [通道列表](#31-----) |
| currency     | 是   | string | 10    | CNY  | [货币列表](#32-----)       |
| actionValue | 是   | decimal | 18, 2    | 2100.00  | 申请代收的金额 (数字货币允许有小数，法币仅允许整数，就算是整数也需格式化为2位小数以便统一验签规则)       |
| accountName | 是   | string | 100    | 张三  | 转账人姓名，姓名中不可包含数字，且不可传空值。当货币为USDT时可不传此参数。当货币为日元时务必传送片假文。       |
| cellphone |    | string | 100    | 01034388769  | 手机号，菲律宾、韩国和印度必传，其他可不传       |
| callbackUrl  |      | string  | 512      |          | 商户回调地址             |
| returnUrl  |      | string  | 512      |          | 支付完成后，商户页面返回地址             |
| outOrderId  |      | string  | 100      |          | 商户订单号             |
| outTips     |      | string  | 100      | 测试订单 | 商户备注               |
| returnType     |      | int  | 1      | 1 | 返回类型 1=充值链接 2=银行、卡号、户名的文本信息。默认为1               |
| nonceStr     |      | string  | 100      | 123456 | 随机数，用于获得返回参数签名，可不传               |
| param1     |      | string  | 100      |  | 越南通道ID5，则必传[银行名称列表-越南代收](#331-----)               |
| param2     |      | string  | 100      |  | 韩国代收通道，则必传玩家ID。               |
| param3     |      | string  | 100      |  | 印度代收通道，则必传玩家邮箱。               |
| sign        | 是   | string  | 32       |          | [签名](#14-----)             |

##### <span id="212-----">2.1.2 返回参数</span>

| 参数名 | 类型   | 字段长度 | 例子    | 说明                                         |
| ------ | ------ | -------- | ------- | -------------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| transactionId    | string | 100      |    RC_10086     | 交易流水号 |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |    | [返回参数签名](#142-----)      |
| outOrderId    | string | 100      |         | 商户订单号 |
| url    | string | 255      |         | 用于跳转至支付页面的链接，returnType=1时返回此项 |
| data    | array |       |         | returnType=2时返回此项，包含以下子项：<br>bankName: 银行名称<br>branchName: 支行名称<br>accountNumber: 银行卡号<br>accountOwner: 户主姓名<br>amount: 金额<br>randomString: 转账备注<br>qrCode: 二维码图片的Base64编码，可直接显示在HTML当中，例如：&lt;img src=&quot;编码&quot;&gt; |

##### <span id="213-----">2.1.3 异步回调通知参数</span>
收到回调时请返回success字样，详情参考[回调机制](#15-----)

| 参数名 | 类型   | 字段长度 | 例子    | 说明                                         |
| ------ | ------ | -------- | ------- | -------------------------------------------- |
| transactionId    | string | 100      |    RC_10086     | 交易流水号 |
| outOrderId | string    | 100        |        | 商户订单号                      |
| outTips    | string  | 100      | 测试订单 | 商户备注 |
| currency    | string | 10    | CNY  | [货币列表](#32-----)  |
| actionValue    | decimal | 18, 2    | 2100.00  | 实际代收金额 (就算是没有小数的货币，也会被格式化为2位小数)      |
| status    | int | 1      | 1 | 1=代收成功 0=代收失败      |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |  | 除了sign之外其他所有参数都需参与签名，同请求时的[签名](#14-----)规则      |

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
    "nonceStr": "123456",
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
    "msg": "success",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
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
    "msg": "success",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
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
    "msg": "通道维护暂时关闭",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

#### <span id="22-----">2.2 代付</span>

请求地址：`{apiAddress}/withdraw`<br>
Header：Content-Type: application/json;charset=utf-8

##### <span id="221-----">2.2.1 传入参数</span>

| 参数名    | 必填 | 类型     | 字段长度 | 例子 | 说明                                          |
| --------- | ---- | -------- | -------- | ---- | --------------------------------------------- |
| appId     | 是   | string   | 32       |      | 应用ID                                        |
| channelId | 是   | int | 5       |   1   | [通道列表](#31-----)            |
| currency     | 是   | string | 10    | CNY  | [货币列表](#32-----)        |
| actionValue | 是   | decimal | 18, 2    | 2100.00  | 申请代付的金额 (数字货币允许有小数，法币仅允许整数，就算是整数也需格式化为2位小数以便统一验签规则)       |
| cardNumber      | 是   | string   | 100        | 982268716  | 卡号（账号）         |
| bankName    |  是  | string      | 100        | 中国建设银行   | [银行名称列表-代付](#34-----)，标准名称   |
| branchName      |    | string   | 100        | 上海分行  | 分支行名称。当货币为INR时此处必传银行名称所对应的IFSC编码。当货币为JPY时此处必传，番号与名称之间以&相连，例“209&リズム⽀店“。         |
| ownerName      |  是  | string   | 100        | 张三  | 收款人姓名，姓名中不可包含数字。当货币为USDT时可不传此参数。中国卡卡和银联通道务必传送实名注册姓名。         |
| callbackUrl  |      | string  | 512      |          | 商户回调地址             |
| outOrderId  |      | string  | 100      |          | 商户订单号             |
| outTips     |      | string  | 100      | 测试订单 | 商户备注               |
| nonceStr     |      | string  | 100      | 123456 | 随机数，用于获得返回参数签名，可不传               |
| param1     |      | string  | 100      |  | 韩国代付，则必传玩家ID。               |
| param2     |      | string  | 100      |  | 巴西PIX，则必传身份证号。印度代付，则必传玩家手机号码。               |
| param3     |      | string  | 100      |  | 印度代付，则必传玩家邮箱。               |
| sign      | 是   | string   | 32       |      | [签名](#14-----)                            |

##### <span id="222-----">2.2.2 返回参数</span>

| 参数名     | 类型   | 字段长度 | 例子           | 说明                                      |
| ---------- | ------ | -------- | -------------- | ----------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| transactionId    | int | 10      |         | 交易流水号 |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |    | [返回参数签名](#142-----)      |

##### <span id="223-----">2.2.3 异步回调通知参数</span>
收到回调时请返回success字样，详情参考[回调机制](#15-----)

| 参数名 | 类型   | 字段长度 | 例子    | 说明                                         |
| ------ | ------ | -------- | ------- | -------------------------------------------- |
| transactionId    | string | 100      |    RC_10086     | 交易流水号 |
| outOrderId | string    | 100        |        | 商户订单号                      |
| outTips    | string  | 100      | 测试订单 | 商户备注 |
| currency    | string | 10    | CNY  | [货币列表](#32-----)  |
| actionValue    | decimal | 18, 2    | 2100.00  | 实际代付金额 (就算是没有小数的货币，也会被格式化为2位小数)      |
| status    | int | 1      | 1 | 1=代付成功 0=代付失败      |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |  | 除了sign之外其他所有参数都需参与签名，同请求时的[签名](#14-----)规则     |

##### <span id="224-----">2.2.4 请求参数示例</span>

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
    "nonceStr": "123456",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

##### <span id="225-----">2.2.5 返回参数示例</span>

 - 返回参数（成功）

```json
{
    "result": 1,
    "transactionId": "87262176",
    "msg": "success",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

 - 返回参数（失败）

```json
{
    "result": 0,
    "transactionId": NULL,
    "msg": "传入参数格式有误"
}
```

##### <span id="226-----">2.2.6 异步回调通知参数示例</span>

 - 代付成功

```json
{
    "transactionId": "RC_10086",
    "outOrderId": "8986327638746",
    "outTips": "这是一个代付订单",
    "currency": "CNY",
    "actionValue": 4000.00,
    "status": 1,
    "msg": "success",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

 - 代付失败

```json
{
    "transactionId": "RC_10086",
    "outOrderId": "8986327638746",
    "currency": "CNY",
    "actionValue": 4000.00,
    "status": 0,
    "msg": "通道维护暂时关闭",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```


#### <span id="23-----">2.3 代收订单查询</span>

#### <span id="231-----">2.3.1 单一代收订单查询</span>

请求地址：`{apiAddress}/recharge-single-order-query`<br>
Header：Content-Type: application/json;charset=utf-8

##### <span id="2311-----">2.3.1.1 传入参数</span>

| 参数名    | 必填 | 类型     | 字段长度 | 例子 | 说明                                          |
| --------- | ---- | -------- | -------- | ---- | --------------------------------------------- |
| appId     | 是   | string   | 32       |      | 应用ID                                        |
| outOrderId     |  是   | string    | 100        |        | 商户订单号                      |
| nonceStr     |      | string  | 100      | 123456 | 随机数，用于获得返回参数签名，可不传               |
| sign      | 是   | string   | 32       |      | [签名](#14-----)                         |

##### <span id="2312-----">2.3.1.2 返回参数</span>

| 参数名     | 类型   | 字段长度 | 例子           | 说明                                      |
| ---------- | ------ | -------- | -------------- | ----------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| data    | array |       |     transactionId: RC_98261876 (交易流水号)<br>currency: CNY (货币)<br>channelId: 15 (通道ID)<br>rechargeRate: 0.01 (代收手续费率)<br>actionValue: 3000.00 (实际代收金额)<br>chargeValue: 30.00 (代收手续费)<br>actualValue: 2970.00 (实际记账金额)<br>accountName: 张三 (付款人姓名)<br>status: 1 (状态值 1=成功 0=失败 2=处理中)<br>statusName: 成功 (状态名)<br>outOrderId: 98227863223 (商户订单号)<br>outTips: 测试的订单 (商户备注)<br>lastUpdatedTime: 2024-02-01 12:15:33 (订单更新时间)<br>createTime: 2024-02-01 09:31:26 (订单生成时间)    | 订单详情以一维数组方式呈现 |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |    | [返回参数签名](#142-----)      |

##### <span id="2313-----">2.3.1.3 请求参数示例</span>

 - 传入参数

```json
{
    "appId": "B32D954CC4E25491F9UIETG3CCBBF",
    "outOrderId": "ABC123456",
    "nonceStr": "123456",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

##### <span id="2314-----">2.3.1.4 返回参数示例</span>

 - 返回参数（成功）

```json
{
	"result": 1,
	"data": {
		"transactionId": "RC_17",
		"currency": "CNY",
		"channelId": "1",
		"rechargeRate": "0.0400",
		"actionValue": "1931.00",
		"chargeValue": "77.24",
		"actualValue": "1853.76",
		"accountName": "\u5f20\u4e09",
		"status": 1,
		"statusName": "\u6210\u529f",
		"outOrderId": "TT_1708203061",
		"outTips": "\u6d4b\u8bd51",
		"lastUpdatedTime": "2024-02-18 04:53:44",
		"createTime": "2024-02-18 04:51:01"
	},
	"msg": "success",
	"sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

 - 返回参数（失败）
```json
{
	"result": 0,
	"data": {},
	"msg": "找不到订单记录",
	"sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

#### <span id="232-----">2.3.2 批量代收订单查询</span>

请求地址：`{apiAddress}/recharge-orders-query`<br>
Header：Content-Type: application/json;charset=utf-8

##### <span id="2321-----">2.3.2.1 传入参数</span>

| 参数名    | 必填 | 类型     | 字段长度 | 例子 | 说明                                          |
| --------- | ---- | -------- | -------- | ---- | --------------------------------------------- |
| appId     | 是   | string   | 32       |      | 应用ID                                        |
| outOrderId     |     | string    | 100        |        | 商户订单号                      |
| dateTimeStart     |    | datetime | 19    | 2024-01-01 10:00:00  | 订单更新时间-起始值       |
| dateTimeEnd     |    | datetime | 19    | 2024-01-01 10:00:00  | 订单更新时间-结束值       |
| pageId      |    | int   | 10        | 3  | 每次最多返回200条记录<br>可使用本字段进行翻页<br>不传此参数默认为1         |
| orderBy    |    | string      | 4        | ASC   | 顺序<br>ASC=升序，DESC=降序<br>不传此参数默认DESC |
| nonceStr     |      | string  | 100      | 123456 | 随机数，用于获得返回参数签名，可不传               |
| sign      | 是   | string   | 32       |      | [签名](#14-----)                         |

##### <span id="2322-----">2.3.2.2 返回参数</span>

| 参数名     | 类型   | 字段长度 | 例子           | 说明                                      |
| ---------- | ------ | -------- | -------------- | ----------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| data    | array |       |         | 返回结果详情，格式参考以下示意 |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |    | [返回参数签名](#142-----)      |

##### <span id="2323-----">2.3.2.3 data格式示意</span>

| 参数名    | 例子           | 说明     |
| ---------- | ------ | -------- |
| orderList |    transactionId: RC_98261876 (交易流水号)<br>currency: CNY (货币)<br>channelId: 15 (通道ID)<br>rechargeRate: 0.01 (代收手续费率)<br>actionValue: 3000.00 (实际代收金额)<br>chargeValue: 30.00 (代收手续费)<br>actualValue: 2970.00 (实际记账金额)<br>accountName: 张三 (付款人姓名)<br>status: 1 (状态值 1=成功 0=失败 2=处理中)<br>statusName: 成功 (状态名)<br>outOrderId: 98227863223 (商户订单号)<br>outTips: 测试的订单 (商户备注)<br>lastUpdatedTime: 2024-02-01 12:15:33 (订单更新时间)<br>createTime: 2024-02-01 09:31:26 (订单生成时间)   | 订单详情以二维数组方式排列                      |
| currentPage |    1    | 当前页码，默认为1<br>每页最多200条记录                      |
| totalPages |    5    | 当前搜索结果可以翻页的总页码<br>例如5表示总共有5页<br>可以在传参时使用pageId翻页     |
| totalRecords |    350    | 当前搜索结果的总纪录数                      |

##### <span id="2324-----">2.3.2.4 请求参数示例</span>

 - 传入参数

```json
{
    "appId": "B32D954CC4E25491F9UIETG3CCBBF",
    "dateTimeStart": "2024-02-01 09:31:26",
    "dateTimeEnd": "2024-02-01 12:15:33",
    "pageId": 2,
    "nonceStr": "123456",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

##### <span id="2325-----">2.3.2.5 返回参数示例</span>

 - 返回参数（成功）

```json
{
	"result": 1,
	"data": {
		"orderList": [{
			"transactionId": "RC_17",
			"currency": "CNY",
			"channelId": "1",
			"rechargeRate": "0.0400",
			"actionValue": "1931.00",
			"chargeValue": "77.24",
			"actualValue": "1853.76",
			"accountName": "\u5f20\u4e09",
			"status": 1,
			"statusName": "\u6210\u529f",
			"outOrderId": "TT_1708203061",
			"outTips": "\u6d4b\u8bd51",
			"lastUpdatedTime": "2024-02-18 04:53:44",
			"createTime": "2024-02-18 04:51:01"
		}, {
			"transactionId": "RC_18",
			"currency": "CNY",
			"channelId": "1",
			"rechargeRate": "0.0400",
			"actionValue": "100.00",
			"chargeValue": "4.00",
			"actualValue": "96.00",
			"accountName": "\u5f20\u4e09",
			"status": 0,
			"statusName": "\u5931\u8d25",
			"outOrderId": "TT_1708203901",
			"outTips": "\u6d4b\u8bd51",
			"lastUpdatedTime": "2024-02-18 05:05:03",
			"createTime": "2024-02-18 05:05:01"
		}],
		"currentPage": 3,
		"totalPages": 17,
		"totalRecords": 3752
	},
	"msg": "success",
    	"sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

 - 返回参数（失败）
```json
{
	"result": 0,
	"data": {
		"orderList": {}, 
		"currentPage": 0,
		"totalPages": 0,
		"totalRecords": 0
	},
	"msg": "找不到订单记录",
    	"sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

#### <span id="24-----">2.4 代付订单查询</span>

#### <span id="241-----">2.4.1 单一代付订单查询</span>

请求地址：`{apiAddress}/withdraw-single-order-query`<br>
Header：Content-Type: application/json;charset=utf-8

##### <span id="2411-----">2.4.1.1 传入参数</span>

| 参数名    | 必填 | 类型     | 字段长度 | 例子 | 说明                                          |
| --------- | ---- | -------- | -------- | ---- | --------------------------------------------- |
| appId     | 是   | string   | 32       |      | 应用ID                                        |
| outOrderId     |   是  | string    | 100        |        | 商户订单号                      |
| nonceStr     |      | string  | 100      | 123456 | 随机数，用于获得返回参数签名，可不传               |
| sign      | 是   | string   | 32       |      | [签名](#14-----)                     |

##### <span id="2412-----">2.4.1.2 返回参数</span>

| 参数名     | 类型   | 字段长度 | 例子           | 说明                                      |
| ---------- | ------ | -------- | -------------- | ----------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| data    | array |       |    transactionId: WD_98261876 (交易流水号)<br>currency: CNY (货币)<br>channelId: 15 (通道ID)<br>withdrawRate: 0.01 (代付手续费率)<br>withdrawFixValue: 3.00 (代付单笔手续费)<br>actionValue: 3000.00 (实际代付金额)<br>chargeValue: 33.00 (代付手续费合计)<br>actualValue: 3033.00 (实际记账金额)<br>bankName: 工商银行 (银行名称)<br>branchName: 广州市分行 (分支行名称)<br>cardNumber: 982268716 (卡号)<br>ownerName: 张三 (户主姓名)<br>status: 1 (状态值 1=成功 0=失败 2=处理中)<br>statusName: 成功 (状态名)<br>outOrderId: 98227863223 (商户订单号)<br>outTips: 测试的订单 (商户备注)<br>lastUpdatedTime: 2024-02-01 12:15:33 (订单更新时间)<br>createTime: 2024-02-01 09:31:26 (订单生成时间)     | 订单详情以一维数组方式呈现 |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |    | [返回参数签名](#142-----)      |

##### <span id="2413-----">2.4.1.3 请求参数示例</span>

 - 传入参数

```json
{
    "appId": "B32D954CC4E25491F9UIETG3CCBBF",
    "outOrderId": "ABC123456",
    "nonceStr": "123456",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

##### <span id="2414-----">2.4.1.4 返回参数示例</span>

 - 返回参数（成功）

```json
{
	"result": 1,
	"data": {
		"transactionId": "WD_23",
		"currency": "CNY",
		"channelId": "1",
		"withdrawRate": "0.0000",
		"withdrawFixValue": "3.00",
		"actionValue": "300.00",
		"chargeValue": "3.00",
		"actualValue": "303.00",
		"bankName": "\u5de5\u5546\u94f6\u884c",
		"branchName": null,
		"cardNumber": "333774636229",
		"ownerName": "\u5f20\u4e09",
		"status": 0,
		"statusName": "\u5931\u8d25",
		"outOrderId": "TT_1708204095",
		"outTips": "\u6d4b\u8bd5\u6d4b\u8bd5",
		"lastUpdatedTime": "2024-02-18 05:08:16",
		"createTime": "2024-02-18 05:08:15"
	},
	"msg": "success",
	"sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

 - 返回参数（失败）
```json
{
	"result": 0,
	"data": {},
	"msg": "找不到订单记录",
	"sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

#### <span id="242-----">2.4.2 批量代付订单查询</span>

请求地址：`{apiAddress}/withdraw-orders-query`<br>
Header：Content-Type: application/json;charset=utf-8

##### <span id="2421-----">2.4.2.1 传入参数</span>

| 参数名    | 必填 | 类型     | 字段长度 | 例子 | 说明                                          |
| --------- | ---- | -------- | -------- | ---- | --------------------------------------------- |
| appId     | 是   | string   | 32       |      | 应用ID                                        |
| outOrderId     |     | string    | 100        |        | 商户订单号                      |
| dateTimeStart     |    | datetime | 19    | 2024-01-01 10:00:00  | 订单更新时间-起始值       |
| dateTimeEnd     |    | datetime | 19    | 2024-01-01 10:00:00  | 订单更新时间-结束值       |
| pageId      |    | int   | 10        | 3  | 每次最多返回200条记录<br>可使用本字段进行翻页<br>不传此参数默认为1         |
| orderBy    |    | string      | 4        | ASC   | 顺序<br>ASC=升序，DESC=降序<br>不传此参数默认DESC |
| nonceStr     |      | string  | 100      | 123456 | 随机数，用于获得返回参数签名，可不传               |
| sign      | 是   | string   | 32       |      | [签名](#14-----)                     |

##### <span id="2422-----">2.4.2.2 返回参数</span>

| 参数名     | 类型   | 字段长度 | 例子           | 说明                                      |
| ---------- | ------ | -------- | -------------- | ----------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| data    | array |       |         | 返回结果详情，格式参考以下示意 |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |    | [返回参数签名](#142-----)      |

##### <span id="2423-----">2.4.2.3 data格式示意</span>

| 参数名    | 例子           | 说明     |
| ---------- | ------ | -------- |
| orderList |    transactionId: WD_98261876 (交易流水号)<br>currency: CNY (货币)<br>channelId: 15 (通道ID)<br>withdrawRate: 0.01 (代付手续费率)<br>withdrawFixValue: 3.00 (代付单笔手续费)<br>actionValue: 3000.00 (实际代付金额)<br>chargeValue: 33.00 (代付手续费合计)<br>actualValue: 3033.00 (实际记账金额)<br>bankName: 工商银行 (银行名称)<br>branchName: 广州市分行 (分支行名称)<br>cardNumber: 982268716 (卡号)<br>ownerName: 张三 (户主姓名)<br>status: 1 (状态值 1=成功 0=失败 2=处理中)<br>statusName: 成功 (状态名)<br>outOrderId: 98227863223 (商户订单号)<br>outTips: 测试的订单 (商户备注)<br>lastUpdatedTime: 2024-02-01 12:15:33 (订单更新时间)<br>createTime: 2024-02-01 09:31:26 (订单生成时间)   | 订单详情以二维数组方式排列                      |
| currentPage |    1    | 当前页码，默认为1<br>每页最多200条记录                      |
| totalPages |    5    | 当前搜索结果可以翻页的总页码<br>例如5表示总共有5页<br>可以在传参时使用pageId翻页     |
| totalRecords |    350    | 当前搜索结果的总纪录数                      |

##### <span id="2424-----">2.4.2.4 请求参数示例</span>

 - 传入参数

```json
{
    "appId": "B32D954CC4E25491F9UIETG3CCBBF",
    "dateTimeStart": "2024-02-01 09:31:26",
    "dateTimeEnd": "2024-02-01 12:15:33",
    "pageId": 2,
    "nonceStr": "123456",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

##### <span id="2425-----">2.4.2.5 返回参数示例</span>

 - 返回参数（成功）

```json
{
	"result": 1,
	"data": {
		"orderList": [{
			"transactionId": "WD_23",
			"currency": "CNY",
			"channelId": "1",
			"withdrawRate": "0.0000",
			"withdrawFixValue": "3.00",
			"actionValue": "300.00",
			"chargeValue": "3.00",
			"actualValue": "303.00",
			"bankName": "\u5de5\u5546\u94f6\u884c",
			"branchName": null,
			"cardNumber": "333774636229",
			"ownerName": "\u5f20\u4e09",
			"status": 0,
			"statusName": "\u5931\u8d25",
			"outOrderId": "TT_1708204095",
			"outTips": "\u6d4b\u8bd5\u6d4b\u8bd5",
			"lastUpdatedTime": "2024-02-18 05:08:16",
			"createTime": "2024-02-18 05:08:15"
		}, {
			"transactionId": "WD_24",
			"currency": "CNY",
			"channelId": "1",
			"withdrawRate": "0.0000",
			"withdrawFixValue": "3.00",
			"actionValue": "30.00",
			"chargeValue": "3.00",
			"actualValue": "33.00",
			"bankName": "\u5de5\u5546\u94f6\u884c",
			"branchName": null,
			"cardNumber": "333774636229",
			"ownerName": "\u5f20\u4e09",
			"status": 0,
			"statusName": "\u5931\u8d25",
			"outOrderId": "TT_1708204132",
			"outTips": "\u6d4b\u8bd5\u6d4b\u8bd5",
			"lastUpdatedTime": "2024-02-18 05:08:53",
			"createTime": "2024-02-18 05:08:52"
		}],
		"currentPage": 1,
		"totalPages": 8,
		"totalRecords": 1560
	},
	"msg": "success",
    	"sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

 - 返回参数（失败）
```json
{
	"result": 0,
	"data": {
		"orderList": {}, 
		"currentPage": 0,
		"totalPages": 0,
		"totalRecords": 0
	},
	"msg": "找不到订单记录",
    	"sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

#### <span id="25-----">2.5 余额查询</span>

请求地址：`{apiAddress}/balance`<br>
Header：Content-Type: application/json;charset=utf-8

##### <span id="251-----">2.5.1 传入参数</span>

| 参数名    | 必填 | 类型     | 字段长度 | 例子 | 说明                                          |
| --------- | ---- | -------- | -------- | ---- | --------------------------------------------- |
| appId     | 是   | string   | 32       |      | 应用ID                                        |
| nonceStr     |      | string  | 100      | 123456 | 随机数，用于获得返回参数签名，可不传               |
| sign      | 是   | string   | 32       |      | [签名](#14-----)                        |

##### <span id="252-----">2.5.2 返回参数</span>

| 参数名     | 类型   | 字段长度 | 例子           | 说明                                      |
| ---------- | ------ | -------- | -------------- | ----------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| data    | array |       |     CNY: 6686.32 (人民币余额)<br>USDT: 927.92 (USDT余额)    | 以二维数组方式排列 |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |    | [返回参数签名](#142-----)      |

##### <span id="253-----">2.5.3 请求参数示例</span>

 - 传入参数

```json
{
    "appId": "B32D954CC4E25491F9UIETG3CCBBF",
    "nonceStr": "123456",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

##### <span id="254-----">2.5.4 返回参数示例</span>

 - 返回参数

```json
{
	"result": 1,
	"data": {
		"CNY": "6686.32",
		"USDT": "927.92"
	},
	"msg": "success",
    	"sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```





### <span id="3-----">3 附件</span>

#### <span id="31-----">3.1 通道列表</span>

请通过商户后台系统 > 信息概览 > 支付通道，获取通道ID及对应名称。

#### <span id="32-----">3.2 货币列表</span>

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
| KRW    | 韩元 |
| EB    | EB |

#### <span id="33-----">3.3 银行名称列表-代收</span>

##### <span id="331-----">3.3.1 银行名称列表-越南代收</span>

| 标准名称   |
| ---- |
| ACB |
| BIDV  |
| DONGABANK |
| EXIMBANK |
| MB |
| MSB  |
| SACOMBANK |
| SHB  |
| TCB |
| VCB |
| VIB  |
| VIETINBANK |
| VPBANK |

#### <span id="34-----">3.4 银行名称列表-代付</span>

##### <span id="341-----">3.4.1 银行名称列表-中国代付</span>

| 标准名称   |
| ---- |
| 工商银行 |
| 农业银行 |
| 中国银行 |
| 建设银行 |
| 云南农村信用合作社 |
| 交通银行 |
| 中信银行 |
| 光大银行 |
| 华夏银行 |
| 民生银行 |
| 广发银行 |
| 平安银行 |
| 招商银行 |
| 兴业银行 |
| 浦发银行 |
| 上海银行 |
| 北京银行 |
| 河北银行 |
| 承德银行 |
| 邢台银行 |
| 蒙商银行 |
| 盛京银行 |
| 昆仑银行 |
| 南京银行 |
| 浙江稠州商业银行 |
| 杭州银行 |
| 浙江民泰商业银行 |
| 齐鲁银行 |
| 临商银行 |
| 中原银行 |
| 平顶山银行 |
| 湖北银行 |
| 华融湘江银行 |
| 珠海华润银行 |
| 广东华兴银行 |
| 广西北部湾银行 |
| 德阳银行 |
| 乐山市商业银行 |
| 贵州银行 |
| 长安银行 |
| 江苏紫金农村商业银行 |
| 江苏农村商业银行 |
| 广州农村商业银行 |
| 成都农村商业银行 |
| 浙商银行 |
| 天津农村商业银行 |
| 渤海银行 |
| 浙江网商银行 |
| 新安银行 |
| 甘肃银行 |
| 北京农商银行 |
| 上海农商银行 |
| 保定银行 |
| 厦门银行 |
| 邮政储蓄银行 |
| 烟台银行 |
| 福建海峡银行股份有限公司 |
| 长春银行 |
| 镇江银行 |
| 宁波银行 |
| 济南银行 |
| 深圳银行 |
| 焦作银行 |
| 温州银行 |
| 广州银行 |
| 汉口银行 |
| 恒丰银行 |
| 齐齐哈尔银行 |
| 沈阳银行 |
| 洛阳银行 |
| 辽阳银行 |
| 大连银行 |
| 苏州银行 |
| 石家庄银行 |
| 东莞银行 |
| 金华银行 |
| 乌鲁木齐银行 |
| 绍兴银行 |
| 抚顺银行 |
| 临沂银行 |
| 宜昌银行 |
| 葫芦岛银行 |
| 天津银行 |
| 郑州银行 |
| 银川银行 |
| 宁夏银行 |
| 珠海银行 |
| 淄博银行 |
| 锦州银行股份有限公司 |
| 徽商银行股份有限公司 |
| 合肥银行 |
| 重庆银行 |
| 哈尔滨银行 |
| 贵阳银行 |
| 西安银行 |
| 无锡银行 |
| 丹东银行 |
| 兰州银行 |
| 江西银行 |
| 太原银行 |
| 青岛银行 |
| 吉林银行 |
| 南通银行 |
| 扬州银行 |
| 九江银行 |
| 日照银行 |
| 鞍山银行 |
| 秦皇岛银行 |
| 西宁银行 |
| 台州银行 |
| 盐城银行 |
| 长沙银行 |
| 潍坊银行 |
| 赣州银行 |
| 泉州银行 |
| 营口银行 |
| 昆明银行 |
| 阜新银行 |
| 常州银行 |
| 淮安银行 |
| 嘉兴银行股份有限公司 |
| 芜湖银行 |
| 廊坊银行股份有限公司 |
| 浙江泰隆商业银行 |
| 呼和浩特市城银行 |
| 湖州银行 |
| 马鞍山银行 |
| 南宁银行 |
| 包商银行 |
| 连云港银行 |
| 威海银行 |
| 淮北银行 |
| 攀枝花银行 |
| 安庆银行 |
| 绵阳银行 |
| 泸州银行 |
| 大同银行 |
| 三门峡城市信用社 |
| 湛江银行 |
| 张家口银行 |
| 桂林银行 |
| 大庆银行 |
| 靖江市长江城市信用社 |
| 徐州银行 |
| 柳州银行 |
| 四川天府银行 |
| 汇丰银行 |
| 东亚银行 |
| 江苏银行股份有限公司 |
| 邯郸市商业银行 |
| 重庆三峡银行 |
| 遂宁银行 |
| 晋中市商业银行股份有限公司 |
| 宁波通商银行 |
| 齐商银行 |
| 顺德农村商业银行 |
| 东营银行 |
| 江西农商银行 |
| 上海农村商业银行 |
| 昆山农信社 |
| 常熟市农村银行 |
| 深圳市农村信用合作社联合社 |
| 广州市农村信用合作社联合社 |
| 杭州市萧山区农村信用合作社联合社 |
| 南海市农村信用合作社联合社 |
| 顺德市农村信用合作社联合社 |
| 昆明市农村信用合作社联合社 |
| 湖北省农村信用社 |
| 武汉市农村信用合作社联合社 |
| 徐州市市郊农村信用合作社联合社 |
| 江阴市农村银行 |
| 重庆市农村信用合作社联合社 |
| 山东省市农村信用社 |
| 青岛农村信用社 |
| 东莞农村信用合作社联合社 |
| 张家港市农村银行 |
| 福建省农村信用社 |
| 厦门市农村信用合作社联合社 |
| 北京农村信用联社 |
| 天津市农村信用合作社联合社 |
| 宁波鄞州农村合作银行 |
| 佛山市三水区农村信用合作社联合社 |
| 成都市农村信用合作社联合社 |
| 沧州市农村信用合作社联合社 |
| 江苏省农村信用合作社联合社 |
| 江门市新会农村信用合作社联合社 |
| 高要市农村信用合作社联合社 |
| 佛山市禅城区农村信用社联合社 |
| 江苏吴江农村银行 |
| 吴江农商行 |
| 浙江省农村信用社联合社 |
| 江苏东吴农村银行 |
| 珠海市农村信用合作社联合社 |
| 中山农村信用合作社联合社 |
| 江苏太仓农村银行股份有限公司 |
| 临汾市尧都区信用合作社联合社 |
| 江苏武进农村银行股份有限公司 |
| 贵州省农村信用合作作联合社 |
| 江苏锡州农村银行股份有限公司 |
| 湖南省农村信用社联合社 |
| 江西农信联合社 |
| 河南省农村信用社联合社 |
| 河北省农村信用社联合社 |
| 陕西省农村信用社联合社 |
| 广西农村信用社联合社 |
| 新疆维吾尔自治区农村信用社联合 |
| 吉林农信联合社 |
| 黄河农村银行 |
| 安徽省农村信用社联合社 |
| 海南省农村信用社联合社 |
| 青海省农村信用社联合社 |
| 广东省农村信用社联合社 |
| 内蒙古自治区农村信用社联合式 |
| 四川省农村信用社联合社 |
| 甘肃省农村信用社联合社 |
| 辽宁省农村信用社联合社 |
| 山西省农村信用社联合社 |
| 天津滨海农村银行 |
| 黑龙江省农村信用社联合社 |
| 武汉农村银行 |
| 江南农村银行 |
| 大邑交银兴民村镇银行 |
| 湖北嘉鱼吴江村镇银行 |
| 青岛即墨北农商村镇银行 |
| 湖北仙桃北农商村镇银行 |
| 海口苏南村镇银行 |
| 双流诚民村镇银行 |
| 宣汉诚民村镇银行 |
| 福建建瓯石狮村镇银行 |
| 恩施常农商村镇银行 |
| 咸丰常农商村镇银行 |
| 浙江长兴联合村镇银行 |
| 浙江平湖工银村镇银行 |
| 重庆璧山工银村镇银行 |
| 北京密云汇丰村镇银行 |
| 湖北随州曾都汇丰村镇银行 |
| 重庆大足汇丰村镇银行有限责任公司 |
| 江苏沭阳东吴村镇银行 |
| 重庆农村银行 |
| 方大村镇银行 |
| 深圳龙岗鼎业村镇银行 |
| 中山小榄村镇银行 |
| 江苏邗江民泰村镇银行 |
| 安徽当涂新华村镇银行 |
| 广州番禹新华村镇银行 |
| 沂水中银富登村镇银行 |
| 京山中银富登村镇银行 |
| 蕲春中银富登村镇银行 |
| 北京顺义银座村镇银行 |
| 江西赣州银座村镇银行 |
| 深圳福田银座村镇银行 |
| 北京怀柔融兴村镇银行 |
| 深圳宝安融兴村镇银行 |
| 南阳村镇银行 |
| 成都银行 |
| 富滇银行 |
| 广东南粤银行 |
| 湖北省农村信用社联合社 |


##### <span id="342-----">3.4.2 银行名称列表-USDT代付</span>

| 标准名称   |
| ---- |
| ERC20 |
| TRC20 |


##### <span id="343-----">3.4.3 银行名称列表-越南代付</span>

| 标准名称   |
| ---- |
|   ABBANK   |
|   ACB   |
|   AGRIBANK   |
|   BACABANK   |
|   BAOVIETBANK   |
|   BIDV   |
|   DONGABANK   |
|   EXIMBANK   |
|   HDBANK   |
|   KIENLONGBANK   |
|   MB   |
|   MSB   |
|   NAMABANK   |
|   OCEANBANK   |
|   PGBANK   |
|   PVCOMBANK   |
|   SAIGONBANK   |
|   SACOMBANK   |
|   SEABANK   |
|   SHB   |
|   TCB   |
|   TPBANK   |
|   VCB   |
|   VIB   |
|   VIETBANK   |
|   VIETINBANK   |
|   VPBANK   |

##### <span id="344-----">3.4.4 银行名称列表-韩国代付</span>
| 标准名称   |
| ---- |
|   한국산업은행   |
|   중소기업은행   |
|   국민은행   |
|   수협은행   |
|   농협(중앙회)   |
|   농협(지역)   |
|   우리은행   |
|   SC 은행   |
|   한국씨티은행   |
|   대구은행   |
|   부산은행   |
|   광주은행   |
|   제주은행   |
|   전북은행   |
|   경남은행   |
|   새마을금고   |
|   신협   |
|   상호저축은행   |
|   우체국   |
|   KEB 하나은행   |
|   신한은행   |
|   카카오뱅크   |
|   케이뱅크   |
|   토스뱅크   |

##### <span id="345-----">3.4.5 银行名称列表-菲律宾代付</span>
| 标准名称   |   全称   |
| ---- | ---- |
|     GCash   |     GCash   |
|     AUB     |     Asia United Bank Corporation     |
|     BDO     |     Banco de Oro Unibank INC     |
|     BOC     |     Bank of Commerce     |
|     BPI     |     Bank of the Philippine Island      |
|     Chinabank     |     China Banking Corporation     |
|     DBP     |     Development Bank of the Philippines     |
|     EastWest     |     East West Banking Corporation     |
|     LBP     |     Land Bank of the Philippines     |
|     Metrobank     |     Metropolitan Bank and Trust Co.     |
|     Netbank     |     Netbank (Community Rural Bank of Romblon)    |
|     PNB     |     Philippine National Bank     |
|     UBP     |     UnionBank of the Philippines     |


##### <span id="346-----">3.4.6 银行名称列表-印度代付</span>
标准名称为银行英文简称，分行所传内容请参考印度金融系统代码 (IFSC)。


##### <span id="347-----">3.4.7 银行名称列表-日本代付</span>
| 标准名称   |
| ---- |
|    イオン銀行    |
|    秋田銀行    |
|    青森銀行    |
|    足利銀行    |
|    阿波銀行    |
|    岩手銀行    |
|    池田泉州銀行    |
|    鳥取銀行    |
|    佐賀銀行    |
|    京都銀行    |
|    Bank of Okinawa, Ltd    |
|    七十七銀行    |
|    紐約梅隆銀行信託銀行    |
|    千葉銀行    |
|    中國銀行    |
|    筑邦銀行    |
|    千葉興業銀行    |
|    第四銀行    |
|    大和next銀行    |
|    十八銀行    |
|    福岡銀行    |
|    福井銀行    |
|    GMOあおぞらネット銀行    |
|    Gunma Bank, Ltd    |
|    八十二銀行    |
|    橫濱銀行    |
|    北越銀行    |
|    肥後銀行    |
|    廣島銀行    |
|    北海道銀行    |
|    北國銀行    |
|    北都銀行    |
|    百十四銀行    |
|    百五銀行    |
|    伊予銀行    |
|    auじぶん銀行    |
|    常陽銀行    |
|    郵政銀行    |
|    日証金信託銀行    |
|    十六銀行    |
|    鹿兒島銀行    |
|    筑波銀行    |
|    北九州銀行    |
|    紀陽銀行    |
|    SBI新生銀行    |
|    陸奧銀行    |
|    瑞穗銀行    |
|    三重銀行    |
|    宮崎銀行    |
|    三菱UFJ信托銀行    |
|    日本Master Trust信託銀行    |
|    武藏野銀行    |
|    三菱UFJ銀行    |
|    名古屋銀行    |
|    南都銀行    |
|    青空銀行    |
|    農中信託銀行    |
|    西日本シティ銀行    |
|    野村信託銀行    |
|    住信SBIネット銀行    |
|    大垣共立銀行    |
|    大分銀行    |
|    関西みらい銀行    |
|    ORIX信託銀行    |
|    PayPay銀行株式会社    |
|    樂天銀行    |
|    北陸銀行    |
|    りそな銀行    |
|    琉球銀行    |
|    埼玉里索那銀行    |
|    セブン銀行    |
|    山陰合同銀行    |
|    SBJ銀行    |
|    靜岡銀行     |
|    新生信託銀行    |
|    親和銀行    |
|    滋賀銀行    |
|    四國銀行    |
|    信金信託銀行    |
|    三井住友銀行    |
|    清水銀行    |
|    荘內銀行    |
|    ソニー銀行    |
|    Suruga銀行    |
|    State Street信託銀行    |
|    三井住友信託銀行     |
|    但馬銀行    |
|    東北銀行    |
|    東邦銀行    |
|    东京都民銀行    |
|    富山銀行    |
|    山形銀行    |
|    山梨中央銀行    |
|    山口銀行    |
|    瑞穗信託銀行    |
        
##### <span id="348-----">3.4.8 银行名称列表-泰国代付</span>
| 标准名称   |   全称   |
| ---- | ---- |
| SCB	|    SIAM COMMERCIAL BANK PUBLIC COMPANY LTD   | 
| BAY	|    BANK OF AYUDHYA PUBLIC COMPANY LTD   | 
| KTB	|    KRUNG THAI BANK PUBLIC COMPANY LTD   | 
| BBL	|    BANGKOK BANK PUBLIC COMPANY LTD   | 
| GSB	|    GOVERNMENT SAVINGS BANK   | 
| TTB	|    TMBTHANACHART BANK PUBLIC COMPANY LIMITED   | 
| BAAC	|    BANK FOR AGRICULTURE AND AGRICULTURAL COOPERATIVES   | 
| KKP	|    KIATNAKIN PHATRA BANK PUBLIC COMPANY LIMITED   | 
| KBANK	|    KASIKORNBANK PUBLIC COMPANY LIMITED   | 
| ANZ	|    AUSTRALIA AND NEW ZEALAND BANKING GROUP LIMITED   | 
| BOC	|    BANK OF CHINA (THAI) PUBLIC COMPANY LIMITED   | 
| CIMBTB|    CIMB THAI BANK PUBLIC COMPANY LIMITED   | 
| CITIB	|    CITIBANK THAILAND   | 
| EXIM	|    EXPORT-IMPORT BANK OF THAILAND   | 
| GHB	|    THE GOVERNMENT HOUSING BANK   | 
| ICBCB	|    ICBC BANK   | 
| IBT	|    ISLAMIC BANK OF THAILAND   | 
| LHB	|    LH BANK   | 
| SMEDB	|    SME DEVELOPMENT BANK   | 
| SCTB	|    STANDARD CHARTERED BANK (THAI) PUBLIC COMPANY LIMITED   | 
| SMTB	|    SUMITOMO MITSUI BANKING CORPORATION   | 
| TCRB	|    THAI CREDIT RETAIL BANK   | 
| TISCO	|    TISCO BANK PUBLIC COMPANY LIMITED   | 
| UOB	|    UNITED OVERSEAS BANK (THAI) PUBLIC COMPANY LIMITED   | 

##### <span id="349-----">3.4.9 银行名称列表-巴西代付</span>
| 标准名称   |
| ---- |
|    PIX    |

##### <span id="3410-----">3.4.10 银行名称列表-印度尼西亚代付</span>
| 标准名称   |
| ---- |
|Bank Agroniaga|
| Amar|
| Bank ANZ|
| Bank Artha Graha|
| Bank Jawa Barat|
| Bank Jawa Barat Syariah|
| Bank BNI Syariah|
| Bank BNP Paribas|
| Bank Bukopin|
| Bank Bumi Arta|
| Bank Capital Indonesia|
| Bank BCA|
| Bank BCA Syariah|
| Bank CIMB Niaga|
| Bank Cimb Niaga Syariah|
| Bank Commonwealth|
| Bank Danamon|
| Bank Danamon Syariah|
| BANK DBS|
| Bank DKI|
| Bank DKI Syariah|
| Bank Ganesha|
| Bank Hana|
| Allo Bank|
| Bank ICBC Indonesia|
| Bank Ina Perdana|
| Bank Jasa Jakarta|
| Bank J Trust Bank|
| Bank Mandiri|
| Bank Maspion|
| Bank Mayapada|
| Bank Maybank|
| Bank Maybank Syariah Indon|
| Bank Mayora|
| Bank Mega|
| Bank Mestika|
| Bank Mizuho Indonesia|
| Bank MNC|
| Bank Muamalat|
| Bank Nobubank|
| Bank BNI|
| Bank Nusantara Parahyangan|
| Bank OCBC NISP|
| Bank OCBC NISP Syariah|
| Bank of America|
| Bank of China Limited|
| Bank of India|
| Bank of Tokyo Mitsubishi U|
| Bank Panin|
| Bank Panin Syariah|
| Bank Permata|
| Bank Permata Syariah|
| Bank QNB|
| Bank Rabobank Internationa|
| Bank BRI|
| Bank Resona Perdania|
| Bank BCA Digital|
| Bank Sahabat Sampoerna|
| Bank SBI Indonesia|
| Bank Shinhan|
| Bank Sinarmas|
| Bank Sinarmas Syariah|
| Bank BRI Syariah|
| Bank Bukopin Syariah|
| Bank Syariah Mandiri|
| Bank MEGA Syariah|
| Bank BTN|
| Bank BTN Syariah|
| Bank BTPN|
| Bank UOB Buana|
| Bank Victoria|
| Bank Victoria Syariah|
| Bank Woori Indonesia|
| Bank Woori Saudara|
| Bank Yudha Bhakti|
| Bank BPD Aceh|
| Bank BPD Aceh Syariah|
| Bank BPD Banten|
| Bank BPD Bengkulu|
| Bank BPD Yogyakarta|
| Bank BPD Jambi|
| Bank BPD Jambi Syariah|
| Bank jateng|
| Bank BPD Jateng Syariah|
| Bank BPD Jatim|
| Bank BPD Jatim Syariah|
| Bank Kalbar|
| Bank Kalbar Syariah|
| Bank Kalsel|
| Bank Kalsel Syariah|
| Bank Kalteng|
| Bank Kaltim|
| Bank Kaltim Syariah|
| Bank Lampung|
| Bank Maluku|
| Bank NTB|
| Bank NTB Syariah|
| Bank NTT|
| Bank Papua|
| Bank RIAU Kepri|
| Bank RIAU Kepri Syariah|
| Bank Sulteng|
| Bank Sultra|
| Bank Sulselbar|
| Bank Sulselbar Syariah|
| Bank SulutGO|
| Bank Sumsel Babel|
| Bank Sumsel Babel Syariah|
| Bank Sumut|
| Bank Sumut Syariah|
| Centratama Nasional Bank|
| Bank CCB|
| Bank Citibank|
| Bank HSBC Syariah|
| Bank HSBC|
| Bank Mandiri Taspen|
| Standard Chartered Bank|
| Bank Syariah Indonesia|
| Dana|
| Gopay|
| LinkAja|
| OVO|
| ShopeePay|
|DANAWD|
| SeaBank Indonesia|
| Bank BNI|
| Bank Jago|
| Bank BJB|
| Bank BPD DIY|

##### <span id="3411-----">3.4.11 银行名称列表-孟加拉代付</span>
| 标准名称   |
| ---- |
|Agranl Bank|
|AB Bank Limited|
|Bangladesh Development Bank|
|The Premler Bank PLC|
|BKash|
|BRAC Bank|
|Community Bank Bangladesh|
|CB BANK|
|City Bank|
|DHAKA BANK LIMTED|
|Eastern Bank Ltd.|
|First Security lslami Bank PLC|
|IFIC BANK PLC|
|Midland Bank Limlted|
|Mutual Trust Bank Ltd|
|Nagad|
|NRBC Bank PLC|
|Pubali Bank PLC|
|PVcomBank|
|Rajshahi Krishi Unnayan Bank|
|Rocket|
|Southeast Bank Limited|
|Social lslami Bank PLC|
|Sonali Bank|
|UPay|
|YOLO digital banking|
