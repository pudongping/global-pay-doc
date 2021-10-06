# 获取汇率

## 例子

```php

    /**
     * 获取汇率
     * document link: https://global.alipay.com/docs/ac/global/forex_rate_file_cn
     */
    public function getExchangeRate()
    {
        $globalPay = GlobalPay::alipay($this->config)->getExchangeRate();

        var_dump($globalPay->toArray());
    }


// 返回值示例：array

/*
["JPY" => "0.059123",
"USD" => "6.706400",
"THB" => "0.190629",
"SGD" => "4.985800",
"SEK" => "0.782500",
"NOK" => "0.792300",
"KRW" => "0.006065",
"HKD" => "0.864700",
"GBP" => "8.865400",
"EUR" => "7.412300",
"DKK" => "0.997400",
"CHF" => "6.836600",
"CAD" => "5.178200",
"AUD" => "5.092300"]
*/

```
