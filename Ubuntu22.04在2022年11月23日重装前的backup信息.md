# Ubuntu22.04在2022年11月23日重装前需要备份的数据



​    为防止vps自带的系统存在监控或后门，需要重新安装官方的系统。重装系统选为==Debian 11==，原因是所用的脚本对Debian支持的更好，新的Debian需要安装snap软件。

​    顺便把==宝塔面板==换掉，因为有后门。



### 一、docker

#### 1、container

##### a、vaultwarden

备份前先从vaultwarden中导出密码，防止备份和还原失败。

截止2022.11.20，vaultwarden的容器命令如下：

```
docker run -d=true --name vaultwarden -v /www/wwwroot/bitwarden.youguess.site/:/data/ -p 8080:80 --restart=always vaultwarden/server:latest
```

只需要备份`/www/wwwroot/bitwarden.youguess.site/`内的文件即可。

##### b、portainer

数据在volume的portainer_data中，无需备份，重新设置。

##### c、MongoDB + rocket.chat

MongoDB是rocket.chat的依赖，属于No-SQL型数据库。rocket.chat的所有数据都是保存在MongoDB中。

重装后的MongoDB和rocket.chat不再安装到docker中，直接安装在宿主系统里。因此不需要备份了。



#### 2、volume

a、portainer_data

b、rocketchat_MongoDB_data



### 二、宝塔面板

用==nginx proxy manager==替换宝塔面板的反向代理功能。



### 三、MySQL + WordPress

MySQL是WordPress的依赖数据库，属于传统的SQL型数据库。

安装WordPress的插件`一站式WP迁移`，英文名称为`All-in-One WP Migration`。

启用此插件后，点击“导出”即可，此插件会把MySQL中的数据一并导出，下载它的导出包。



### 四、gost

进入 `/etc/gost/`，备份其中的`config.json`文件。

在新系统的gost脚本安装完成后，关闭gost，把上述文件粘贴进对应目录，重启gost。

注意：gost不要安装docker版，因为创建容器时需要映射宿主主机到容器的端口，但是gost提供的服务是动态的，端口无法在创建时确定，所以直接在主机上安装更为方便后期使用。



### 五、x-ui

进入 `/etc/x-ui/`，备份其中的`x-ui.db`文件。

另外还要备份SSL证书所在的目录，设置的路径不是固定的，本人设置在`/root/xui_crt/`，新系统安装好后也要放在此目录，因为`x-ui.db`已经保存了这个路径。

注意：x-ui不要安装docker版，因为创建容器时需要映射宿主主机到容器的端口，但是x-ui提供的服务是动态的，端口无法在创建时确定，所以直接在主机上安装更为方便后期使用。



### 六、反向代理的端口

docker版vaultwarden的宿主端口：8080

docker版rocket.chat的宿主端口：3000

docker版portainer的宿主端口：9000

adguardhome：81和3001（原本使用3000，被rocket.chat占用）

只有WordPress本身搭建的网站，使用的80端口，未使用反向代理。既宝塔未成功接管80端口，但是接管了443端口。
