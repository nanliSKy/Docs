# 			钱包支付SDK接口文档说明



## 版本&更新记录

| 版本号  | 作者         | 日期       | 更新内容 |
| ------- | ------------ | ---------- | -------- |
| v.1.0.0 | bcbwalletPay | 2019-06-27 | 初始版本 |

---

---



## 功能说明

本文档提供钱包的SDK访问接口说明。IOS版本。



## 接口访问方式

API调用，返回的内容也是一个json串，里面会携带返回的状态 码以及一些返回的必要参数。

## 1.检查BCB钱包是否已被用户安装

### 1.1 方法原型

**+ (**BOOL**)isBCBWalletAppInstalled;

### 1.2 返回结果

**示例：返回结果-已安装**

```java
return YES;
```

**示例：返回结果-未安装**

```java
return NO;
```



## 2.获取当前BCB钱包SDK的版本号

### 2.1 方法原型

**+(NSString *)getApiVersion;**

### 2.2 返回结果

**示例：返回结果**

```java
return @"1.0.0";
```



## 3.链接支付接口

### 3.1 方法原型

**- (**void**)payOrder:(NSString *)orderStr fromScheme:(NSString *)schemeStr callback:(CompletionBlock)completionBlock;

**输入参数说明**

| 参数名    | 类型   | 必须 | 说明                                                         |
| --------- | ------ | ---- | ------------------------------------------------------------ |
| orderStr  | string | 是   | 支付链接（后端生成：https://testclt.cgs.cool/crowdfunding/api/business/wallet/St5iKQVxS5BXfSkMAkoCKHlIb3EGkT4X） |
| schemeStr | string | 是   | 调用支付的app注册在info.plist中的scheme，用于支付完成后跳转原APP |



### 3.2 返回结果

**示例：返回结果-正确时**

```java
{
    "code":0,
	"msg": "支付成功",
}
```

**示例：返回结果-错误时**

```java
{
    "code":1,
	"msg": "取消支付",
}
```



## 4.转账支付接口

### 4.1 方法原型

**-(void) transationCoinAddr:(NSString *)coinAddr toAddr:(NSString *)toAddr value:(NSString *)value note:(NSString *)note callback:(CompletionBlock)completionBlock;**

**参数字段说明**

| 字段名   | 类型   | 必须 | 说明                     |
| -------- | ------ | ---- | ------------------------ |
| coinAddr | string | 是   | 要转账币种的合约地址     |
| toAddr   | string | 是   | 转账到的目标地址         |
| value    | string | 是   | 转账的金额，例如"102.23" |
| note     | string | 否   | 转账的备注               |



### 4.2 返回结果

**示例：返回结果-正确时**

```java
{
    "code":0,
	"msg": "支付成功",
}
```

**示例：返回结果-错误时**

```java
{
    "code":1,
	"msg": "取消支付",
}
```



## 5.处理BCBWallet支付后跳回商户app携带的支付结果Url

### 5.1 方法原型

**-**(**void**)processOrderWithPaymentResult:(NSURL *)resultUrl standbyCallback:(CompletionBlock)completionBlock**;**

**参数字段说明**

| 字段名    | 类型   | 必须 | 说明                   |
| --------- | ------ | ---- | ---------------------- |
| resultUrl | string | 是   | AppDelegate中的openURL |



### 5.2 返回结果

**示例：返回结果-正确时**

```java
{
    "code":0,
	"msg": "支付成功",
}
```

**示例：返回结果-错误时**

```java
{
    "code":1,
	"msg": "取消支付",
}
```



## 6.待讨论

### 6.1 支付结果是否需要txhash,若需要，是否需要提供hash查询接口

### 6.2 是否提供当前BCBWallet版本与SDK版本是否兼容

