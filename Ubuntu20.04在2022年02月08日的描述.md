# Ubuntu20.04在2022年02月08日的描述



自定义镜像中，已安装docker，docker中已运行容器：==vaultwarden==、==xray==、socks5、==adguardhome==、smartDNS、autoheal、watchtower、==kms==。

还安装了proxychains-ng、==宝塔==、==WordPress==、==x-ui==（免流）、==gost==、bbr、fail2ban、resolvconf。

系统自带netplan，因此单独配置了netplan，将ipv4的DHCP设置为静态ip模式。



注意：高亮文字是对外提供服务的程序。



与上次的区别是删除了dnsmasq，DNS体系变为2层。
