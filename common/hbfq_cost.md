# 获取花呗分期计费情况

## 返回参数说明

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

## 例子

```php

    /**
     * 获取花呗分期计费情况
     */
    public function getHbFqCost()
    {
        $totalAmount = 100.88;

        // 只需要获取 3 6 12 期相对应到还款数
        // $globalPay = GlobalPay::alipay($this->config)->getHbFqCost($totalAmount);

        // 获取 3 6 12 期相对应到还款数且显示出每一期的还款情况（用户承担所有的手续费）
        $globalPay = GlobalPay::alipay($this->config)->getHbFqCost($totalAmount, true);

        // 获取 3 6 12 期相对应到还款数且显示出每一期的还款情况（商家承担所有的手续费）
        $globalPay = GlobalPay::alipay($this->config)->getHbFqCost($totalAmount, true, true);

        var_dump($globalPay->toArray());
    }


// 返回值示例：array
/*
array:3 [
    0 => array:8 [
    "nper" => 3
    "total_amount" => 1111.11
    "total_charge" => 25.55
    "rate" => 0.023
    "per_charge" => 8.52
    "per_amount" => 370.37
    "per_total_amount" => 378.89
    "refund_list" => array:3 [
    0 => array:4 [
    "nper" => 1
        "charge" => 8.52
        "amount" => 370.37
        "current_total_amount" => 378.89
      ]
      1 => array:4 [
    "nper" => 2
        "charge" => 8.52
        "amount" => 370.37
        "current_total_amount" => 378.89
      ]
      2 => array:4 [
    "nper" => 3
        "charge" => 8.52
        "amount" => 370.37
        "current_total_amount" => 378.89
      ]
    ]
  ]
  1 => array:8 [
    "nper" => 6
    "total_amount" => 1111.11
    "total_charge" => 50.0
    "rate" => 0.045
    "per_charge" => 8.33
    "per_amount" => 185.18
    "per_total_amount" => 193.51
    "refund_list" => array:6 [
    0 => array:4 [
    "nper" => 1
        "charge" => 8.35
        "amount" => 185.21
        "current_total_amount" => 193.56
      ]
      1 => array:4 [
    "nper" => 2
        "charge" => 8.33
        "amount" => 185.18
        "current_total_amount" => 193.51
      ]
      2 => array:4 [
    "nper" => 3
        "charge" => 8.33
        "amount" => 185.18
        "current_total_amount" => 193.51
      ]
      3 => array:4 [
    "nper" => 4
        "charge" => 8.33
        "amount" => 185.18
        "current_total_amount" => 193.51
      ]
      4 => array:4 [
    "nper" => 5
        "charge" => 8.33
        "amount" => 185.18
        "current_total_amount" => 193.51
      ]
      5 => array:4 [
    "nper" => 6
        "charge" => 8.33
        "amount" => 185.18
        "current_total_amount" => 193.51
      ]
    ]
  ]
  2 => array:8 [
    "nper" => 12
    "total_amount" => 1111.11
    "total_charge" => 83.33
    "rate" => 0.075
    "per_charge" => 6.94
    "per_amount" => 92.59
    "per_total_amount" => 99.53
    "refund_list" => array:12 [
    0 => array:4 [
    "nper" => 1
        "charge" => 6.99
        "amount" => 92.62
        "current_total_amount" => 99.61
      ]
      1 => array:4 [
    "nper" => 2
        "charge" => 6.94
        "amount" => 92.59
        "current_total_amount" => 99.53
      ]
      2 => array:4 [
    "nper" => 3
        "charge" => 6.94
        "amount" => 92.59
        "current_total_amount" => 99.53
      ]
      3 => array:4 [
    "nper" => 4
        "charge" => 6.94
        "amount" => 92.59
        "current_total_amount" => 99.53
      ]
      4 => array:4 [
    "nper" => 5
        "charge" => 6.94
        "amount" => 92.59
        "current_total_amount" => 99.53
      ]
      5 => array:4 [
    "nper" => 6
        "charge" => 6.94
        "amount" => 92.59
        "current_total_amount" => 99.53
      ]
      6 => array:4 [
    "nper" => 7
        "charge" => 6.94
        "amount" => 92.59
        "current_total_amount" => 99.53
      ]
      7 => array:4 [
    "nper" => 8
        "charge" => 6.94
        "amount" => 92.59
        "current_total_amount" => 99.53
      ]
      8 => array:4 [
    "nper" => 9
        "charge" => 6.94
        "amount" => 92.59
        "current_total_amount" => 99.53
      ]
      9 => array:4 [
    "nper" => 10
        "charge" => 6.94
        "amount" => 92.59
        "current_total_amount" => 99.53
      ]
      10 => array:4 [
    "nper" => 11
        "charge" => 6.94
        "amount" => 92.59
        "current_total_amount" => 99.53
      ]
      11 => array:4 [
    "nper" => 12
        "charge" => 6.94
        "amount" => 92.59
        "current_total_amount" => 99.53
      ]
    ]
  ]
]*/

```
