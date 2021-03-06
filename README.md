V2Ray 基于 Nginx 的 vless+ws+tls 一键安装脚本

    感谢 JetBrains 提供的非商业开源软件开发授权

    Thanks for non-commercial open source development authorization by JetBrains

vless支持基于首包长度的分流fallback，可以将长度<18或认证失败或无效协议转发到指定地址，注意fallback只适用于TCP模式下，其他模式不可以有这个配置项，值为空也不可以（会报错）。
vless目前没有加密，所以目前使用vless最安全的办法就是借用TLS加密通道，支持vless的v2ray core最低版本为4.27+，服务端需要更新版本，同时客户端也要去官网下载最新版本，官方release传送门。

准备工作

    准备一个域名，并将A记录添加好。
    V2ray官方说明，了解 TLS WebSocket 及 V2ray 相关信息
    安装好 wget
    此脚本 git 环境需要自己安装，不然伪装网站无法正常拉取！脚本并未集成该命令，安装命令如下
    
    
2、放行VPS服务器端口
在执行VLESS一键安装脚本之前，我们必须在防火墙放行你要开启的服务器端口（我这里以80/443端口为例），否则安装SSL证书会失败。请提前检查你的VPS服务器是否已经放行了你要开放的端口，否则请执行以下操作命令
    
  （1）如果你是 CentOS/Fedora/RedHat 系统，则依次执行以下命令：

```
firewall-cmd --query-port=端口号/tcp #查看“端口号”是否放行
systemctl start firewalld.service #开启防火墙
firewall-cmd --zone=public --add-port=80/tcp --permanent #放行80端口
firewall-cmd --zone=public --add-port=443/tcp --permanent #放行443端口
systemctl restart firewalld.service #重启防火墙
firewall-cmd --reload #重新载入配置
```


