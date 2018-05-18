# yii2-easy-wechat
WeChat SDK for yii2 , 基于 [overtrue/wechat](https://github.com/overtrue/wechat).     
This extension helps you access `overtrue/wechat` application in a simple & familiar way:   `Yii::$app->wechat`.   

[![Latest Stable Version](https://poser.pugx.org/maxwen/yii2-easy-wechat/v/stable)](https://packagist.org/packages/maxwen/yii2-easy-wechat)
[![Total Downloads](https://poser.pugx.org/maxwen/yii2-easy-wechat/downloads)](https://packagist.org/packages/maxwen/yii2-easy-wechat)
[![License](https://poser.pugx.org/maxwen/yii2-easy-wechat/license)](https://packagist.org/packages/maxwen/yii2-easy-wechat)

## 安装
```
composer require jianyan74/yii2-easy-wechat
```

## 配置

Add the SDK as a yii2 application `component` in the `config/main.php`:

```php

'components' => [
	// ...
	'wechat' => [
		'class' => 'jianyan74\easywechat\Wechat',
		// 'userOptions' => []  # 用户身份类参数
		// 'sessionParam' => '' # 微信用户信息将存储在会话在这个密钥
		// 'returnUrlParam' => '' # returnUrl 存储在会话中
	],
	// ...
]
```

## 使用
```php

// here are two representative examples that will help you:

// 微信网页授权:
if(Yii::$app->wechat->isWechat && !Yii::$app->wechat->isAuthorized()) {
	return Yii::$app->wechat->authorizeRequired()->send();
}

// 微信支付(JsApi):
$orderData = [ 
	'openid' => '.. '
	// ... etc. 
];
$order = new WechatOrder($orderData);
$payment = Yii::$app->wechat->payment;
$prepayRequest = $payment->prepare($order);
if($prepayRequest->return_code = 'SUCCESS' && $prepayRequest->result_code == 'SUCCESS') {
	$prepayId = $prepayRequest->prepay_id;
}else{
	throw new yii\base\ErrorException('微信支付异常, 请稍后再试');
}

$jsApiConfig = $payment->configForPayment($prepayId);

return $this->render('wxpay', [
	'jsApiConfig' => $jsApiConfig,
	'orderData'   => $orderData
]);

```


### How to load Wechat configures?
the `overtrue/wechat` application always constructs with a `$options` parameter. 
I made the options as a yii2 param in the `params.php`:

recomended way:
```php
// in this way you need to create a wechat.php in the same directory of params.php
// put contents in the wechat.php like:
// return [ 
// 		// wechat options here 
// ];
'WECHAT' => require(__DIR__.'/wechat.php'),
```
OR 
```php
'WECHAT' => [ // wechat options here ]
```

[Wechat options configure help docs.](https://easywechat.org/zh-cn/docs/configuration.html)


### More documentation
see [EasyWeChat Docs](https://easywechat.org/zh-cn/docs/index.html).

Thanks to `overtrue/wechat` , realy a easy way to play with wechat SDK 😁.

## More repos for Yii2:
[yii2-ckeditor-widget](https://github.com/max-wen/yii2-ckeditor-widget)   
[yii2-adminlte-gii](https://github.com/max-wen/yii2-adminlte-gii)   
[yii2-curl](https://github.com/max-wen/yii2-curl)   
