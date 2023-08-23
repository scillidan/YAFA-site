---
date: 2023-05-03
description: >
		Blog, VPS, CDN, VNC, pm2, rsync, Rocky Linux, Apache , 腾讯云
---

# Deploy Sites to VPS - Expand Content

## CDN

1. 云产品 → 对象存储 → 存储桶列表 → 创建存储桶 ... 公有读私有写
2. 内容分发网络 → 域名管理 → 添加域名 ... 静态加速/流媒体点播加速 → COS源
    1. 高级配置 → 宽带封顶配置 → 访问闸值
3. DNS解析 → 添加记录 → 记录类型CNAME...

### HTTPS

1. 云产品 → SSL证书 → 我的证书 → 申请
2. 内容分发网络 → 域名管理 → 管理 → HTTPS配置 → 配置证书

### CORS

可用于CDN字体服务等。详情请参考[设置静态网站](https://cloud.tencent.com/document/product/436/14984)、[设置跨域访问](https://cloud.tencent.com/document/product/436/13318)。大致流程为：
1. 新建储存桶，用于静态网站
    1. 储存桶 → 安全管理 → 跨域访问CORS设置 → 添加规则
    2. 上传网页字体文体到储存桶
2. 给储存桶设置CDN加速
    1. 设置HTTP响应头配置
3. 在`.css`文件中调用字体文件的网址，进行测试

## VNC

### VNC server

```bash
sudo dnf update -y
sudo dnf install -y epel-release
sudo dnf groupinstall -y "Xfce" "base-x"
sudo systemctl set-default graphical
sudo reboot
```

```bash
sudo dnf install tigervnc-server
sudo adduser vncuser
sudo passwd vncuser
sudo su - vncuser
```

```bash
sudo dnf install firewalld -y
sudo systemctl enable firewalld
sudo systemctl start firewalld
sudo firewall-cmd --state
firewall-cmd --zone=public --permanent --add-service=vnc-server
firewall-cmd --reload
```

```bash
su vncuser
vim ~/.vnc/config
```

``` title="config"
session=xfce
geometry=1280x800
# localhost
# alwaysshared
```

```bash
sudo systemctl start firewalld
```

If you need to kill the process:

```bash
pf -fu vncuser
kill -9 vncuser
```

↪ [Install and Configure VNC Server on Rocky Linux 8](https://techviewleo.com/install-and-configure-vnc-server-on-rocky-linux)  
↪ [How To Set Up a Firewall Using firewalld on Rocky Linux 8](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-rocky-linux-8)

### VNC viewer (on PC)

```bash
scoop install tightvnc
tvnviewer vpsip::5901 -password=yourpassword
```

↪ [How to Install VNC Server on Rocky Linux](https://www.howtoforge.com/how-to-install-vnc-server-on-rocky-linux/)

## PM2

以部署[excalith/excalith-start-page](https://github.com/excalith/excalith-start-page)为例。安装依赖项：

```bash
dnf install nodejs -y
npm install --global yarn
yarn global add pm2
```

克隆库：

```
cd /var/www
git clone https://github.com/excalith/excalith-start-page
cd excalith-start-page
```

安装`package.json`中的依赖项：

```bash
yarn
```

README.md中提到了`yarn dev`命令。这里用`package.json`中写的另外的`script`。

```
yarn build
yarn start
```

按`Ctrl + C`退出运行。

```
pm2 start npm --name "excalith-start-page" --watch -- start
```

```
rsync -r "/cygdrive/d/yourpath/excalith-start-page/" "root@yourvpsip:/var/www/excalith-start-page" --include={'.*'} --exclude={'.github','.next/','build/','node_modules/','.git'}
```

这里用子域名来访问。新建一个VirtualHost（虚拟主服务器）配置

```
vi /etc/httpd/conf.d/sub.example.com.conf
```

```bash title="sub.example.com.conf"
<VirtualHost *:80>
  ServerName www.sub.example.com
  ServerAlias sub.example.com

  ErrorLog /var/log/httpd/sub.example.com-error.log
  CustomLog /var/log/httpd/sub.example.com-access.log combined
  ProxyPreserveHost On
  ProxyPass / http://localhost:3000/
  ProxyPassReverse / http://localhost:3000/
</VirtualHost>
```

↪ †	[How to deploy Next.js website on Apache webserver](https://stackoverflow.com/questions/74681648)


🔥我想要使用`3000`以外的端口，但目前没有成功。

```
vi package.json
```

``` package.json linenums="7"
		"start": "next start",
```

To

``` package.json linenums="7"
		"start": "next start -p 3100",
```

https://stackoverflow.com/questions/31502351/how-to-specify-a-port-number-for-pm2
https://medium.com/frontendweb/how-to-change-port-in-nextjs-1b99930bb81f

## rsync

```bash
dnf install rsync
```

## Grafana

↪ [通过 Grafana 使用 CLS](https://cloud.tencent.com/document/product/614/52102)

