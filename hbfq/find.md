# 蚂蚁花呗分期-订单查询

方法 | 参数 | 返回值
:---: | :---: | :---:
find | array $order | Illuminate\Support\Collection

## 示例

```php

    public function find()
    {
        // out_trade_no 和 trade_no 参数可以同时含有，也可以二选一
        $order = [
            'out_trade_no' => 'alex_1629950066',
            // 'trade_no' => '2021082622001364941434754996',
            'is_hbfq' => true,  // 该笔订单是否为花呗分期支付，订单查询出来的结果会含有 hb_fq_num 参数，不是花呗分期订单则没有这个参数
        ];

        $globalPay = GlobalPay::alipay($this->config)->find($order);

        var_dump($globalPay->toArray());
    }
    
    
// 花呗分期获取的结果
// 重要的是需要关注 hb_fq_num （花呗分期期数）这个参数，有这个参数表示是花呗分期，否则不为花呗分期支付

/*
[
    "is_success" => "T",
    "sign" => "dHO8JgQWlZaMdtCA1VEs/sxQiawr0R3Svh2cGNnv5CcOyA5UveLPHgzEHlC70ToXB5nTOgTYOzvusm5K74rc0vLnH4kg+l2dvwUPYzAChG4+fasbFpombP+7Vaf4sM7ZM5c79zg4bFi615h90sqSvpdOf7+BWt3m",
    "sign_type" => "RSA",
    "params" => [
        "buyer_email" => "she***@126.com",
        "buyer_id" => "2088000000000000",
        "discount" => "0.00",
        "flag_trade_locked" => "0",
        "gmt_create" => "2021-09-18 16:12:07",
        "gmt_last_modified_time" => "2021-09-18 16:13:53",
        "gmt_payment" => "2021-09-18 16:13:53",
        "hb_fq_num" => "3",
        "is_total_fee_adjust" => "F",
        "operator_role" => "B",
        "out_trade_no" => "order_tank_1631952640",
        "payment_type" => "100",
        "price" => "0.45",
        "quantity" => "1",
        "seller_email" => "zik***@yahoo.co.jp",
        "seller_id" => "20880000000000006",
        "seller_received_total_amount" => "0.00",
        "send_back_fee" => "0.00",
        "subject" => "交易 3331631952640",
        "to_buyer_fee" => "0.00",
        "to_seller_fee" => "0.45",
        "total_fee" => "0.45",
        "trade_no" => "2021091822001380751437516153",
        "trade_status" => "TRADE_FINISHED",
        "use_coupon" => "F"
    ]
];
*/

```
