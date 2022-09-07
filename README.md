# tools
Easy to use tool

v2ray.sh （此脚本对网上的原脚本做了改动，主要为域名解析验证部分，参考如下：）
https://eatash.com/index.php/2021/11/16/%E6%8A%9B%E7%A0%96%E5%BC%95%E7%8E%89%E4%B9%8B-v2ray%E4%B8%80%E9%94%AE%E8%84%9A%E6%9C%AC%E7%9A%84%E4%BF%AE%E5%A4%8D%EF%BC%88hijk-art%E5%85%B3%E7%AB%99%E5%90%8Ehostip%E4%B8%8D%E8%83%BD%E8%AE%BF%E9%97%AE/

参考站点：
https://dppbk.com/634.html

本脚本能让您在服务端一键安装基于Nginx+websocket+tls的v2ray流量伪装和bbr加速模块，接下来就感受稳如狗的体验吧，再也不用担心ip被墙了！

伪装的前提是需要一个域名（例如hijk.pw），并且域名的某个主机名（例如test.hijk.pw）正确解析到服务器的ip！

本次的V2ray一键脚本功能强大，支持常规VMESS协议、VMESS+websocket+TLS+Nginx、VLESS+TCP+XTLS、VLESS+TCP+TLS等六种组合，支持CentOS 7/8、Ubuntu 16.04以上、Debian 8以上系统，以及相关衍生系统。

使用步骤：

1.准备一个境外服务器，如果用VMESS+WS+TLS或者VLESS系列协议，则还需一个域名。

2. 如果vps运营商开启了防火墙（阿里云、Ucloud、腾讯云、AWS、GCP等VPS默认开启，搬瓦工/hostdare/vultr默认关闭），请先登录vps管理后台放行80和443端口，否则可能会导致获取证书失败；

3.ssh链接到你购买的服务器：

4.在服务器上wget v2ray.sh 获取此脚本，执行脚本
```
bash v2ray.sh
```

