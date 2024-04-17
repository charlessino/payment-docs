# FuGuo Payment Gateway Documents

- Contents
  + [1 Overview](#1-----)
    - [1.1 Interface Usage](#11-----)
    - [1.2 Interface Application](#12-----)
    - [1.3 Header Parameters](#13-----)
    - [1.4 Signature](#14-----)
      * [1.4.1 Signature example](#141-----)
      * [1.4.2 Return parameter signature](#142-----)
    - [1.5 Callback mechanism](#15-----)
  + [2 List of interfaces](#2-----)
    + [2.1 Recharge](#21-----)
      + [2.1.1 Incoming parameters](#211-----)
      + [2.1.2 Return parameters](#212-----)
      + [2.1.3 Asynchronous callback notification parameters](#213-----)
      + [2.1.4 Example request parameters](#214-----)
      + [2.1.5 Example of return parameters](#215-----)
      + [2.1.6 Example of asynchronous callback notification parameters](#216-----)
    + [2.2 Withtraw](#21-----)
      + [2.2.1 Incoming parameters](#221-----)
      + [2.2.2 Return parameters](#222-----)
      + [2.2.3 Asynchronous callback notification parameters](#223-----)
      + [2.2.4 Example request parameters](#224-----)
      + [2.2.5 Example of return parameters](#225-----)
      + [2.2.6 Example of asynchronous callback notification parameters](#226-----)
    + [2.3 Recharge order inquiry](#23-----)
      + [2.3.1 Incoming parameters](#231-----)
      + [2.3.2 Return parameters](#232-----)
      + [2.3.3 Data format schema](#233-----)
      + [2.3.4 Example of request parameters](#234-----)
      + [2.3.5 Example of return parameters](#235-----)
    + [2.4 Withdraw order inquiry](#24-----)
      + [2.4.1 Incoming parameters](#241-----)
      + [2.4.2 Return parameters](#242-----)
      + [2.4.3 Data format schema](#243-----)
      + [2.4.4 Example request parameters](#244-----)
      + [2.4.5 Return parameter example](#245-----)
    + [2.5 Balance inquiry](#25-----)
      + [2.5.1 Incoming parameters](#251-----)
      + [2.5.2 Return parameters](#252-----)
      + [2.5.3 Example request parameters](#253-----)
      + [2.5.4 Example of return parameters](#254-----)
  + [3 Attachments](#3-----)
    + [3.1 Channel List](#31-----)
    + [3.2 List of currencies](#32-----)
    + [3.3 List of Bank Names - Recharge](#33-----)
    	+ [3.3.1 List of Bank Names - Vietnam Recharge](#331-----)
    + [3.4 List of Bank Names-Withdraw](#34-----)
    	+ [3.4.1 List of Bank Names-China Withdraw](#341-----)
    	+ [3.4.2 List of Bank Names - USDT Withdraw](#342-----)
    	+ [3.4.3 List of Bank Names - Vietnam Withdraw](#343-----)


### <span id="1-----">1 Overview</span>

#### <span id="11-----">1.1 Interface Usage</span>

This interface is used to access the FuGuo Payments system. This interface is pure Restful style , the incoming parameters and return parameters are all in JSON format.

#### <span id="12-----">1.2 Interface Application</span>

Please contact customer service to apply for the interface, after the application is approved you can get the following information:

    1. apiAddress
    2. appId
    3. key

#### <span id="13-----">1.3 Header Parameters</span>

Request method: POST

| Parameter       | Required | Type/Parameter Value      | Description         |
| ------------ | ---- | ---------------- | ------------ |
| Content-Type | Yes   | Type/json | Request parameter type |

#### <span id="14-----">1.4 Signature</span>

  1. Arrange the parameter names of all incoming parameters (except sign) in dictionary order, note that parameters with null values do not need to be incoming
  2. Construct it as a link parameter
  3. Add the key at the end (please contact customer service for the key).
  4. encrypt the string with md5
  5. Convert to lowercase

##### <span id="141-----">1.4.1 Signature example</span>

 - 1.Incoming parameters

```json
{
    "appId": "B32D954CC4E25491F99EFE42DF1CCBBF",
    "channelId": 1,
    "currency": "KRW",
    "actionValue": 1200.00,
    "outOrderId": "ESP837647136232",
    "outTips": "test"
}
```

 - 2.Arrange the parameter names in dictionary order

```json
{
    "actionValue": 1200.00,
    "appId": "B32D954CC4E25491F99EFE42DF1CCBBF",
    "channelId": 1,
    "currency": "KRW",
    "outOrderId": "ESP837647136232",
    "outTips": "test"
}
```

 - 3.Construct it as a link parameter

actionValue=1200.00&appId=B32D954CC4E25491F99EFE42DF1CCBBF&channelId=1&currency=KRW&outOrderId=ESP837647136232&outTips=test

 - 4.Add the key at the end（e.g. key为aaabbbccc）

actionValue=1200.00&appId=B32D954CC4E25491F99EFE42DF1CCBBF&channelId=1&currency=KRW&outOrderId=ESP837647136232&outTips=test&key=aaabbbccc

 - 5.Convert to lowercase md5，hich is sign

61695d4fb053b3c769205820170b9dea

##### <span id="142-----">1.4.2 Return parameter signature</span>

每次请求接口，接口返回时会带着一个sign，这个sign的签名规则为：md5(nonceStr + key)

e.g.：<br>
nonceStr = 123456<br>
key = aaabbbccc<br>
md5("123456aaabbbccc") = 4118e6a1a1d43665ba1b77f49759b130<br>

4118e6a1a1d43665ba1b77f49759b130 就是返回参数的签名

请注意，如果不传nonceStr参数，则不会返回这个sign

#### <span id="15-----">1.5 Callback mechanism</span>

  1. 代收和代付订单在收到状态更新之后，会立即回调商户指定的回调地址
  2. 如果收到商户返回success字样表示回调成功
  3. 如果未收到success字样，系统会每隔一分钟尝试再次回调，最多10次，直到收到success时停止
  4. 10次之后仍然未收到success字样，系统也不再发送回调信息
  5. 如您需要再次回调，欢迎随时联系客服



### <span id="2-----">2 List of interfaces</span>

​    

#### <span id="21-----">2.1 Recharge</span>

请求地址：`{apiAddress}/recharge`<br>
Header：Content-Type: application/json;charset=utf-8

##### <span id="211-----">2.1.1 Incoming parameters</span>

| Parameter      | Required | Type    | 字段长度 | Example     | Description                     |
| ----------- | ---- | ------- | -------- | -------- | ------------------------ |
| appId       | Yes   | string  | 32       |          | appId                   |
| channelId   | Yes   | int     | 5        | 1        | [Channel List](#31-----) |
| currency     | Yes   | string | 10    | KRW  | [List of currencies](#32-----)       |
| actionValue | Yes   | decimal | 18, 2    | 2100.00  | 申请代收的金额 (数字货币允许有小数，法币仅允许整数，就算是整数也需格式化为2位小数以便统一验签规则)       |
| accountName | Yes   | string | 100    | 张三  | 付款人姓名，姓名中不可包含数字，且不可传空值       |
| cellphone |    | string | 100    | 01034388769  | 手机号，韩国必传，其他可不传       |
| callbackUrl  |      | string  | 512      |          | 商户回调地址             |
| returnUrl  |      | string  | 512      |          | 支付完成后，商户页面返回地址             |
| outOrderId  |      | string  | 100      |          | 商户订单号             |
| outTips     |      | string  | 100      | 测试订单 | 商户备注               |
| returnType     |      | int  | 1      | 1 | 返回类型 1=充值链接 2=银行、卡号、户名的文本信息。默认为1               |
| nonceStr     |      | string  | 100      | 123456 | 随机数，用于获得返回参数签名，可不传               |
| param1     |      | string  | 100      |  | 越南通道ID5，则必传[银行名称列表-越南代收](#331-----)               |
| param2     |      | string  | 100      |  | 预留参数2，可不传               |
| param3     |      | string  | 100      |  | 预留参数3，可不传               |
| sign        | Yes   | string  | 32       |          | [Signature](#14-----)             |

##### <span id="212-----">2.1.2 Return parameters</span>

| Parameter | Type   | 字段长度 | Example    | Description                                         |
| ------ | ------ | -------- | ------- | -------------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| transactionId    | string | 100      |    RC_10086     | 交易流水号 |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |    | [返回参数签名](#142-----)      |
| url    | string | 255      |         | 用于跳转至支付页面的链接，returnType=1时返回此项 |
| data    | array |       |         | returnType=2时返回此项，包含以下子项：<br>bankName: 银行名称<br>branchName: 支行名称<br>accountNumber: 银行卡号<br>accountOwner: 户主姓名<br>amount: 金额 |

##### <span id="213-----">2.1.3 Asynchronous callback notification parameters</span>
收到回调时请返回success字样，详情参考[回调机制](#15-----)

| Parameter | Type   | 字段长度 | Example    | Description                                         |
| ------ | ------ | -------- | ------- | -------------------------------------------- |
| transactionId    | string | 100      |    RC_10086     | 交易流水号 |
| outOrderId | string    | 100        |        | 商户订单号                      |
| outTips    | string  | 100      | 测试订单 | 商户备注 |
| currency    | string | 10    | KRW  | [List of currencies](#32-----)  |
| actionValue    | decimal | 18, 2    | 2100.00  | 实际代收金额 (就算是没有小数的货币，也会被格式化为2位小数)      |
| status    | int | 1      | 1 | 1=代收成功 0=代收失败      |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |  | 除了sign之外其他所有参数都需参与签名，同请求时的[签名](#14-----)规则      |

##### <span id="214-----">2.1.4 Example request parameters</span>

 - Incoming parameters

```json
{
    "appId": "B32D954CC4E25491F9UIETG3CCBBF",
    "channelId": 1,
    "currency": "KRW",
    "actionValue": 2000.00,
    "accountName": "张三",
    "callbackUrl": "https://aaa.bbb.ccc/port1/withdraw",
    "outOrderId": "WE8681762354832",
    "outTips": "测试订单",
    "nonceStr": "123456",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```
##### <span id="215-----">2.1.5 Example of return parameters</span>

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

##### <span id="216-----">2.1.6 Example of asynchronous callback notification parameters</span>

 - 代收成功

```json
{
    "transactionId": "RC_10086",
    "outOrderId": "8986327638746",
    "outTips": "用户申请代收",
    "currency": "KRW",
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
    "currency": "KRW",
    "actionValue": 2500.00,
    "status": 0,
    "msg": "通道维护暂时关闭",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

#### <span id="22-----">2.2 Withtraw</span>

请求地址：`{apiAddress}/withdraw`<br>
Header：Content-Type: application/json;charset=utf-8

##### <span id="221-----">2.2.1 Incoming parameters</span>

| Parameter    | Required | Type     | 字段长度 | Example | Description                                          |
| --------- | ---- | -------- | -------- | ---- | --------------------------------------------- |
| appId     | Yes   | string   | 32       |      | appId                                        |
| channelId | Yes   | int | 5       |   1   | [Channel List](#31-----)            |
| currency     | Yes   | string | 10    | KRW  | [List of currencies](#32-----)        |
| actionValue | Yes   | decimal | 18, 2    | 2100.00  | 申请代付的金额 (数字货币允许有小数，法币仅允许整数，就算是整数也需格式化为2位小数以便统一验签规则)       |
| cardNumber      | Yes   | string   | 100        | 982268716  | 卡号（账号）         |
| bankName    |  Yes  | string      | 100        | 中国建设银行   | [银行名称列表-代付](#34-----)   |
| branchName      |    | string   | 100        | 上海分行  | 分支行名称         |
| ownerName      |  Yes  | string   | 100        | 张三  | 户主姓名，姓名中不可包含数字         |
| callbackUrl  |      | string  | 512      |          | 商户回调地址             |
| outOrderId  |      | string  | 100      |          | 商户订单号             |
| outTips     |      | string  | 100      | 测试订单 | 商户备注               |
| nonceStr     |      | string  | 100      | 123456 | 随机数，用于获得返回参数签名，可不传               |
| sign      | Yes   | string   | 32       |      | [签名](#14-----)                             |

##### <span id="222-----">2.2.2 Return parameters</span>

| Parameter     | Type   | 字段长度 | Example           | Description                                      |
| ---------- | ------ | -------- | -------------- | ----------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| transactionId    | int | 10      |         | 交易流水号 |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |    | [返回参数签名](#142-----)      |

##### <span id="223-----">2.2.3 Asynchronous callback notification parameters</span>
收到回调时请返回success字样，详情参考[回调机制](#15-----)

| Parameter | Type   | 字段长度 | Example    | Description                                         |
| ------ | ------ | -------- | ------- | -------------------------------------------- |
| transactionId    | string | 100      |    RC_10086     | 交易流水号 |
| outOrderId | string    | 100        |        | 商户订单号                      |
| outTips    | string  | 100      | 测试订单 | 商户备注 |
| currency    | string | 10    | KRW  | [List of currencies](#32-----)  |
| actionValue    | decimal | 18, 2    | 2100.00  | 实际代付金额 (就算是没有小数的货币，也会被格式化为2位小数)      |
| status    | int | 1      | 1 | 1=代付成功 0=代付失败      |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |  | 除了sign之外其他所有参数都需参与签名，同请求时的[签名](#14-----)规则     |

##### <span id="224-----">2.2.4 Example request parameters</span>

 - Incoming parameters

```json
{
    "appId": "B32D954CC4E25491F9UIETG3CCBBF",
    "channelId": 1,
    "currency": "KRW",
    "actionValue": 2000.00,
    "cardNumber": "938265716",
    "callbackUrl": "https://aaa.bbb.ccc/port1/withdraw",
    "outOrderId": "WE8681762354832",
    "outTips": "测试订单",
    "nonceStr": "123456",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

##### <span id="225-----">2.2.5 Example of return parameters</span>

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

##### <span id="226-----">2.2.6 Example of asynchronous callback notification parameters</span>

 - 代付成功

```json
{
    "transactionId": "RC_10086",
    "outOrderId": "8986327638746",
    "outTips": "这是一个代付订单",
    "currency": "KRW",
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
    "currency": "KRW",
    "actionValue": 4000.00,
    "status": 0,
    "msg": "通道维护暂时关闭",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```


#### <span id="23-----">2.3 Recharge order inquiry</span>

请求地址：`{apiAddress}/recharge-orders-query`<br>
Header：Content-Type: application/json;charset=utf-8

##### <span id="231-----">2.3.1 Incoming parameters</span>

| Parameter    | Required | Type     | 字段长度 | Example | Description                                          |
| --------- | ---- | -------- | -------- | ---- | --------------------------------------------- |
| appId     | Yes   | string   | 32       |      | 应用ID                                        |
| outOrderId     |     | string    | 100        |        | 商户订单号                      |
| dateTimeStart     |    | datetime | 19    | 2024-01-01 10:00:00  | 订单更新时间-起始值       |
| dateTimeEnd     |    | datetime | 19    | 2024-01-01 10:00:00  | 订单更新时间-结束值       |
| pageId      |    | int   | 10        | 3  | 每次最多返回200条记录<br>可使用本字段进行翻页<br>不传此参数默认为1         |
| orderBy    |    | string      | 4        | ASC   | 顺序<br>ASC=升序，DESC=降序<br>不传此参数默认DESC |
| nonceStr     |      | string  | 100      | 123456 | 随机数，用于获得返回参数签名，可不传               |
| sign      | Yes   | string   | 32       |      | [签名](#14-----)                         |

##### <span id="232-----">2.3.2 Return parameters</span>

| Parameter     | Type   | 字段长度 | Example           | Description                                      |
| ---------- | ------ | -------- | -------------- | ----------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| data    | array |       |         | 返回结果详情，格式参考以下示意 |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |    | [返回参数签名](#142-----)      |

##### <span id="233-----">2.3.3 Data format schema</span>

| Parameter    | Example           | Description     |
| ---------- | ------ | -------- |
| orderList |    transactionId: RC_98261876 (交易流水号)<br>currency: KRW (货币)<br>channelId: 15 (通道ID)<br>rechargeRate: 0.01 (手续费率)<br>actionValue: 3000.00 (实际代收金额)<br>chargeValue: 30.00 (手续费)<br>actualValue: 2970.00 (实际记账金额)<br>accountName: 张三 (付款人姓名)<br>status: 1 (状态值 1=成功 0=失败 2=处理中)<br>statusName: 成功 (状态名)<br>outOrderId: 98227863223 (商户订单号)<br>outTips: 测试的订单 (商户备注)<br>lastUpdatedTime: 2024-02-01 12:15:33 (订单更新时间)<br>createTime: 2024-02-01 09:31:26 (订单生成时间)   | 订单详情以二维数组方式排列                      |
| currentPage |    1    | 当前页码，默认为1<br>每页最多200条记录                      |
| totalPages |    5    | 当前搜索结果可以翻页的总页码<br>例如5表示总共有5页<br>可以在传参时使用pageId翻页     |
| totalRecords |    350    | 当前搜索结果的总纪录数                      |

##### <span id="234-----">2.3.4 Example request parameters</span>

 - Incoming parameters

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

##### <span id="235-----">2.3.5 Example of return parameters</span>

 - 返回参数

```json
{
	"result": 1,
	"data": {
		"orderList": [{
			"transactionId": "RC_17",
			"currency": "KRW",
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
			"currency": "KRW",
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



#### <span id="24-----">2.4 Withdraw order inquiry</span>

请求地址：`{apiAddress}/withdraw-orders-query`<br>
Header：Content-Type: application/json;charset=utf-8

##### <span id="241-----">2.4.1 Incoming parameters</span>

| Parameter    | Required | Type     | 字段长度 | Example | Description                                          |
| --------- | ---- | -------- | -------- | ---- | --------------------------------------------- |
| appId     | Yes   | string   | 32       |      | 应用ID                                        |
| outOrderId     |     | string    | 100        |        | 商户订单号                      |
| dateTimeStart     |    | datetime | 19    | 2024-01-01 10:00:00  | 订单更新时间-起始值       |
| dateTimeEnd     |    | datetime | 19    | 2024-01-01 10:00:00  | 订单更新时间-结束值       |
| pageId      |    | int   | 10        | 3  | 每次最多返回200条记录<br>可使用本字段进行翻页<br>不传此参数默认为1         |
| orderBy    |    | string      | 4        | ASC   | 顺序<br>ASC=升序，DESC=降序<br>不传此参数默认DESC |
| nonceStr     |      | string  | 100      | 123456 | 随机数，用于获得返回参数签名，可不传               |
| sign      | Yes   | string   | 32       |      | [签名](#14-----)                     |

##### <span id="242-----">2.4.2 Return parameters</span>

| Parameter     | Type   | 字段长度 | Example           | Description                                      |
| ---------- | ------ | -------- | -------------- | ----------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| data    | array |       |         | 返回结果详情，格式参考以下示意 |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |    | [返回参数签名](#142-----)      |

##### <span id="243-----">2.4.3 Data format schema</span>

| Parameter    | Example           | Description     |
| ---------- | ------ | -------- |
| orderList |    transactionId: WD_98261876 (交易流水号)<br>currency: KRW (货币)<br>channelId: 15 (通道ID)<br>withdrawRate: 0.01 (手续费率)<br>withdrawFixValue: 3.00 (代付固定手续费)<br>actionValue: 3000.00 (实际代付金额)<br>chargeValue: 33.00 (手续费)<br>actualValue: 3033.00 (实际记账金额)<br>bankName: 工商银行 (银行名称)<br>branchName: 广州市分行 (分支行名称)<br>cardNumber: 982268716 (卡号)<br>ownerName: 张三 (户主姓名)<br>status: 1 (状态值 1=成功 0=失败 2=处理中)<br>statusName: 成功 (状态名)<br>outOrderId: 98227863223 (商户订单号)<br>outTips: 测试的订单 (商户备注)<br>lastUpdatedTime: 2024-02-01 12:15:33 (订单更新时间)<br>createTime: 2024-02-01 09:31:26 (订单生成时间)   | 订单详情以二维数组方式排列                      |
| currentPage |    1    | 当前页码，默认为1<br>每页最多200条记录                      |
| totalPages |    5    | 当前搜索结果可以翻页的总页码<br>例如5表示总共有5页<br>可以在传参时使用pageId翻页     |
| totalRecords |    350    | 当前搜索结果的总纪录数                      |

##### <span id="244-----">2.4.4 Example request parameters</span>

 - Incoming parameters

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

##### <span id="245-----">2.4.5 Example of return parameters</span>

 - 返回参数

```json
{
	"result": 1,
	"data": {
		"orderList": [{
			"transactionId": "WD_23",
			"currency": "KRW",
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
			"currency": "KRW",
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



#### <span id="25-----">2.5 Balance inquiry</span>

请求地址：`{apiAddress}/balance`<br>
Header：Content-Type: application/json;charset=utf-8

##### <span id="251-----">2.5.1 Incoming parameters</span>

| Parameter    | Required | Type     | 字段长度 | Example | Description                                          |
| --------- | ---- | -------- | -------- | ---- | --------------------------------------------- |
| appId     | Yes   | string   | 32       |      | 应用ID                                        |
| nonceStr     |      | string  | 100      | 123456 | 随机数，用于获得返回参数签名，可不传               |
| sign      | Yes   | string   | 32       |      | [签名](#14-----)                        |

##### <span id="252-----">2.5.2 Return parameters</span>

| Parameter     | Type   | 字段长度 | Example           | Description                                      |
| ---------- | ------ | -------- | -------------- | ----------------------------------------- |
| result | int    | 1        | 1       | 调用结果，1=成功 0=失败                      |
| data    | array |       |     KRW: 6686.32 (人民币余额)<br>USDT: 927.92 (USDT余额)    | 以二维数组方式排列 |
| msg    | string | 200      | success | 如出错时，返回出错原因，成功时为success      |
| sign    | string | 32      |    | [返回参数签名](#142-----)      |

##### <span id="253-----">2.5.3 Example request parameters</span>

 - Incoming parameters

```json
{
    "appId": "B32D954CC4E25491F9UIETG3CCBBF",
    "nonceStr": "123456",
    "sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```

##### <span id="254-----">2.5.4 Example of return parameters</span>

 - 返回参数

```json
{
	"result": 1,
	"data": {
		"KRW": "6686.32",
		"USDT": "927.92"
	},
	"msg": "success",
    	"sign": "cbc0b11733b785b0317f1cc7d6f20fd8"
}
```





### <span id="3-----">3 Attachments</span>

#### <span id="31-----">3.1 Channel List</span>

请通过商户后台系统 > 信息概览 > 支付通道，获取通道ID及对应名称。

#### <span id="32-----">3.2 List of currencies</span>

| 参数   | 货币名称     |
| ---- | -------- |
| CNY    | Renminbi |
| USDT    | Tether |
| VND    | Vietnamese Dong |
| INR    | Indian Rupee |
| THB    | Thai Baht |
| IDR    | Indonesian Rupiah |
| BRL    | Brazilian Real |
| MXP    | Mexican Peso |
| JPY    | Japanese Yen |
| PHP    | Philippine Peso |
| KRW    | South Korean Won |

#### <span id="33-----">3.3 List of Bank Names - Recharge</span>

##### <span id="331-----">3.3.1 List of Bank Names - Vietnam Recharge</span>

| Standard name   |
| ---- |
| VP BANK |
| ACB BANK |
| BIDV BANK |
| VIETTIN BANK |
| MB BANK |
| EXIM BANK |
| SACOM |
| TECHCOM BANK |
| VIETCOM BANK |
| DONGA BANK |
| VIB BANK |
| MSB BANK |
| SHB BACNK |


#### <span id="34-----">3.4 List of Bank Names-Withdraw</span>

##### <span id="341-----">3.4.1 List of Bank Names-China Withdraw</span>

| Standard name   |
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


##### <span id="342-----">3.4.2 List of Bank Names - USDT Withdraw</span>

| Standard name   |
| ---- |
| ERC20 |
| TRC20 |


##### <span id="343-----">3.4.3 List of Bank Names - Vietnam Withdraw</span>

| Standard name   |
| ---- |
|   VP BANK   |
|   ACB BANK   |
|   BIDV BANK   |
|   VIETTIN BANK   |
|   MB BANK   |
|   VP BANK   |
|   TECHCOM BANK   |
|   VIETCOM BANK   |
|   DONGA BANK   |
|   VIB BANK   |
|   MSB BANK   |
|   SHB BACNK   |
|   EXIM BANK   |
|   IBK HCM   |
|   AGRI BANK   |
|   NHB HN   |
|   PVCOMBANK   |
|   OCEANBANK   |
|   ABBANK   |
|   NAMABANK   |
|   KIENLONGBANK   |
|   SEABANK   |
|   VIETCAPITAL BANK   |
|   SAIGONBANK   |
|   TPBANK   |
|   VIETBANK   |
|   PG BANK   |
|   VIET NAM   |
|   PBVN   |
|   WOORIBANK   |

