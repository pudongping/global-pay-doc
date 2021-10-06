# 快速开始

## 简介

支持国际版支付的 PHP SDK，目前**只支持支付宝国际版**。因目前支付宝跨境在线支付服务只支持 app、wap、web 和报关这四种，
本 SDK 提供了 app、wap、web 这三种跨境支付，[详见文档](https://global.alipay.com/docs/ac/legacy/legacydoc) 。

> 因本人所在单位是进行外贸交易，支付需要采用国际支付，故此本人写了 `global-pay` 这个 php 扩展包。后因公司业务需要，
> 需要集成蚂蚁花呗分期支付，因此此扩展包也支持蚂蚁花呗分期支付。为了好区分一般支付和花呗分期支付的不同，因此在文档介绍中
> 对二者分别做了介绍。另外注意：**花呗分期这个支付能力需要单独与支付宝客服沟通后才可以开通。**

## 特点

- 命名规范
- 隐藏开发者不需要关注的细枝末节
- 符合 PSR 规范，可以方便的与各种 PHP 框架集成

## 支持的支付方法

### 支付宝

- 电脑支付
- 手机网站支付
- APP 支付

method | 描述
:---: | :---:
web | 电脑支付
wap | 手机网站支付
app | APP 支付

## 支持的方法

> 所有网关均支持以下方法

- find(array|string $order)  
  **说明：** 查找订单接口  
  **参数：** `$order` 为 `string` 类型时，请传入系统订单号，对应跨境支付宝中的 `out_trade_no` 参数； `array` 类型时，参数请参考[支付宝境外订单单笔查询文档](https://global.alipay.com/docs/ac/global/single_trade_query_cn) 。    
  **返回：** 查询成功，返回 `Illuminate\Support\Collection` 实例，可以通过 `$collection->toArray()` 或者 `$collection->all()` 或者 `$collection->get('field')` 访问服务器返回的数据。

- refund(array $order)   
  **说明：** 退款接口  
  **参数：**  `$order` 数组格式，退款参数请参考[支付宝境外退款接口文档](https://global.alipay.com/docs/ac/global/forex_refund_cn) 。   
  **返回：** 退款成功，返回 `Illuminate\Support\Collection` 实例，可以通过 `$collection->toArray()` 或者 `$collection->all()` 或者 `$collection->get('field')` 访问服务器返回的数据。

- verify()  
  **说明：** 验证服务器返回数据是否合法  
  **返回：** 验证成功，返回 `Illuminate\Support\Collection` 实例，可以通过 `$collection->toArray()` 或者 `$collection->all()` 或者 `$collection->get('field')` 访问服务器返回的数据。
  

## 其他通用方法

- getExchangeRate()  
  **说明：** 获取汇率。详见[支付宝境外汇率查询接口](https://global.alipay.com/docs/ac/global/forex_rate_file_cn) 。  
  **返回：** 获取成功，返回 `Illuminate\Support\Collection` 实例，可以通过 `$collection->toArray()` 或者 `$collection->all()` 或者 `$collection->get('field')` 访问服务器返回的数据。  
  **注意：** 1、货币间的汇率会在北京时间每日 9：00 到 11:00 间变动一次；  2、汇率每日获取上限为 100 次。 （可能需要考虑通过缓存保存汇率，防止接口出现异常，因为本 SDK 没有做缓存处理）

- getHbFqCost(float $totalAmount, bool $isShowAll = false, bool $isSellerPercent = false)  
  **说明：** 获取花呗分期计费情况  
  **参数：** `$totalAmount` 为分期的本金，`$isShowAll` 为是否显示每一期的还款数，`$isSellerPercent` 为 `true` 表示商家承担全部手续费，为 `false` 表示用户承担全部手续费。  
  **返回：** 获取成功，返回 `Illuminate\Support\Collection` 实例，可以通过 `$collection->toArray()` 或者 `$collection->all()` 或者 `$collection->get('field')` 访问服务器返回的数据。

返回参数说明

参数 | 含义
--- | :---:
nper | 期数
total_amount | 本金
total_charge | 总手续费
rate | 利率
per_charge | 每期手续费
per_amount | 每期本金
per_total_amount | 每期总费用
refund_list | 还款列表
refund_list.nper | 第几期
refund_list.charge | 当前期数所需要支付的手续费
refund_list.amount | 当前期数所需要支付的本金数
refund_list.current_total_amount | 当前期数所需要支付的总费用
