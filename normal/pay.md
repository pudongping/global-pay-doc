# 支付

跨境支付宝支付目前只支持 app、wap、web 这三种。

方法 | 说明 | 参数 | 返回值
:---: | :---: | :---: | :---:
web | 电脑支付 | array $order | Symfony\Component\HttpFoundation\Response
wap | 手机网站支付 | array $order | Symfony\Component\HttpFoundation\Response
app | APP 支付 | array $order | Symfony\Component\HttpFoundation\Response

## 电脑支付

### 例子

```php

    /**
     * document link: https://global.alipay.com/docs/ac/web_cn/about
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


// 如果只需要参数，不需要返回 html 代码时
// 第一步：需要给 $order 增加 `$order['_only_args'] = true;`
// 第二步：将 send() 方法，改成 getContent() 方法，获取返回值
// 只需要参数返回时，示例值： json 字符串
// {"service":"create_forex_trade","partner":"2088000000000000","_input_charset":"UTF-8","sign_type":"RSA","notify_url":"http:\/\/api.030book.com:8016\/v2\/alipay\/forexNotify","return_url":"http:\/\/api.030omni.local.dev\/alipay\/forexReturn","subject":"交易 alex ","out_trade_no":1629946242,"currency":"JPY","rmb_fee":"0.20","refer_url":"https:\/\/www.030buy.net","product_code":"NEW_OVERSEAS_SELLER","trade_information":"{\"business_type\":4,\"goods_info\":\"変身ベ...^1\",\"total_quantity\":1}","sign":"EJtAlxj0zgSAdHzdLJ2pXEDUMjRWknrIV5wdPpQd0p9P\/gdub4hxUnqdqssRsRr2gjmn4331MNvJcG7ukrmReklJCEspEXxwDfouHCsLcObJDKNUICh6aZoZIyj+KkY7gwp1Jxr+UCdwB4BWsVUi7kHUpwpw3Lnxg4NC32GhhT4=","action":"https:\/\/intlmapi.alipay.com\/gateway.do?_input_charset=UTF-8"}HTTP/1.0 200 OK Cache-Control: no-cache, private Date: Thu, 26 Aug 2021 02:50:42 GMT {"service":"create_forex_trade","partner":"2088011062799076","_input_charset":"UTF-8","sign_type":"RSA","notify_url":"http:\/\/api.030book.com:8016\/v2\/alipay\/forexNotify","return_url":"http:\/\/api.030omni.local.dev\/alipay\/forexReturn","subject":"交易 alex ","out_trade_no":1629946242,"currency":"JPY","rmb_fee":"0.20","refer_url":"https:\/\/www.030buy.net","product_code":"NEW_OVERSEAS_SELLER","trade_information":"{\"business_type\":4,\"goods_info\":\"変身ベ...^1\",\"total_quantity\":1}","sign":"EJtAlxj0zgSAdHzdLJ2pXEDUMjRWknrIV5wdPpQd0p9P\/gdub4hxUnqdqssRsRr2gjmn4331MNvJcG7ukrmReklJCEspEXxwDfouHCsLcObJDKNUICh6aZoZIyj+KkY7gwp1Jxr+UCdwB4BWsVUi7kHUpwpw3Lnxg4NC32GhhT4=","action":"https:\/\/intlmapi.alipay.com\/gateway.do?_input_charset=UTF-8"}


// 同步回调： get 请求到 return_url 地址
// 参数示例如下：
/*
[
    'out_trade_no' => '1629977581',
    'total_fee' => '3.00',
    'trade_status' => 'TRADE_FINISHED',
    'sign' => 'EA9hATX97g18l2o1AXgnOBJoMyK7shEIk i7Bx3Q23J1eOSuE1hIdum0O6Lg24fMs3O0okUpQ9Q CRbpjrzC6SR/LU KYkeUZn/fC/wq51HSVzqwXSMC3hXnbk7h0D2yxTqMAnI 0As82W0MDBfKVZNfqxghXMZir6w/pdTj9js=',
    'trade_no' => '2021082622001380751416304935',
    'currency' => 'JPY',
    'sign_type' => 'RSA',
];
*/


// 异步回调： post 请求到 notify_url 地址
// 参数示例如下：
/*
[
    'notify_id' => '2021082600222193407080751409662370',
    'notify_type' => 'trade_status_sync',
    'sign' => 'N1C8bZINnHveqGVH35R2g8sVuM8vzGAFEzQVYdnutg2+gDMVKb4xzmNA/AHFwQD4N4BMcETthVbaMHhEXjr3ML4F9IN+Y8JAf/0dbt+2QB+PSKs22kQvF+0xQQ7Otm/r1IvrcIMWrmex6WGCA7126XQPjT98vv+jyOoeeyK9Tuw=',
    'trade_no' => '2021082622001380751416304935',
    'total_fee' => '3.00',
    'out_trade_no' => '1629977581',
    'notify_time' => '2021-08-26 19:34:07',
    'currency' => 'JPY',
    'trade_status' => 'TRADE_FINISHED',
    'sign_type' => 'RSA',
];
*/

```

