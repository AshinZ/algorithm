### webhook对应php脚本

```php
<?php
ini_set('date.timezone','Asia/Shanghai');
$target = ''; // 生产环境web目录
//密钥
$secret = "";
//获取GitHub发送的内容
$json = file_get_contents('php://input');
$content = json_decode($json, true);
//github发送过来的签名
$signature = $_SERVER['HTTP_X_HUB_SIGNATURE'];
if (!$signature) {
   return http_response_code(404);
}
list($algo, $hash) = explode('=', $signature, 2);
//计算签名
$payloadHash = hash_hmac($algo, $json, $secret);
// 判断签名是否匹配
if ($hash === $payloadHash) {
   
    $cmd = "cd $target && sudo git pull  2>&1";
    $res = shell_exec($cmd);
   //下面是返回给github的返回信息 可不加
    $res_log = 'Success:'.PHP_EOL;
    $res_log .= $cmd . '结果' . $res .PHP_EOL;
    $res_log .= $content['head_commit']['author']['name'] . ' 在' . date('Y-m-d H:i:s') . '向' . $content['repository']['name'] . '项目的' . $content['ref'] . '分支push了' . count($content['commits']) . '个commit：' . PHP_EOL;
    $res_log .= $res.PHP_EOL;
    $res_log .= '======================================================================='.PHP_EOL;
    echo $res_log;
} else {
    $res_log  = 'Error:'.PHP_EOL;
    $res_log .= $content['head_commit']['author']['name'] . ' 在' . date('Y-m-d H:i:s') . '向' . $content['repository']['name'] . '项目的' . $content['ref'] . '分支push了' . count($content['commits']) . '个commit：' . PHP_EOL;
    $res_log .= '密钥不正确不能pull'.PHP_EOL;
    $res_log .= '======================================================================='.PHP_EOL;
    echo $res_log;
}

```
