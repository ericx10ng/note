# WifiDog + AuthPuppy

- Netgear 3700
- 路由器固件: OpenWRT/DDWRT
- Gateway: WifiDog
- Auth Server: AuthPuppy

每个路由器就是一个节点（node）

## AuthPuppy 的安装
### 方法1:
- 安装xampp
- 下载 AuthPuppy 源码包 https://launchpad.net/authpuppy
- 解压至xampp的htdoc目录
- 修改xampp最下面的，改为Allow ALL
- 访问服务器,比如localhost/authpuppy/web/, 按指引安装即可（db需要先创建）
### 方法2:
- 下载 AuthPuppy 源码包 https://launchpad.net/authpuppy
- 解压 `tar zxvf authpuppy-1.0.0-stable.tgz`
- 移动到网站目录 `mv authpuppy /var/www/`
- 修改文件所有者以免出现权限问题报错 `chown -R www-data:www-data /var/www/authpuppy`
- 创建 MySQL 数据库（略）
- 创建 Apache/Nginx 虚拟主机
	- 网站路径：`/var/www/authpuppy/web`
	- 绑定域名：`auth.blackmagic.science`
	- 启用 `url rewrite`
- 打开 http://auth.blackmagic.science 执行 AuthPuppy 安装程序

## 添加新节点

### 登录 AuthPuppy 管理平台，新建节点

- Name: 这个节点的名称，可以随意。比如 `Gateway 01` 
- gw id: gateway id，比如 `GW01` 对应 wifidog 里的配置
- Deployment status: deployed

	（其他可选填）

## 路由器上的设置

### 安装 wifidog 

- OpenWRT： `opkg install wifidog`
- DDWRT: （已经内置预装了）

### 配置 wifidog

编辑 `etc/wifidog.conf`, 需要修改两个地方

- GatewayID GW01
- AuthServer

```
AuthServer {
    Hostname auth.blackmagic.science
    SSLAvailable no
    Path /
}
```

### 启用 wifidog

- `/etc/init.d/wifidog enable`
- `/etc/init.d/wifidog start`

## TODOs
- AuthPuppy 和 现有用户系统的整合
- 认证页面的美化
- 白名单

## bug
- DD-WRT Netgear 3700无法自动重定向, 比如路由器地址192.168.1.1, 需要手动访问192.168.1.1:2060，才会引导至认证页面