## 手机网站支付

### 例子

```php

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
    
    
// 如果只需要参数，不需要返回 html 代码时
// 第一步：需要给 $order 增加 `$order['_only_args'] = true;`
// 第二步：不需要调用 send() 方法，直接调用 getContent() 方法，获取返回值
// 只需要参数返回时，示例值： json 字符串
// {"service":"create_forex_trade_wap","partner":"2088000000000000","_input_charset":"UTF-8","sign_type":"RSA","notify_url":"http:\/\/api.030book.com:8016\/v2\/alipay\/forexNotify","return_url":"http:\/\/api.030omni.local.dev\/alipay\/forexReturn","subject":"交易 alex ","out_trade_no":1629945647,"currency":"JPY","rmb_fee":"0.10","refer_url":"https:\/\/www.030buy.net","product_code":"NEW_WAP_OVERSEAS_SELLER","trade_information":"{\"business_type\":4,\"goods_info\":\"変身ベ...^1\",\"total_quantity\":1}","sign":"n6dsDa10zmtFNqJhsAl6hONU8K70J8MeQxBQAWgerODoOxKE4Bnf7\/LVTD7wcei4HAbjWIEkQDxVVtRthp2avrtgztwK2KSf6DB+\/Kf9JHnGg4Z81wp5hL4X1SB3MW1Ur11NaXmV6pdwOlNXtcWtWUIp5A3Pw2oxQBXPLGWK\/Lg=","action":"https:\/\/intlmapi.alipay.com\/gateway.do?_input_charset=UTF-8"}


// 同步回调：当用户点了[完成]之后，get 请求到 return_url 地址，并携带相关参数
// 参数示例如下：
/*
[
    'currency' => 'JPY',
    'out_trade_no' => '1629976225',
    'rmb_fee' => '0.10',
    'total_fee' => '2.00',
    'trade_no' => '2021082622001380751416021962',
    'trade_status' => 'TRADE_FINISHED',
    'sign' => 'VGtbriGZeCtSBDqgTDzzRmQfAbyylIk+zGD7pIAkPXGsiHSHOZhItOAqb0vSlSIHERqQaceA6HHiMQCxEmKyhce3d7JbIC2YDj2ZEaHaNxWPbQ2jMr4/VSAl5KYElj69VyysIDJ0PNP2vaI9dyb3OyxQc6ynaGGNPEskFGSVww8=',
    'sign_type' => 'RSA',
];*/


// 异步回调： post 请求到 notify_url 地址
// 参数示例如下：
/*
[

    'notify_id' => '2021082600222191212080751409773128',
    'notify_type' => 'trade_status_sync',
    'sign' => 'EEB3VzsLCFjg4GQPH5CxGMnbYuOMpVt2oIpb9qUNg/QfP3O7mnOLxqEJwbjMeJPW80AYPXTXMBRLhUvt2C6SmJcmATu3wMqbYeQ1PNOiaO+9WwhdVcYNetKLAD7P8ycUi0VZaACla4z0Z4WKiBxdJKUMe7qOjk6MN0rse5Tiqsk=',
    'trade_no' => '2021082622001380751416021962',
    'total_fee' => '2.00',
    'out_trade_no' => '1629976225',
    'notify_time' => '2021-08-26 19:12:12',
    'currency' => 'JPY',
    'trade_status' => 'TRADE_FINISHED',
    'sign_type' => 'RSA'

];*/    

```

## APP 支付

### 例子

```php

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
    
/*
 * app 支付成功异步通知（post 请求）返回值为：

[
'notify_id' => '2021082600222115620064941403711009',
'notify_type' => 'trade_status_sync',
'sign' => 'N9ulga/B5upBXPEXREo3zQ08bG2s+xcI/xHkRtpEyz7ZjSbaldYRyKYhXqrONlUibIexALxADs6Qx8Gz4ymamLBxI4T8DB9zfcwjxfime1nlmL3ShSYV3lkRMUrA0e/Dy1I1s0iMNPtGmyJVuDNg8z3xRjmqq+3FGe3mkSnofzw=',
'trade_no' => '2021082622001364941434754996',
'total_fee' => '17.00',
'out_trade_no' => 'alex_1629950066',
'notify_time' => '2021-08-26 11:59:29',
'currency' => 'JPY',
'trade_status' => 'TRADE_FINISHED',
'sign_type' => 'RSA'
]
*/    

```
