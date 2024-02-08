---
title: "部署网站到VPS (2024)"
date:
    created: 2024-01-27
    updated: 2024-01-29
keywords: >
    Deploy Site, VPS, Rocky Linux, Apache , Ubuntu, Nginx
locale: en
draft: true
---

如果你有一堆创作或者记录，无论是文字、图片还是音视频，为此搭建一个个人网站，你便能够像走进房间一样，随意走动、按喜好布置、偶尔清扫。

部署网页到VPS，这是一个由来古老的可选项。相比流行的「托管式」——例如通过[Github Pages](https://pages.github.com/)等服务，或者[Vercel](https://vercel.com)、[Netlify](https://app.netlify.com/)等平台来构建、部署，「」。它的过程十分繁琐。

但将一些经常被省略了的，或者说这个「古代技术」的历史透露。好在你是一个有困难，但不坏的选择。除非你能手搓一个网页生成器，不然去到流行的网页框架如[Hexo](https://hexo.io)、[Hugo](https://gohugo.io)

等等的主题市场，挑选模板是很好的选择。官方文档通常太过详细，适合排查错误或者了解。大部分模板的使用，要不是作者很久没有更新了

个人网站能够能够被你你的长久居所，

Hexo里更多的是博客模板，我最早使用[Icarus](https://github.com/ppoffice/hexo-theme-icarus)，在几年的使用里，我掌握了一部分的HTML。[Stellar](https://github.com/xaoxuu/hexo-theme-stellar)。

Hugo中有相对丰富的模板种类，例如图墙模板[Gallery Deluxe](https://github.com/bep/gallerydeluxe)，特别的食谱模板[recipe-book](https://github.com/rametta/recipe-book)等等。也通常更易于进行私人定制。

我个人使用[Hugo Book Theme](https://github.com/alex-shpak/hugo-book)，[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)，并打算[Vivliostyle](https://vivliostyle.org/)。

根据生成器的官方文档，以及所选主题的指导，你可以在PC上预览网页。要部署网页时，很流行的方式是使用。

如果你打算购买VPS并部署网页，可以参考下文。主要涉及「多个二级域名」、「开启HTTPS」

它的最终目的地是以「本地构建，云端部署」或者「远程构建并部署」的方式同时更新Github Pages和VPS。在过程中，经验包是沿线散布而单独的，例如「重装VPS系统」「新装Linux系统后更新软件包」「新建子用户」等等。基本上，我对照着2021年的实况笔记和各个文档、手册进行操作。官方文档描述一个操作时，有时就像一部字典解释词义、语法，但它并不和你正在书写的东西和上下文。。这也是有时候，我通过看一些视频，来排查错误。而我新掌握了——气急败坏地删除一些报「危」的文件。

## 首个劝退者

!!! repotemplate "「旧笔记」"

    这里沿用了2021年的笔记，使用的是腾讯云VPS。随着平台后续更新，具体的操作可能已经变化，但流程没多大问题。

1. 注册腾讯云帐号 → 实名认证
2. 云服务器 → 立即选购
3. 云产品 → DNS解析DNSPod → 域名注册
4. 域名 → 操作 → 解析 → 记录管理 → 添加记录 ...
5. 网站备案 → 我的备案 → 网站信息 → 新增网站 ...
6. 留意核验短信

## VPS安装系统

我最早使用[Ubuntu](https://ubuntu.com/) + [Nginx](https://www.nginx.com/)的组合，听说了[CentOS](https://www.centos.org/) + [Apache](https://www.apache.org/)的性能会更好。[Rocky Linux](https://rockylinux.org)则是一个由社区支持的CentOS的代替品。

腾讯云 → 控制台 → 云服务器 → 实例 → 目标实例 → 操作系统 → 重装系统 → 重装配置:

```
镜像配置 公共镜像
目标镜像 Rocky Linux，最新版
登录方式 设置密码
用户名 `root`
新密码 `YourPassword`
确认密码 `YourPassword`
```

## 登录VPS（管理员）

目标实例 → IP地址 `YourHost (公)` → 复制

我使用的SSH工具箱是[WinSSHTerm](https://winsshterm.blogspot.com/)。

WinSSHTerm → Connections → Connect → Add Connection:

```
Name `vps`
Host/IP `YourHost`
port `22`
User `root`
Password `YourPassword`
```

双击`vps` → PuTTY Secutity Alert → Accept

## 更新系统软件包

显示系统信息：

```bash
hostnamectl
```

更新所有软件包，确认时默认`Yes`：

```bash
dnf update -y
```

## Apache上配置（多）站点

安装Apache：

```bash
dnf install httpd php -y
```

Rocky Linux的文档中，为了防止一个网站出问题时影响其他网站，采用了稍稍复杂的文件结构：

```bash
mkdir -p /etc/httpd/sites-available /etc/httpd/sites-enabled
```

使用[Vim](https://www.vim.org/)编辑器编辑Apache的配置文件：

```bash
vi /etc/httpd/conf/httpd.conf
```

可按`PageDown`定位到末尾。在新的一行，按`i`进入`Insert`模式。可复制下面这行，在WinSSHTerm中可以右键粘贴。：

```
IncludeOptional /etc/httpd/sites-enabled
```

按`Esc`退回`Normal`模式。按`:`进入`Command`模式，继续输入`wq`，即`write`并`quit`。

但是这里将配置文件新建在`sites-available/`，之后通过系统链接可选择地新建到`sites-enabled/`。文件夹中，以`com.Example.www`, `com.Example.SubDomain`(子域名)的风格来命名，也没毛病。

这里，文档假设了：主网站的文件放在`/var/www/html/`，各个子网站的放在`/var/www/sub-domains/SubDomain/html/`。

新建并配置(主)域名。粘贴之前，根据你的实际域名，替换`Example.com`, `SubDomain`等示例名：

```bash
vi /etc/httpd/sites-available/com.Example
```

```
<VirtualHost *:80>
    ServerName www.Example.com
    ServerAlias Example.com
    DocumentRoot /var/www/html
    DirectoryIndex index.php index.htm index.html

    CustomLog "/var/log/httpd/access_log" combined
    ErrorLog  "/var/log/httpd/error_log"

    <Directory /var/www/html>
        Options -ExecCGI -Indexes
        AllowOverride None

        Order deny,allow
        Deny from all
        Allow from all

        Satisfy all
    </Directory>
</VirtualHost>
```

新建并配置二级域名：

```bash
vi /etc/httpd/sites-available/com.Example.SubDomain
```

```
<VirtualHost *:80>
    ServerName SubDomain.Example.com
    DocumentRoot /var/www/sub-domains/SubDomain/html
    DirectoryIndex index.php index.htm index.html

    CustomLog "/var/log/httpd/SubDomain-access_log" combined
    ErrorLog  "/var/log/httpd/SubDomain-error_log"

    <Directory /var/www/sub-domains/SubDomain/html>
        Options -ExecCGI -Indexes
        AllowOverride None

        Order deny,allow
        Deny from all
        Allow from all

        Satisfy all
    </Directory>
</VirtualHost>
```

新建配置中指出的文件夹：

```bash
mkdir -p /var/www/html
```

```bash
mkdir -p /var/www/sub-domains/SubDomain/html
```

启动Web服务：

```bash
systemctl start httpd
systemctl enable httpd
```

通过新建(或删除)系统链接，来控制Web服务开启（或关闭）站点。这里是开启：

```bash
ln -s /etc/httpd/sites-available/com.Example /etc/httpd/sites-enabled/
ln -s /etc/httpd/sites-available/com.Example.SubDomain /etc/httpd/sites-enabled/
```

每次编辑过站点设置，或者开关站点后，重启Web服务：

```bash
systemctl restart httpd
```

去到腾讯云 → 控制台 → 云解析DNS → 我的解析 → 目标域名/解析 → 添加记录:

对于域名，添加两条记录：

```
主机记录 `@`和`www`
记录类型 `A`
记录值 `YourHost`
```

对于子域名，新增记录：

```
主机记录 `SubDomain`
记录类型 `A`
记录值 `YourHost`
```

在浏览器中进行确认，将看到：

![]()


↪ [Apache web server multiple site setup](https://docs.rockylinux.org/guides/web/apache-sites-enabled/)  
↪ [What ServerAlias for my Subdomain?](https://www.linode.com/community/questions/10436/what-serveralias-for-my-subdomain)

## 防火墙设置

防火墙内打开一些服务和端口。

```bash
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --permanent --add-port=443/tcp
firewall-cmd --reload
firewall-cmd --list-all
```

↪ [How to install Apache on Rocky Linux 9](https://www.linuxteck.com/how-to-install-apache-on-rocky-linux/)
↪ [firewalld for Beginners](https://docs.rockylinux.org/guides/security/firewalld-beginners/)
↪ [mod_ssl on Rocky Linux in an Apache web](https://docs.rockylinux.org/guides/web/mod_SSL_apache/)

## 配置SSL证书

使用[certbot](https://certbot.eff.org/)来配置域名的SSL证书，需要安装额外的软件包。

```bash
dnf install epel-release -y
dnf install certbot python3-certbot-apache mod_ssl -y
systemctl restart httpd
```

申请证书：

```bash
certbot --apache
```

回答3个问题：

1. 填入一个你的邮箱来接收重要信息
2. 阅读并接受用户协议的条款。我没有读，你可以读一读
3. 是否愿意分享你的邮箱地址给它的一个合作伙伴，非盈利机构。也可以`Y(es)`

如果顺利，这里会列出Web服务中开启的域名：

```bash
1: Example.com
2: www.Example.com
3: SubDomain.Example.com
```

输入数字，以空格间隔即可，`1 2 3`。如果是以后新增一个域名，这里只要输入新域名的数字应该就行了。

只是测试一下证书的续订功能是否正常：

```bash
certbot renew --dry-run
```

↪ [Generating SSL keys - Let's Encrypt](https://docs.rockylinux.org/guides/security/generating_ssl_keys_lets_encrypt/)

## 休息时间

这时，如果你能够上传网页文件到对应的文件夹，就可以访问了。

这个步骤，网络上有许多不同的工具或脚本，能够以不同的方式做到。这里介绍依赖`git`的两种自动化部署方式：

1. 本地`build`网页文件，再上传到云端
2. 上传到云端，再进行`build`

## PC上使用Git

安装分布式版本控制系统[Git](https://git-scm.com/)。注册一个[GitHub](https://github.com/)账号。打开终端：

```sh
git config --global user.name "YourName"
git config --global user.email YourEmail
git config --list
ssh-keygen -t rsa -C "YourEmail"
```

复制`C:\Users\YourName\.ssh\id_rsa.pub`里的内容, 浏览器中打开Github的[SSH and GPG keys](https://github.com/settings/keys)设置，「New SSH key」，粘贴进去。密钥的名字可以填写，并表示了是你的某一台计算机。

测试与GitHub的SSH连接。这里的`ssh.exe`位于Git安装目录的`\usr\bin\`内：

```sh
ssh -T git@github.com
```

## PC上添加远程Repo

这个方法来自[Deploy Hexo sites to VPS](https://scillidan.github.io/repo_page/Deploy%20Hexo%20sites%20to%20VPS.md.html)。原文链接已经失效，我在[Internet Archive](https://archive.org/)上找到了存档并保存了副本。虽然写于2015年，但是方法易懂、实用。

新建一个[Repository](https://github.com/new)。Repo默认「公开」，对所有人可见，这样也能够使用[Github Pages](https://pages.github.com/)服务。名称可填写你的网页的名字。

在你使用网页生成器生成文件夹后，进入文件夹，新建一个本地Repo：

```sh
git init
```

加入远程Repo。这里的`origin`是后文的目标远程Repo的默认简称：

```sh
git remote add origin git@github.com:YourName/YourRepo.git

加入VPS上的Repo，命名为`vps`。这里，VPS上还没有`SubUser`这个用户，和`com.Example.git`文件夹，稍后会新建。

```sh
git remote add vps SubUser@YourHost:/home/SubUser/com.Example.git
```

（应该）对于任何的本地Repo，在`git init`后，都可以通过修改配置文件，来在`git push`时，同时`push`到Github和VPS。编辑`.git/config`，增加两行：

```
[remote "origin"]
    ...
    url = SubUser@YourHost:/home/git/com.Example.git
    fetch = +refs/heads/*:refs/remotes/vps/*
```

编辑`.gitignore`，添加需要忽略的文件和文件夹。例如对于某些网页生成器，需要添加:

```
node_modules/
```

如果你使用[Hugo](https://gohugo.io/)，并且没有使用相关的[GitHub Action](https://github.com/marketplace)自动化部署任务，可以使用这个方法。

如果你使用[Hexo](https://hexo.io/zh-cn/index.html)和部署插件[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)，可编辑Hexo配置文件`_config.yml`，添加远程Repo:

```yaml
deploy:
  - type: git
    repo: git@github.com:YourName/Repo.git
    branch: main
  - type: git
    repo: SubUser@YourHost:/home/git/com.Example.git
    branch: main
```

## VPS上新建子账户

回到VPS。增加一个SubUser（子用户），命名为`git`:

```bash
adduser git
```

设置子账户的登录密码：

```bash
passwd git
```

给予`git`账户，网页文件夹`/var/www/html`的权限：

```bash
chown git:git -R /var/www/html
```

切换到`git`账户：

```bash
su git
```

为子账户设置SSH访问：

```bash
cd ~
mkdir .ssh
touch .ssh/authorized_keys
vim .ssh/authorized_keys
```

粘贴PC上的`C:\Users\YourName\.ssh\id_rsa.pub`里的内容，保存并退出。

回到PC。测试以`git`账户登录VPS：

```bash
ssh git@YourHost
```

## 新建Hook脚本

VPS上安装Git：

```bash
su root
yum install git -y
```

账户目录下新建一个仅协作用的Repo：

```bash
cd ~
mkdir com.Example.git
cd com.Example.git
git config --global init.defaultBranch main
git init --bare

新建会被触发的Bash脚本：

```bash
cd hooks
touch post-receive
chmod +x post-receive
vi post-receive
```

```
#!/bin/bash -l
GIT_REPO=/home/git/com.Example.git
TMP_GIT_CLONE=/tmp/com.Example
PUBLIC_WWW=/var/www/html
rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_WWW}/*
cp -rf ${TMP_GIT_CLONE}/BuildDir/* ${PUBLIC_WWW}
```

这里的`BuildDir`是网页文件所在的目录名，常见的有`public`、`site`、`docs`等。对于使用[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)部署的分支，这里可能需要改成`cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}`。

如果你在这个步骤出错，可尝试重新删除相关文件夹，再重新配置。

## 上传到Repo（PC上）

暂存当前目录和子目录的所有更改：

```sh
git add .
```

描述一下这次更改，准备提交：

```sh
git commit -m "SomeMessage"
```

第一次提交时，需指定远程仓库名和分支名，加上`-u origin main`：

```sh
git push -u origin main
```

在较长的一段时间里，我都在使用的这个方法更新VPS上的页面。我现在更多地使用着Action任务，但这个方法依然还有一个使用场景。稍微调整本地Repo的设置后，能够将上传到GitHub和上传到VPS分开。这样在更新Repo后，也可以不同时更新网页。

##

## 腾讯云CDN服务配置

控制台 → 访问管理 → 用户/用户列表 → 新建用户 → 快速创建 ... 成功新建用户 → 下载CSV文件
打开访客窗口或无痕窗口 → 登录界面/子账号 → 腾讯云 → 右上角 → 填写账号ID, 初始密码 → 重置密码 → 完善关联手机信息
主账号 → 控制台 → 访问管理 → 用户/用户设置 → 目标子账号 → 授权 → 搜索`COS`并全选条目 → 搜索`CDN`并全选条目 → 确定
子账号 → 控制台 → 访问管理 → API密钥管理 → 新建密钥 → 创建SecretKey → 下载CSV文件 → 我已知晓并保存SecretKey (On) → 确定

主账号 → 控制台 → 内容分发网路 → 添加域名 → 域名配置 → 验证方法 ... 验证

```
加速区域 全球
加速类型 CDN网页小文件
    当你文件类型是html, css, js等网页文件、Web字体、vtt字幕等时，选择此类型。这只是经验，不是标准。
    是音频、视频文件时，选择CDN音视频点播
源站类型 COS源
源站类型 目标COS桶
```

下一步 ... 提升访问性能 → 智能压缩 (Off)
下一步 → 防止费用超额 → 配置封顶配置 → 生效下方配置项 (On) → 新增规则:

```
统计类型 累计用量
统计周期 每小时
流量封顶 流量封顶 10GB
解封时间 永不解封
告警阈值 开启
```

```
统计类型 瞬间用量
封顶配置 流量封顶 1536MB(1.5GB)
解封时间 永不解封
```
... 提交所有配置

配置CNAME → 一键配置 → 确定


如果你在重启`httpd`或者设置SSL时出错，通常跟`/etc/httpd/conf/httpd.conf`和`/etc/httpd/sites-available/`里的站点配置有关

按提示`systemctl status httpd.service`后，可尝试删除报错的文件，然后重试。
步骤被分割得细碎，如果你在某个地方卡壳，可以向前退回到某个还没报错节点
