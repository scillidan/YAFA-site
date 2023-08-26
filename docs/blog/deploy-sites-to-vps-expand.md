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

[Hexo 腾讯云CDN主动刷新插件](https://github.com/Techeek/hexo-deploy-tencentcloud-cdn)
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

README.md中提到了`yarn dev`命令。这里用`package.json`中写的另外的`script`：

```
yarn build
yarn start
```

想要了解`yarn dev`和`yarn start`的区别，我找到了一段描述，来自帖子[What's the difference between npm run dev and npm run start in Next.js?](https://stackoverflow.com/questions/69400243/whats-the-difference-between-npm-run-dev-and-npm-run-start-in-next-js)。附上机翻（腾讯翻译君）：

> 太长不看：在Next.js中，`Next dev`命令用于在开发模式下运行应用程序。另一方面，`Next start`命令用于在生产模式下运行应用程序，但需要首先运行`Next Build`命令以生成优化的生产版本。
>
> **TL;DR:** In Next.js, `next dev` is used to run the app in development mode. On the other hand, `next start` is used to run the app in production mode, but requires `next build` to be run first to generate an optimized production build.

再搜一下Development和Production的区别。出自帖子 [Difference between production and development build in ReactJS](https://stackoverflow.com/questions/48151128/difference-between-production-and-development-build-in-reactjs)，同样附上机翻：

> 使用开发构建--顾名思义--是出于开发原因。在这些版本中，您拥有源代码映射、调试和经常热重新加载的能力。
  另一方面，生产构建在生产模式下运行，这意味着这是在您的客户端机器上运行的代码。生产构建运行uglify并将源文件构建到一个或多个最小化文件中。它还提取css和图像，当然还有你用webpack加载的任何其他来源。此外，还不包括热重装。根据您的webpack开发工具设置，源地图可能会作为单独的文件包含在内。
>
> The development build is used - as the name suggests - for development reasons. You have Source Maps, debugging and often times hot reloading ability in those builds.
> The production build, on the other hand, runs in production mode which means this is the code running on your client's machine. The production build runs uglify and builds your source files into one or multiple minimized files. It also extracts CSS and images and of course any other sources you're loading with Webpack. There's also no hot reloading included. Source Maps might be included as separate files depending on your webpack `devtool` [settings](https://webpack.js.org/configuration/devtool/).

但我在运行时遇到了`error`信息，根据这个回答[error Delete `··` prettier/prettie](https://github.com/prettier/eslint-plugin-prettier/issues/219#issuecomment-835770395)，需要先：

```
yarn lint --fix
```

这次运行成功后。按`Ctrl + C`退出。

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

尝试：

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

