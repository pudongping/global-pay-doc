# 订单查询

方法 | 参数 | 返回值
:---: | :---: | :---:
find | array&#124;string $order | Illuminate\Support\Collection

## 示例

```php

    /**
     * 单笔查询接口 document link: https://global.alipay.com/docs/ac/global/single_trade_query_cn
     */
    public function find()
    {
        // out_trade_no 和 trade_no 参数可以同时含有，也可以二选一
        $order = [
            'out_trade_no' => 'alex_1629950066',
            // 'trade_no' => '2021082622001364941434754996',
        ];

        $globalPay = GlobalPay::alipay($this->config)->find($order);

        var_dump($globalPay->toArray());

    }
    
    
// 返回值示例：array
/*
[
    "is_success" => "T",
    "sign" => "fnlFMVJln9RGwrAdg1ijjcfX1ccxFoWA64EDLulb1yd12m9/t41tppVuvsWhYv9I4GE6aqs/UjXG8DucM3+BIvfyC9khP+tYEcCY9cAYGNs8Dw2iwK3yMQJgs/5Hi775cbwZVzOea7pBIfx+xVppocga78t+vNjchsSaGLDEaVg=",
    "sign_type" => "RSA",
    "params" => [
        "buyer_email" => "185******40",
        "buyer_id" => "208800000000000",
        "discount" => "0.00",
        "flag_trade_locked" => "0",
        "gmt_create" => "2021-08-26 11:56:19",
        "gmt_last_modified_time" => "2021-08-26 11:56:19",
        "gmt_payment" => "2021-08-26 11:56:19",
        "is_total_fee_adjust" => "F",
        "operator_role" => "B",
        "out_trade_no" => "alex_1629950066",
        "payment_type" => "100",
        "price" => "1.01",
        "quantity" => "1",
        "seller_email" => "mik***@gmail.com",
        "seller_id" => "208800000000002",
        "subject" => "交易 5200",
        "to_buyer_fee" => "0.00",
        "to_seller_fee" => "1.01",
        "total_fee" => "1.01",
        "trade_no" => "2021082622001364941434754996",
        "trade_status" => "TRADE_FINISHED",
        "use_coupon" => "F",
    ]
];*/    

```
