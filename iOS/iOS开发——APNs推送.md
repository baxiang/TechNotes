## P12文件
访问 [苹果开发者账户](https://developer.apple.com/)，进入帐号首页，选择Certificates, Identifiers & Profiles
![image.png](http://upload-images.jianshu.io/upload_images/143845-df06275030ec9081.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在 Certificates, Identifiers & Profiles 中，点击 App IDs 创建应用的AppID。
![image.png](http://upload-images.jianshu.io/upload_images/143845-c8880f66aa0a0c08.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此处需要指定完整的 Bundle ID，不能使用通配符星号，拥有通配符appID是无法正常使用APNs推送服务。
![image.png](http://upload-images.jianshu.io/upload_images/143845-4cb08d67e5934d6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为 App 开启 Push Notification 功能。如果是已经创建的 App ID 也可以通过设置开启 Push Notification 功能。
![image.png](http://upload-images.jianshu.io/upload_images/143845-d2908a2234987989.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果你之前没有创建过 Push 证书或者是要重新创建一个新的，请在证书列表下面新建。  APNs 证书有开发（Development）和生产（Production）两种。开发证书用于开发调试使用；生产证书既能用于开发也可以产品发布，但是建议开发和发布分开以免出现推送事故。
![image.png](http://upload-images.jianshu.io/upload_images/143845-ed660506b4f89066.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击 "Continue", 之后选择该证书准备关联的 AppID。
![image.png](http://upload-images.jianshu.io/upload_images/143845-f3de981d46948cde.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上传 CSR 文件，
![image.png](http://upload-images.jianshu.io/upload_images/143845-87e2f72d4cbedda9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开系统自带的 钥匙串KeychainAccess 创建  CSR 文件

![image.png](http://upload-images.jianshu.io/upload_images/143845-b5ddfeae3a115115.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
填写“用户邮箱”和“常用名称” ，并选择“存储到磁盘”，证书文件后缀为 .certSigningRequest 
![image.png](http://upload-images.jianshu.io/upload_images/143845-327814d66300ba59.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上传刚刚生成的后缀为 .certSigningRequest 的文件。
生成证书成功后，点击 “Download” 按钮把证书下载下来，是后缀为 .cer 的文件

![image.png](http://upload-images.jianshu.io/upload_images/143845-e7deb6c36442891b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在钥匙串列表中“登录”——“我的证书”，找到刚才下载的证书，并导出为 .p12 文件
![image.png](http://upload-images.jianshu.io/upload_images/143845-09e370a519fbaa4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##PEM文件
p12文件生成pem的脚本
```
#gen
openssl pkcs12 -clcerts -nokeys -out cert.pem -in Certificates.p12
openssl pkcs12 -nocerts -out key.pem -in Certificates.p12
openssl rsa -in key.pem -out key.unencrypted.pem
cat cert.pem key.unencrypted.pem > ck.pem

#clear
rm cert.pem
rm key.pem
rm key.unencrypted.pem
```

##推送测试
将生成的pem文件和PHP在一个文件夹下 修改发送的token地址
```PHP
<?php
//Usage: php pushMe.php (sandbox 1 password 123456 token xxx message 'push test')

//Params
$params = array(
	'sandbox' => 1,
	'token' => '9bf41ed10a47ac3efcea9bb4c670f557dc60366d815192b49dbc471b01617ca5',
	'password' => '123456',
	'message' =>'baxiang test!',
	);

if ($argc > 1){
	for ($i = 1; $i < $argc; $i+=2){
		$key = $argv[$i];
		$value = '';
		if ($i+1 < $argc) $value = $argv[$i+1];
		$params[$key] = $value;
	}
}
// print_r($params);

// Put your device token here (without spaces):
$deviceToken = $params['token'];

// Put your private key's passphrase here:
$passphrase = $params['password'];	//password

// Put your alert message here:
$message = $params['message'];

$host = 'ssl://gateway.push.apple.com:2195';
if ($params['sandbox'] == 1) $host = 'ssl://gateway.sandbox.push.apple.com:2195';

////////////////////////////////////////////////////////////////////////////////

$ctx = stream_context_create();
stream_context_set_option($ctx, 'ssl', 'local_cert', 'ck.pem');
stream_context_set_option($ctx, 'ssl', 'passphrase', $passphrase);
stream_context_set_option($ctx, 'ssl', 'verify_peer', false);
// Open a connection to the APNS db2_server_info(connection)
$fp = stream_socket_client(
	$host, $err,
	$errstr, 60, STREAM_CLIENT_CONNECT|STREAM_CLIENT_PERSISTENT, $ctx);

if (!$fp)
	exit("Failed to connect: $err $errstr" . PHP_EOL);

echo 'Connected to APNS' . PHP_EOL;

// Create the payload body
$body['aps'] = array(
	'alert' => $message,
	'sound' => 'default'
	);
$body['msgType'] = 'ORDER';

// Encode the payload as JSON
$payload = json_encode($body);

// Build the binary notification
$msg = chr(0) . pack('n', 32) . pack('H*', $deviceToken) . pack('n', strlen($payload)) . $payload;

// Send it to the server
$result = fwrite($fp, $msg, strlen($msg));

if (!$result)
	echo 'Message not delivered to '.$host . PHP_EOL;
else
	echo 'Message successfully delivered to '.$host . PHP_EOL;

// Close the connection to the server
fclose($fp);
```
测试发送PHP
```
php pushTest.php
```
![image.png](http://upload-images.jianshu.io/upload_images/143845-651c0f3b7062bb68.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
