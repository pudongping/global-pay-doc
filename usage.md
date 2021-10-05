# 使用说明

## 非花呗分期支付

```php

<?php

declare(strict_types=1);

namespace App\Controller;

use Pudongping\GlobalPay\GlobalPay;
use Pudongping\GlobalPay\Log;

class PayController
{

    protected $config = [
        'partner' => '2088000000000000',  // 合作身份者 id，以 2088 开头的 16 位纯数字
        'notify_url' => 'http://a90b-8-37-43-168.demo.io/index/notify_url',  // 异步回调地址
        'return_url' => 'http://a90b-8-37-43-168.demo.io/index/return_url',  // 同步回调地址
        'refer_url' => 'https://www.demo.net',  // 二级商户网站地址
        'seller_email' => 'xxxx@gmail.com',  // 签约支付宝账号或卖家支付宝帐户
        'key' => 'xxxx',  // 安全检验码，以数字和字母组成的 32 位字符
        'sign_type' => 'RSA',  // 不需要修改
        'input_charset' => 'UTF-8',  // 商户网站使用的编码格式，建议不需要修改
        'transport' => 'http',  // 访问模式,根据自己的服务器是否支持 ssl 访问，若支持请选择 https；若不支持请选择 http
        'split_fund' => '2088000000000000:0.10',  // 接受分账资金的支付宝账户 ID 和比例，用逗号分隔其他帐号信息。ID 是以 2088 开头的纯 16 位数字。
        'private_key' => '/Users/pudongping/glory/key/alipay_private_key.pem',  // 私钥路径
        'public_key' => '/Users/pudongping/glory/key/alipay_public_key.pem',  // 公钥路径

        'log' => [ // optional
            'file' => 'alipay.log',  // 当前目录下
            'level' => 'debug', // 建议生产环境等级调整为 info，开发环境为 debug
            'type' => 'single', // optional, 可选 daily.
            'max_file' => 30, // optional, 当 type 为 daily 时有效，默认 30 天
        ],
        'http' => [ // optional
            'timeout' => 5.0,
            'connect_timeout' => 5.0,
            // 更多配置项请参考 [Guzzle](https://guzzle-cn.readthedocs.io/zh_CN/latest/request-options.html)
        ],
        'mode' => 'dev', // optional,设置此参数，将进入沙箱模式
    ];

    /**
     * document link: https://global.alipay.com/docs/ac/web_cn/about
     *
     * @return mixed
     */
    public function web()
    {
        $order = [
            'out_trade_no' => time(),
            'subject' => '交易 alex ',
            'currency' => 'JPY',
            'rmb_fee' => '0.20',
            'trade_information' => json_encode([
                'business_type' => 4,
                'goods_info' => '変身ベ...^1',
                'total_quantity' => 1
            ], 256),
            // '_only_args' => true  // 只需要返回参数模式时增加
        ];

        $globalPay = GlobalPay::alipay($this->config)->web($order);

        return $globalPay->send();

        // 如果设置了 `_only_args` 为 true，则使用以下方法获取所有的参数
        // var_dump($globalPay->getContent());

    }

    /**
     * document link:  https://global.alipay.com/docs/ac/wap_cn/start
     *
     * @return mixed
     */
    public function wap()
    {

        $order = [
            'out_trade_no' => time(),
            'subject' => '交易 alex ',
            'currency' => 'JPY',
            'rmb_fee' => '0.10',
            'trade_information' => json_encode([
                'business_type' => 4,
                'goods_info' => '変身ベ...^1',
                'total_quantity' => 1
            ], 256),
            // '_only_args' => true  // 只需要返回参数模式时增加
        ];

        $globalPay = GlobalPay::alipay($this->config)->wap($order);

        return $globalPay->send();

        // 如果设置了 `_only_args` 为 true，则使用以下方法获取所有的参数
        // var_dump($globalPay->getContent());

    }

    /**
     * document link: https://global.alipay.com/docs/ac/app_cn/about
     */
    public function app()
    {
        $order = [
            'out_trade_no' => 'alex_' . time(),
            'subject' => '交易 5200',
            'currency' => 'JPY',
            'rmb_fee' => '1.01',
            'trade_information' => json_encode([
                'business_type' => 4,
                'goods_info' => '大海にて^1',
                'total_quantity' => 1
            ], 256),
        ];

        $globalPay = GlobalPay::alipay($this->config)->app($order);

        $content = $globalPay->getContent();

        var_dump($content);

    }

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

    /**
     * 退款接口 document link： https://global.alipay.com/docs/ac/global/forex_refund_cn
     */
    public function refund()
    {
        $order = [
            'out_return_no' => 'alex_refund_' . time(),
            'out_trade_no' => 'alex_1629950066',
            'return_rmb_amount' => '1.01',
            'currency' => 'JPY',
            'reason' => '退款测试',
            // 'is_sync' => 'N',  // 如果 is_sync => N 则开启异步通知，否则不开启异步通知，不开启异步通知 notify_url 参数将会失效（不需要开启时，则不需要设置）
            // 'notify_url' => 'http://api.demo.com:8016/v2/alipay/forexNotify',  // $order['notify_url'] 设置了，则使用 $order['notify_url'] 的值，否则使用配置文件中的 notify_url 参数
            // 'type' => 'pc',  // 如果是网站支付，则需要设置 type 参数为 pc，手机浏览器或支付宝钱包支付时，不需要设置
        ];

        $globalPay = GlobalPay::alipay($this->config)->refund($order);

        var_dump($globalPay->toArray());

    }

    /**
     * 同步验签
     */
    public function return()
    {
        $data = GlobalPay::alipay($this->config)->verify();
    }

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

    /**
     * 获取汇率
     */
    public function getExchangeRate()
    {
        $globalPay = GlobalPay::alipay($this->config)->getExchangeRate();

        var_dump($globalPay->toArray());
    }

}

```

