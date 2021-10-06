# 验签

## 同步验签示例

```php

    /**
     * 同步验签
     */
    public function return()
    {
        $data = GlobalPay::alipay($this->config)->verify();
    }

```

## 异步验签示例

```php

    /**
     * 异步验签
     *
     * @return mixed
     */
    public function notify()
    {
        $globalPay = GlobalPay::alipay($this->config);

        try {

            $data = $globalPay->verify();  // 验签

            // 建议必须对以下几个参数进行业务逻辑验证
            $outTradeNo = $data->get('out_trade_no');  // 商户需要验证该通知数据中的 out_trade_no 是否为商户系统中创建的订单号。
            $tradeStatus = $data->get('trade_status');  // 在支付宝的业务通知中，只有交易通知状态为 TRADE_FINISHED 时，支付宝才会认定为买家付款成功。
            $totalFee = $data->get('total_fee');  // 该笔订单的总金额。请求时对应的参数，原样通知回来。（外币金额）

            Log::debug('GlobalPay Notify ===> ', $data->all());

        } catch (\Exception $exception) {
            Log::error('异步通知异常 ===> ' . $exception->getMessage());
            return $globalPay->fail()->send();  // 其他框架
            // return $globalPay->fail();  // Laravel 框架可以直接这样
        }

        return $globalPay->success()->send();  // 其他框架
        // return $globalPay->success();  //  Laravel 框架可以直接这样
    }

// 验签成功会原样返回参数
/*
[
    "notify_id" => "2021082600222115620064941403711009",
    "notify_type" => "trade_status_sync",
    'sign' => 'N9ulga/B5upBXPEXREo3zQ08bG2s+xcI/xHkRtpEyz7ZjSbaldYRyKYhXqrONlUibIexALxADs6Qx8Gz4ymamLBxI4T8DB9zfcwjxfime1nlmL3ShSYV3lkRMUrA0e/Dy1I1s0iMNPtGmyJVuDNg8z3xRjmqq+3FGe3mkSnofzw=',
    "trade_no" => "2021082622001364941434754996",
    "total_fee" => "17.00",
    "out_trade_no" => "alex_1629950066",
    "notify_time" => "2021-08-26 11:59:29",
    "currency" => "JPY",
    "trade_status" => "TRADE_FINISHED",
    "sign_type" => "RSA"
];*/

// 验签失败会抛 Pudongping\GlobalPay\Exceptions\InvalidSignException 异常

```