（2）如果你是 Debian/Ubuntu 系统，则依次执行以下命令：
```
apt-get install iptables #安装iptables
iptables -I INPUT -p tcp --dport 80 -j ACCEPT #放行80端口
iptables -I INPUT -p tcp --dport 443 -j ACCEPT #放行443端口
iptables-save #保存规则
apt-get install iptables-persistent #安装iptables-persistent
netfilter-persistent save
netfilter-persistent reload
``
#（3）重启VPS服务器，执行命令：

reboot

#3、安装Git环境
   yum install -y git  #CentOS安装命令

   apt install -y git  #Debian安装命令
   

安装/更新方式（h2 和 ws 版本已合并）


#4、执行VLESS一键安装脚本{2选一}


#   VLESS+Ws+Tls 一键安装脚本1

                                 
```
wget -N --no-check-certificate -q -O install.sh "https://raw.githubusercontent.com/wulabing/V2Ray_ws-tls_bash_onekey/dev/install.sh" && chmod +x install.sh && bash install.sh
```

#   VLESS+Ws+Tls 一键安装脚本2
```
bash <(curl -L -s https://raw.githubusercontent.com/atrandys/v2ray-ws-tls/master/vless_tcp_tls.sh)
```

注意事项

    如果你不了解脚本中各项设置的具体含义，除域名外，请使用脚本提供的默认值
    使用本脚本需要你拥有 Linux 基础及使用经验，了解计算机网络部分知识，计算机基础操作
    目前支持Debian 9+ / Ubuntu 18.04+ / Centos7+ ，部分Centos模板可能存在难以处理的编译问题，建议遇到编译问题时，请更换至其他系统模板
    群主仅提供极其有限的支持，如有问题可以询问群友
    每周日的凌晨3点，Nginx 会自动重启以配合证书的签发定时任务进行，在此期间，节点无法正常连接，预计持续时间为若干秒至两分钟

更新日志

    更新内容请查看 CHANGELOG.md

鸣谢

    本脚本的另一个分支版本（Use Host）地址： https://github.com/dylanbai8/V2Ray_ws-tls_Website_onekey 请根据需求进行选择 该作者可能已停止维护
    本脚本中 MTProxy-go TLS 版本项目引用 https://github.com/whunt1/onekeymakemtg 在此感谢 whunt1
    本脚本中 锐速4合1脚本原项目引用 https://www.94ish.me/1635.html 在此感谢
    本脚本中 锐速4合1脚本修改版项目引用 https://github.com/ylx2016/Linux-NetSpeed 在此感谢 ylx2016

证书

    如果你已经拥有了你所使用域名的证书文件，可以将 crt 和 key 文件命名为 v2ray.crt v2ray.key 放在 /data 目录下（若目录不存在请先建目录），请注意证书文件权限及证书有效期，自定义证书有效期过期后请自行续签

脚本支持自动生成 let's encrypted 证书，有效期3个月，理论上自动生成的证书支持自动续签
查看客户端配置

cat ~/v2ray_info.txt
V2ray 简介

    V2Ray是一个优秀的开源网络代理工具，可以帮助你畅爽体验互联网，目前已经全平台支持Windows、Mac、Android、IOS、Linux等操作系统的使用。
    本脚本为一键完全配置脚本，在所有流程正常运行完毕后，直接按照输出结果设置客户端即可使用
    请注意：我们依然强烈建议你全方面的了解整个程序的工作流程及原理

建议单服务器仅搭建单个代理

    本脚本默认安装最新版本的V2ray core
    V2ray core 目前最新版本为 4.22.1（同时请注意客户端 core 的同步更新，需要保证客户端内核版本 >= 服务端内核版本）
    建议使用默认的443端口作为连接端口
    伪装内容可自行替换。

注意事项

    推荐在纯净环境下使用本脚本，如果你是新手，请不要使用Centos系统。
    在尝试本脚本确实可用之前，请不要将本程序应用于生产环境中。
    该程序依赖 Nginx 实现相关功能，请使用 LNMP 或其他类似携带 Nginx 脚本安装过 Nginx 的用户特别留意，使用本脚本可能会导致无法预知的错误（未测试，若存在，后续版本可能会处理本问题）。
    V2Ray 的部分功能依赖于系统时间，请确保您使用V2RAY程序的系统 UTC 时间误差在三分钟之内，时区无关。
    本 bash 依赖于 V2ray 官方安装脚本 及 acme.sh 工作。
    Centos 系统用户请预先在防火墙中放行程序相关端口（默认：80，443）

启动方式

启动 V2ray：systemctl start v2ray

停止 V2ray：systemctl stop v2ray

启动 Nginx：systemctl start nginx

停止 Nginx：systemctl stop nginx
相关目录

Web 目录：/home/wwwroot/3DCEList

V2ray 服务端配置：/etc/v2ray/config.json

V2ray 客户端配置: ~/v2ray_info.inf

Nginx 目录： /etc/nginx

证书文件: /data/v2ray.key 和 /data/v2ray.crt 请注意证书权限设置



捐赠

目前支持通过 捐赠


（截止到2020年8月15日）

图形客户端
V2RayNG   
V2RayNG 是一个基于 V2Ray 内核的 Android 应用，它可以创建基于 VMess 的 VPN 连接。

V2rayN   
V2RayN 是一个基于 V2Ray 内核的 Windows 客户端。

V2rayU   
V2rayU，基于 V2Ray 核心的 macOS 客户端，使用 Swift 4.2 编写，支持 VMess、Shadowsocks、SOCKS5 等服务协议，支持订阅，支持二维码、剪贴板导入、手动配置、二维码分享等。

Qv2ray   
跨平台 V2Ray 客户端，支持 Linux、Windows、macOS，可通过插件系统支持 SSR / Trojan / Trojan-Go / NaiveProxy 等协议，不支持批量测速，不支持自动更新，不建议小白使用

支持VLESS软路由插件
（截止到2020年8月15日）

PassWall
PassWall（v3.9.35+），支持 OpenWRT