![image](https://user-images.githubusercontent.com/36028587/188813851-cd1feb29-20da-42de-b5e7-46ec20ed2107.png)
![image](https://user-images.githubusercontent.com/36028587/188814002-0d424e7d-58d6-479e-a66c-266ae1763afb.png)

接下来按照提示一步步安装即可，中间会安装各种组件，会自动安装

此脚本可能会造成v2ray安装失败，如下：
![image](https://user-images.githubusercontent.com/36028587/188814859-90a1bb38-e5f0-4d1d-8b50-9129061f918b.png)

```
cp: cannot stat '/tmp/v2ray/v2ctl': No such file or directory
 BBR模块已安装
 v2ray启动失败，请检查日志或查看端口是否被占用！

 V2ray运行状态：已安装 未运行
 ```
 
 然后通过systemctl status v2ray 查看为启动失败的状态，此故障可能为v2ray的版本问题导致的
 
 解决方法，更新版本：
 
 bash <(curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/goV2.sh)
 
 ```
[root@ecsEfG4z v2ray]# bash <(curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/goV2.sh)
Installing V2Ray v4.33.0 on x86_64
Downloading V2Ray: https://github.com/v2fly/v2ray-core/releases/download/v4.33.0/v2ray-linux-64.zip
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100 11.8M  100 11.8M    0     0  1046k      0  0:00:11  0:00:11 --:--:-- 1164k
Archive:  /tmp/v2ray/v2ray.zip
  inflating: /usr/bin/v2ray/geoip.dat  
  inflating: /usr/bin/v2ray/geosite.dat  
  inflating: /usr/bin/v2ray/v2ctl    
  inflating: /usr/bin/v2ray/v2ray    
Removed /etc/systemd/system/multi-user.target.wants/v2ray.service.
Created symlink /etc/systemd/system/multi-user.target.wants/v2ray.service → /etc/systemd/system/v2ray.service.
V2Ray v4.33.0 is installed.
[root@ecsEfG4z v2ray]# 

[root@ecsEfG4z v2ray]# systemctl restart v2ray
[root@ecsEfG4z v2ray]# systemctl status v2ray
● v2ray.service - V2Ray Service
   Loaded: loaded (/etc/systemd/system/v2ray.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2022-09-07 13:36:39 CST; 2s ago
     Docs: https://www.v2ray.com/
           https://www.v2fly.org/
 Main PID: 18454 (v2ray)
    Tasks: 6 (limit: 5036)
   Memory: 7.4M
   CGroup: /system.slice/v2ray.service
           └─18454 /usr/bin/v2ray/v2ray -config /etc/v2ray/config.json

Sep 07 13:36:39 ecsEfG4z systemd[1]: Started V2Ray Service.
Sep 07 13:36:39 ecsEfG4z v2ray[18454]: V2Ray 4.33.0 (V2Fly, a community-driven edition of V2Ray.) Custom (go1.15.5 linux/amd64)
Sep 07 13:36:39 ecsEfG4z v2ray[18454]: A unified platform for anti-censorship.
Sep 07 13:36:39 ecsEfG4z v2ray[18454]: 2022/09/07 13:36:39 [Info] v2ray.com/core/main/jsonem: Reading config: /etc/v2ray/config.json
Sep 07 13:36:39 ecsEfG4z v2ray[18454]: 2022/09/07 13:36:39 [Warning] v2ray.com/core: V2Ray 4.33.0 started
[root@ecsEfG4z v2ray]# 
 ```
 
 查看systemctl status v2ray 可以正常运行，访问伪装的域名（如abc.grooow.cn）可以看到之前设置的页面或站点。访问域名+路径 abc.grooow.cn/def，响应Bad Request，说明成功了。
 
 

##### 其他
1. 查看v2ray运行状态 / 配置：bash <(curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/centos_install_v2ray2.sh) info

2. v2ray管理命令：启动：systemctl start v2ray，停止：systemctl stop v2ray，重启：systemctl restart v2ray；

3. nginx管理命令：测试配置文件有无错误：nginx -t，启动：systemctl start nginx，停止：systemct stop nginx，重启：systemctl restart nginx；

4. 更新v2ray到最新版：bash <(curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/goV2.sh)（提示“装不上daemon”不用管，systemctl restart v2ray重新启动v2ray就好了）

5. 查看SSL证书：certbot certificates，更新证书：systemctl stop nginx; certbot renew; systemctl restart nginx

6. 卸载： bash <(curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/centos_install_v2ray2.sh) uninstall；


各个安装模式菜单说明：
V2ray脚本安装菜单
VMESS，即最普通的V2ray服务器，没有伪装，也不是VLESS
VMESS+TCP+TLS，带伪装的V2ray，不能过CDN中转
VMESS+WS+TLS，即最通用的V2ray伪装方式，能过CDN中转，推荐使用
VLESS+TCP+TLS，最通用的VLESS版本，不能过CDN中转，但比VMESS+TCP+TLS方式性能更好
VLESS+WS+TLS，最通用V2ray伪装的VLESS脚本，能过CDN中转，推荐使用
VLESS+TCP+XTLS，目前最强悍的VLESS+XTLS组合，强力推荐使用（但是客户端支持没那么好）

目前V2ray一键脚本支持以下功能：

目前最新版的V2ray内核支持trojan以及trojan+xtls，但是没来得及加上，后续有空再完善。

注意：目前一些客户端不支持VLESS协议，或者不支持XTLS，请按照自己的情况选择组合

VLESS+TCP+XTLS一键脚本注意事项
1. 如果服务器已经有运行网站，请联系网站运维再执行脚本，否则可能导致原来网站无法访问！（本脚本目前不支持宝塔）

2. 对域名没有要求，不管国内还是国外注册的都可以，不需要备案，不会影响使用，也不会带来安全/隐私上的问题；

3. 除非443端口被墙或另有它用，建议使用443！

4. 如果想上cdn（必须是WS版才可以）

5. 查看certbot申请的SSL证书：certbot certificates，更新证书：systemctl stop nginx; certbot renew; systemctl restart nginx

6. 刚搭建好不要猛上流量，否则会导致被限速、端口被墙，严重可能ip被墙。

目前app store上没有免费v2ray客户端，付费的v2ray客户端有：

Shadowrocket（俗称小火箭，注意不是shadowrocket VPN）
i2Ray
pepi
Kitsunebi
Quantumult（内置免费节点）
客户端下载：

V2ray windows客户端
V2ray 安卓客户端
V2ray Mac客户端

参考站点：

https://www.itblogcn.com/article/1501.html/comment-page-1

https://github.com/luciferkids/hijkpw-scripts

https://ssrvps.org/archives/10346
