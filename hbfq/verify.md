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
    
// 花呗分期同步回调
/*[
    'out_trade_no' => 'order_tank_1631953377',
    'total_fee' => '39.00',
    'trade_status' => 'TRADE_FINISHED',
    'sign' => 'FEyfMiOQirHR8ruSstTj2MidiPbVsePGh325GIxvRXoEJxBuB6d9d6wc370mPhstnOHJEV3L3ljQvf3mSwJVVaYQeDk5eXu2sTk3Hifu8A850itZdaVavpw7UVQJxMC3DszoKx0G5cx5wDcYfhLszaQfJx/5qX6eJhBdM9MAvNE=',
    'trade_no' => '2021091822001380751437525954',
    'currency' => 'JPY',
    'sign_type' => 'RSA',
];*/

```

## 异步验签示例

```php

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
// 花呗分期异步回调，会多一个 hb_fq_pay_info 参数（USER_INSTALL_NUM 字段为分期的期数）
/*[
    'notify_id' => '2021091800222162349080751439311277',
    'notify_type' => 'trade_status_sync',
    'sign' => 'MizvIkQ2vdmyZS/ONRLRWQ187ZwF9v8kdzt2cF3mQzG9HCw6r2H1g/+AmqyTWBeynytA8deHUkIbOxv8iph8Oe0v+arBXhM8j5IrZre+lUb0oro6wyjdT1hFwV/tzIyPZArlHysZabHS7F0chpDR3lykXMI/+Mn63s8VK3zAwqs=',
    'trade_no' => '2021091822001380751437525954',
    'total_fee' => '39.00',
    'out_trade_no' => 'order_tank_1631953377',
    'currency' => 'JPY',
    'notify_time' => '2021-09-18 16:23:49',
    'hb_fq_pay_info' => '{"USER_INSTALL_NUM":"3"}',
    'trade_status' => 'TRADE_FINISHED',
    'sign_type' => 'RSA',
];*/

// 验签失败会抛 Pudongping\GlobalPay\Exceptions\InvalidSignException 异常

```
