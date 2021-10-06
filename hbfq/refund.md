# 蚂蚁花呗分期退款

方法 | 参数 | 返回值
:---: | :---: | :---:
refund | array $order | Illuminate\Support\Collection

## 示例

```php

    public function refund()
    {
        // 花呗分期退款和非花呗分期退款操作流程一致
        $order = [
            'out_return_no' => 'alex_refund_' . time(),
            'out_trade_no' => 'alex_1629950066',
            'return_rmb_amount' => 3.45,
            'currency' => 'JPY',
            'reason' => '退款测试',
            // 'is_sync' => 'N',  // 如果 is_sync => N 则开启异步通知，否则不开启异步通知，不开启异步通知 notify_url 参数将会失效（不需要开启时，则不需要设置）
            // 'notify_url' => 'http://api.demo.com:8016/v2/alipay/forexNotify',  // $order['notify_url'] 设置了，则使用 $order['notify_url'] 的值，否则使用配置文件中的 notify_url 参数
            // 'type' => 'pc',  // 如果是网站支付，则需要设置 type 参数为 pc，手机浏览器或支付宝钱包支付时，不需要设置
        ];

        $globalPay = GlobalPay::alipay($this->config)->refund($order);

        var_dump($globalPay->toArray());
    }

// 返回值示例：array （不管是不是异步通知都会有同步结果）
// 第一次退款成功之后，如果再次调用退款的接口，那么会返回 is_success => 'F'

/*
[
    "is_success" => "T"
];
*/


// 花呗分期和非花呗分期退款时，返回的字段一样
// 异步通知时的返回值为：
/*
[
    'notify_type' => 'refund_status_sync',
    'return_amount' => '1.01',
    'notify_time' => '2021-08-26 18:32:35',
    'out_trade_no' => 'alex_1629950066',
    'refund_status' => 'REFUND_SUCCESS',
    'sign' => 'mm6YzTvgDvoEMZqHPOxf6RE24lhj7r3QGaBICOwQ6qYWM56dRn1rEXMoxFqJ6+s5W/1cijVhZBRM0CRdFDmbyxHMu5c4o4uWWkigSDGUcgLp2YBE858Kct/AqEMDyr0eeqSRzNGiSWjmNrouXdhPr4KhgIrVEiBK36rUa2O7H8g=',
    'out_return_no' => 'alex_refund_1629973952',
    'currency' => 'CNY',
    'sign_type' => 'RSA',
    'notify_id' => '2021082600222183235095291417346698',
]
    */

```