## 花呗分期支付

```php

<?php

declare(strict_types=1);

namespace App\Controller;

use Pudongping\GlobalPay\GlobalPay;
use Pudongping\GlobalPay\Log;

class HbfqPayController
{

    public function web()
    {
        $order = [
            'out_trade_no' => time(),
            'subject' => '交易 alex',
            'currency' => 'JPY',
            'rmb_fee' => 5.45,
            'trade_information' => json_encode([
                'business_type' => 4,
                'goods_info' => '交易费用^1',
                'total_quantity' => 1
            ], 256),
            'hb_fq_param' => [
                'num' => 3,  // 花呗分期分期数，只支持 3、6、12 期
                // 只有 is_has_household 为 true， is_seller_percent 才能设置为 true
                'is_has_household' => false,  // 是否拥有出资户，只有拥有出资户，商家才能贴息，否则只能用户贴息
                'is_seller_percent' => false,  // 是否商家贴息
                // 花呗分期开启订单传参贴息活动（不支持 PC 支付，无论是国际还是国内的交易都不支持）
                // 因此相比 app 支付，不能传递 is_order_subsidy 参数
            ],
            // '_only_args' => true  // 只需要返回参数模式时增加
        ];

        $globalPay = GlobalPay::alipay($this->config)->web($order);

        return $globalPay->send();

        // 如果设置了 `_only_args` 为 true，则使用以下方法获取所有的参数
        // var_dump($globalPay->getContent());
    }

    public function wap()
    {
        $order = [
            'out_trade_no' => time(),
            'subject' => '交易 alex',
            'currency' => 'JPY',
            'rmb_fee' => 5.45,
            'trade_information' => json_encode([
                'business_type' => 4,
                'goods_info' => '交易费用^1',
                'total_quantity' => 1
            ], 256),
            'hb_fq_param' => [
                'num' => 3,  // 花呗分期分期数，只支持 3、6、12 期
                // 只有 is_has_household 为 true， is_seller_percent 才能设置为 true
                'is_has_household' => false,  // 是否拥有出资户，只有拥有出资户，商家才能贴息，否则只能用户贴息
                'is_seller_percent' => false,  // 是否商家贴息
                // 花呗分期开启订单传参贴息活动（不支持 PC 支付，无论是国际还是国内的交易都不支持）
                // 因此相比 app 支付，不能传递 is_order_subsidy 参数
            ],
            // '_only_args' => true  // 只需要返回参数模式时增加
        ];

        $globalPay = GlobalPay::alipay($this->config)->wap($order);

        return $globalPay->send();

        // 如果设置了 `_only_args` 为 true，则使用以下方法获取所有的参数
        // var_dump($globalPay->getContent());
    }

    public function app()
    {
        $order = [
            'out_trade_no' => time(),
            'subject' => '交易 alex',
            'currency' => 'JPY',
            'rmb_fee' => 3.45,
            'trade_information' => json_encode([
                'business_type' => 4,
                'goods_info' => '交易费用^1',
                'total_quantity' => 1
            ], 256),
            'hb_fq_param' => [
                'num' => 3,  // 花呗分期分期数，只支持 3、6、12 期
                // 只有 is_has_household 为 true， is_seller_percent 才能设置为 true，否则 is_seller_percent 只能设置为 false
                'is_has_household' => false,  // 是否拥有出资户，只有拥有出资户，商家才能贴息，否则只能用户贴息
                'is_seller_percent' => false,  // 是否商家贴息， true 为商家贴息， false 为用户贴息
                'is_order_subsidy' => false,  // 是否开启订单传参贴息活动
                // 出资户贴息和订单传参贴息只能允许一个为 true
            ],
        ];

        $globalPay = GlobalPay::alipay($this->config)->app($order);

        $content = $globalPay->getContent();

        var_dump($content);
    }

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

    public function return()
    {
        $data = GlobalPay::alipay($this->config)->verify();
    }

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

    /**
     * 获取花呗分期计费情况
     */
    public function getHbFqCost()
    {
        $totalAmount = 100.88;

        // 只需要获取 3 6 12 期相对应的还款数
        // $globalPay = GlobalPay::alipay($this->config)->getHbFqCost($totalAmount);

        // 获取 3 6 12 期相对应到还款数且显示出每一期的还款情况（用户承担所有的手续费）
        // $globalPay = GlobalPay::alipay($this->config)->getHbFqCost($totalAmount, true);

        // 获取 3 6 12 期相对应到还款数且显示出每一期的还款情况（商家承担所有的手续费）
        $globalPay = GlobalPay::alipay($this->config)->getHbFqCost($totalAmount, true, true);

        var_dump($globalPay->toArray());
    }

}

```

