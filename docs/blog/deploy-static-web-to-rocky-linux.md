---
title: "Rocky Linux部署静态网站：Apache、多二级域名、HTTPS、本地Build、云端Build"
created: 2024-01-27
modified: 2024-01-29
locale: zh
keywords: >
    Deploy Site, VPS, Rocky Linux, Apache
---

如果你有一些，或者正在进行着记录、创作，无论是文字、图片还是音频、视频——像档案一样归类、保存它们，再使用流行却陌生的「生成式网页」工具，从你的文本文件和媒体文件生成Web页面，上传到云端，你便可以在空中有一块土地。你可以一砖一瓦地建造，亦或是重建。也可以在其中任意移动、偶尔清扫。对你来说，它是你的文件的备份，历史内容的归档，归属关系的证明。对于别人来说，它是高可信度的来源，目录化、图形化的可读、可视事物。

从「生成式网页」到「静态内容托管」，是很好的入门选择。例如从[Hexo](https://hexo.io)、[Hugo](https://gohugo.io)等网页框架选择一或多个主题模板，通过[Github Pages](https://pages.github.com/)等服务，或者[Vercel](https://vercel.com)、[Netlify](https://app.netlify.com/)等平台来构建和部署站点。

Hexo中更多的是博客模板，我最早使用了几年的[Icarus](https://github.com/ppoffice/hexo-theme-icarus)，也较喜欢[Stellar](https://github.com/xaoxuu/hexo-theme-stellar)的风格。Hugo有相对丰富的主题，例如图墙模板的[AutoPhugo](https://github.com/kc0bfv/autophugo)、[Gallery Deluxe](https://github.com/bep/gallerydeluxe)、[Hugo Split Gallery](https://gitlab.com/tmuguet/hugo-split-gallery)，食谱模板的[recipe-book](https://github.com/rametta/recipe-book)等等，也较容易修改源码。

而「部署到VPS」几乎是一门古代技术，此文即是这条古老却生僻的路线的徒步记录。主要途径「多个二级域名」、「开启HTTPS」，以及「本地Build」「云端Build」两种部署方案，涉及到了不少Unix系统上的基础操作。

而我之前买的VPS再不使用的话，就要到期了。最终，在那样一个原生环境下，低性能的VPS配置理应会有相对良好的性能。具体情况下，它又受到了VPS性能、网络运营商、CDN资源（Web字体等）、大型媒体文件（图片、音频、视频）等因素的影响。

## 选购VPS

!!! repotemplate "「旧笔记」"

    这里沿用了2021年的笔记，使用的是腾讯云VPS。随着平台后续的更新，具体的操作可能已经变化，但流程差不太多。选择其他云服务商也没什么问题。

1. 注册腾讯云帐号 → 实名认证
2. 购买云服务器
3. 域名注册
4. 域名 → 操作 → 解析 → 记录管理 → 添加记录 ...
5. 网站备案 → 我的备案 → 网站信息 → 新增网站 ...
6. 留意核验短信

备案时，需要让站点页面可以被访问。这个步骤可以放在最后。完成备案后会有年审（吧），每次年审会检查（主域名的）页面的底部是否有备案号链接，以及网站名称是否和提交的信息一致。

## VPS上安装系统

我最早使用「[Ubuntu](https://ubuntu.com/) + [Nginx](https://www.nginx.com/)」的组合，后来听说「[CentOS](https://www.centos.org/) + [Apache](https://www.apache.org/)」的性能会更好。[Rocky Linux](https://rockylinux.org)则是一个由社区支持的CentOS的代替品。

腾讯云 → 控制台 → 云服务器 → 实例 → 目标实例 → 操作系统 → 重装系统 → 重装配置:

```
镜像配置 公共镜像
目标镜像 Rocky Linux（我当前用的是9.3）
登录方式 设置密码
用户名 `root`
新密码 `<your_password>`
确认密码 `<your_password>`
```

## 登录VPS

目标实例 → IP地址 `<your_host> (公)` → 复制

我使用的SSH工具箱是[WinSSHTerm](https://winsshterm.blogspot.com/)。也可以选择[WindTerm](https://github.com/kingToolbox/WindTerm)、[electerm](https://electerm.github.io/electerm/)、[MobaXterm](https://mobaxterm.mobatek.net/)等。

WinSSHTerm → Connections → Connect → Add Connection:

```
Name `vps`
Host/IP `<your_host>`
port `22`
User `root`
Password `<your_password>`
```

双击`vps` → PuTTY Secutity Alert → Accept

## 更新系统软件包

显示系统信息：

```sh
hostnamectl
```

![](winsshterm.png)

更新所有软件包，确认时默认`Yes`：

```sh
dnf update -y
```

## Apache上配置（多）站点 [^1][^2]

安装Apache：

```sh
dnf install httpd php -y
```

在Rocky Linux的文档中，为了防止一个网站出问题时影响其他网站，网页的配置采用了稍复杂的文件结构。

```sh
mkdir -p /etc/httpd/sites-available /etc/httpd/sites-enabled
```

使用[Vim](https://www.vim.org/)编辑器编辑Apache的配置文件：

```sh
vi /etc/httpd/conf/httpd.conf
```

按`PageDown`定位到末尾。按`i`进入`Insert`模式，在新的一行，粘贴下面这行。在WinSSHTerm中可以右键粘贴：

```
IncludeOptional /etc/httpd/sites-enabled
```

按`Esc`退回`Normal`模式。按`:`进入`Command`模式，继续输入`wq`并回车，即`write(save)`并`quit`。

这里，文档将网站的配置文件新建在了`sites-available/`下，再通过系统链接，可选择地复制到`sites-enabled/`。文件夹中，以`com.<example>.www`, `com.<example>.<sub_domain>`（子域名）的风格来命名。且主域名的文件放在`/var/www/html/`，各个子域名的放在各自的`/var/www/sub-domains/<sub_domain>/html/`。

进行以下操作时，根据你的实际域名，替换掉`<example>.com`, `<sub_domain>`等示例名。

新建并配置域名：

```sh
vi /etc/httpd/sites-available/com.<example>
```

```
<VirtualHost *:80>
    ServerName www.<example>.com
    ServerAlias <example>.com
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

```sh
vi /etc/httpd/sites-available/com.<example>.<sub_domain>
```

```
<VirtualHost *:80>
    ServerName <sub_domain>.<example>.com
    DocumentRoot /var/www/sub-domains/<sub_domain>/html
    DirectoryIndex index.php index.htm index.html

    CustomLog "/var/log/httpd/<sub_domain>-access_log" combined
    ErrorLog  "/var/log/httpd/<sub_domain>-error_log"

    <Directory /var/www/sub-domains/<sub_domain>/html>
        Options -ExecCGI -Indexes
        AllowOverride None

        Order deny,allow
        Deny from all
        Allow from all

        Satisfy all
    </Directory>
</VirtualHost>
```

新建文件夹：

```sh
mkdir -p /var/www/html
```

```sh
mkdir -p /var/www/sub-domains/<sub_domain>/html
```

启动Web服务：

```sh
systemctl start httpd
systemctl enable httpd
```

通过「新建或删除系统链接」，来开启或关闭站点。这里是开启：

```sh
ln -s /etc/httpd/sites-available/com.<example> /etc/httpd/sites-enabled/
ln -s /etc/httpd/sites-available/com.<example>.<sub_domain> /etc/httpd/sites-enabled/
```

每次编辑过站点配置，或者开关了站点后，重启Web服务：

```sh
systemctl restart httpd
```

去到腾讯云 → 控制台 → 云解析DNS → 我的解析 → 目标域名/解析 → 添加记录:

对于主域名，添加两条记录：

```
主机记录 `@`和`www`
记录类型 `A`
记录值 `<your_host>`
```

对于子域名，新增记录：

```
主机记录 `<sub_domain>`
记录类型 `A`
记录值 `<your_host>`
```

在浏览器中确认`<example>.com`、`<sub_domain>.<example>.com`。如果顺利，将看到：

![](rocky-linux-apache.png)

## 防火墙设置 [^3][^4][^5]

在防火墙上，永久地打开公域的80和443端口，对应访问网站使用的HTTP和HTTPS协议。

```sh
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --permanent --add-port=443/tcp
firewall-cmd --reload
firewall-cmd --list-all
```

## 配置SSL证书 [^6]

使用[certbot](https://certbot.eff.org/)来配置域名的SSL证书，启用HTTPS访问。安装软件包：

```sh
dnf install epel-release -y
dnf install certbot python3-certbot-apache mod_ssl -y
systemctl restart httpd
```

申请证书：

```sh
certbot --apache
```

回答3个问题：

1. 填入一个你的邮箱来接收重要信息
2. 阅读并接受用户协议的条款。我没有读，你可以读一读
3. 是否愿意分享你的邮箱地址给它的一个合作伙伴、非盈利机构

如果顺利，这里会列出Web服务中开启的域名：

```sh
1: <example>.com
2: www.<example>.com
3: <sub_domain>.<example>.com
```

输入数字，以空格间隔即可，如`1 2 3`。如果是以后新增一个域名，这里只需输入新域名的数字。

只是测试一下证书的续订功能是否正常：

```sh
certbot renew --dry-run
```

## 选择部署方式

到此，如果你能够上传网页文件到对应的文件夹，就可以进行访问了。这个步骤，网络上有许多不同的方法和工具。例如，对于Hexo，可参考[One-Command Deployment](https://hexo.io/docs/one-command-deployment)，对于Hugo，可参考[Hosting and Deployment](https://gohugo.io/categories/hosting-and-deployment/)。我试过[SiriKali](https://mhogomchungu.github.io/sirikali/)、[cwRsync](https://itefix.net/cwrsync)等等，最终使用[Gitbub Actions](https://github.com/marketplace)在云端进行构建，再部署到Gihub Pages和VPS。

## 本地Build

这个方法源自[Deploy Hexo sites to VPS](https://github.com/scillidan/PIDAN-journal/blob/main/blog/Deploy-Hexo-sites-to-VPS.md)。原文写于2015年，链接在几年前失效，我在[Internet Archive](https://archive.org/)上找到了存档并保存了副本。

### 安装和配置Git（PC上）

在PC上，安装分布式版本控制系统[Git](https://git-scm.com/)。注册一个[GitHub](https://github.com/)账号。打开终端，配置本机上的Git的用户信息：

```sh
git config --global user.name "YourName"
git config --global user.email YourEmail
git config --list
```

生成SSH密钥对，用来与远程Repo通信：

```sh
ssh-keygen -t rsa -C "YourEmail"
```

在浏览器中打开Github的[SSH and GPG keys](https://github.com/settings/keys)设置，选择「New SSH key」。复制`C:\Users\YourName\.ssh\id_rsa.pub`里的内容, 粘贴进去。密钥的名字随意，当你看到名字，能够知道是哪台设备即可。

测试与GitHub的SSH连接：

```sh
ssh -T git@github.com
```

这里的`ssh.exe`位于Git安装目录的`\usr\bin\`内，不是其他的`ssh.exe`。可以通过`where ssh`检查。

### 新建本地Repo（PC上）

新建一个[Repository](https://github.com/new)，名称可以与你的网站有关。Repo默认「公开」，对所有人可见，这样也便能够使用[Github Pages](https://pages.github.com/)服务。

参考你的网页生成器的官方文档，新建项目文件夹，例如`hexo init YourFolder`, `hugo new site YourFolder`等命令。另外也需要阅读一遍你选择的主题模板的说明。

进入文件夹，新建一个本地Repo：

```sh
cd YourFolder
git init
```

### 添加远程Repo（PC上）

首先，添加`github.com`上的远程Repo。这里的`origin`是随后的，目标远程Repo的默认的简称：

```sh
git remote add origin git@github.com:YourName/YourRepo.git
```

添加VPS上的远程Repo。这里，如果你使用[Hexo](https://hexo.io/zh-cn/index.html)和部署插件[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)，只需要编辑Hexo配置文件`_config.yml`，再添加一个Repo:

```yaml
deploy:
  - type: git
    repo: git@github.com:YourName/Repo.git
    branch: main
  - type: git
    repo: SubUser@<your_host>:/home/git/com.<example>.git
    branch: main
```

这样，在运行`hexo deploy`后，会同时上传`public/`里的内容，分别到`github.com`和VPS上的Repo。

没有专用的部署工具时，可采用手动的方法，以Hugo为例。编辑`.git/config`，在`origin`下面，额外加一个URL：

```
[remote "origin"]
	url = https://github.com/YourName/Repo.git
	fetch = +refs/heads/*:refs/remotes/origin/*
    url = SubUser@<your_host>:/home/git/com.<example>.git
    fetch = +refs/heads/*:refs/remotes/vps/*
```

这个操作实际上是添加了一个VPS上的远程Repo：

```sh
git remote add vps SubUser@<your_host>:/home/SubUser/com.<example>.git
```

但是组合进了`origin`，使得运行`git push -u origin main`时，同时上传到两个Repo。

手动设置时，注意是否需要编辑`.gitignore`，来添加要忽略的文件和文件夹。

### 新建子用户

回到VPS，新建上文提到的Repo。先添加一个SubUser（子用户），命名为`git`:

```sh
adduser git
```

设置子用户的登录密码：

```sh
passwd git
```

### 给予用户文件夹权限

递归地给予`git`用户网页文件夹的权限：

```sh
chown git:git -R /var/www/html
chown git:git -R /var/www/sub-domains/<sub_domain>/html
```

### 为子用户开启SSH访问

切换到`git`用户：

```sh
su git
```

为子用户设置用以SSH访问的密钥：

```sh
cd ~
mkdir .ssh
touch .ssh/authorized_keys
vim .ssh/authorized_keys
```

粘贴PC上的`C:\Users\YourName\.ssh\id_rsa.pub`里的内容，保存并退出。

回到PC，测试以`git`账户登录VPS：

```sh
ssh git@<your_host>
```

### 文件夹里新建Hook脚本

VPS上安装Git：

```sh
su root
yum install git -y
```

在子用户的`home/`目录里，新建仅用以协作的Repo：

```sh
su git
cd ~
mkdir com.<example>.git
cd com.<example>.git
git config --global init.defaultBranch main
git init --bare
```

`com.<example>.git`对应了`com.<example>`。`com.<example>.<sub_domain>.git`则对应`com.<example>.<sub_domain>`。

新建Hook脚本：

```sh
cd hooks
touch post-receive
chmod +x post-receive
vi post-receive
```

粘贴进去：

```sh
#!/bin/bash -l
GIT_REPO=/home/git/com.<example>.git
TMP_GIT_CLONE=/tmp/com.<example>
PUBLIC_WWW=/var/www/html
rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_WWW}/*
cp -rf ${TMP_GIT_CLONE}/BuildDir/* ${PUBLIC_WWW}
```

对应子域名是：

```sh
#!/bin/bash -l
GIT_REPO=/home/git/com.<example>.<sub_domain>.git
TMP_GIT_CLONE=/tmp/com.<example>.<sub_domain>
PUBLIC_WWW=/var/www/html/sub-domains/<sub_domain>/html
rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_WWW}/*
cp -rf ${TMP_GIT_CLONE}/BuildDir/* ${PUBLIC_WWW}
```

这是一个只有变量和命令的Bash文件，也只涉及了简单的命令。基本上，你需要留意`2-4`行的变量的值，以及最后一行。在最后一行，`BuildDir`是网页文件所在的目录，常见的目录名有`public`、`site`、`docs`等。如果你在PC上运行过构建命令，如`hexo build`、`hugo build`，将会看见。

如果使用了[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)，这行自然是：

```sh
cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}
```

### 从本地Repo上传（PC上）

运行了构建命名，生成了网页文件后，暂存当前目录和子目录的所有更改：

```sh
git add .
```

描述一下这次更改，准备好提交：

```sh
git commit -m "SomeMessage"
```

第一次提交时，需指定远程仓库名和分支名，加上`-u origin main`：

```sh
git push -u origin main
```

打开浏览器，访问`<example>.com`，查看页面是否更新。

## 云端Build

首先去取得「部署到Github Pages」的自动化脚本。

以Hugo站点为例，参考[Host on GitHub Pages](https://gohugo.io/hosting-and-deployment/hosting-on-github/)，在`.github/workflows`下新建`hugo.yaml`，或者重命名为`deploy.yml`。填入内容并保存。

复制这个文件，重命名新文件为`deploy_cps.yml`，编辑该文件。修改这里的`--baseURL`这行:

```yaml
run: |
  hugo \
    --minify \
    --baseURL "${{ steps.pages.outputs.base_url }}/"
```

到：

```yaml
--baseURL "https://<example>.com"
```

对应子域名是：

```yaml
--baseURL "https://<sub_domain>.<example>.com"
```

参考[SCP for GitHub Actions](https://github.com/appleboy/scp-action)，替换`Deployment job`以下的内容为：

```yaml
- name: Rename public to html
  run: mv public html
- name: Copy file to server
  uses: appleboy/scp-action@v0.1.7
  with:
    host: ${{ secrets.VPS_HOST }}
    username: ${{ secrets.VPS_USERNAME }}
    password: ${{ secrets.VPS_PASSWORD }}
    port: ${{ secrets.VPS_PORT }}
    source: "./html/*"
    target: /var/www/
```

对应子域名是：

```yaml
- name: Rename public to html
  run: mv public html
- name: Copy file to server
  uses: appleboy/scp-action@v0.1.7
  with:
    host: ${{ secrets.VPS_HOST }}
    username: ${{ secrets.VPS_USERNAME }}
    password: ${{ secrets.VPS_PASSWORD }}
    port: ${{ secrets.VPS_PORT }}
    source: "./html/*"
    target: /var/www/sub-domains/<sub_domain>/
```

去到你在Github上的Repo → Settings → Secrets and variables → Actions → New repository secret → 依次创建脚本中写到的4个secret。`Name`和`Secret`分别为：

```
VPS_HOST `<your_host>`
VPS_PORT `22`
VPS_USERNAME `git`
VPS_PASSWORD `<your_password>`
```

再举一个例子，在Material for MkDocs的[Publishing your site](https://squidfunk.github.io/mkdocs-material/publishing-your-site/)部分，也提供了脚本，并且提示去看[MkDocs文档](https://www.mkdocs.org/user-guide/deploying-your-docs)来进行手动部署。新建`deploy.yml`后，再复制一份，重命名为`deploy_cps.yml`，修改`deploy_cps.yml`的：

```yaml
- run: mkdocs gh-deploy --force
```

到：

```yaml
- run: mkdocs build --clean
- name: Rename site to html
  run: mv site html
- name: Copy file to server
  uses: appleboy/scp-action@v0.1.7
  with:
    host: ${{ secrets.VPS_HOST }}
    username: ${{ secrets.VPS_USERNAME }}
    password: ${{ secrets.VPS_PASSWORD }}
    port: ${{ secrets.VPS_PORT }}
    source: "./html/*"
    target: /var/www/
```

两个例子的最后，上传你的脚本并测试。直接在主分支上进行测试不是一个规范的操作，但问题不大：

```sh
git add .github\workflows\deploy.yml .github\workflows\deploy_scp.yml
git commit -m "test deploy_scp"
git push
```

到你在Github上的Repo的Actions栏，观察运行情况。如果顺利的话，将看到：

![](deploy_cps.png)

如果你未看到有任何信息，可能需要检查：

- Setting → Pages → Build and deployment/Source → Github Actions ...
- Actions → I understand my workflows, go ahead and enable them

!!! repotemplate "Bug"
    
    后来，我重新处理、压缩了[BYYA](https://github.com/scillidan/BYYA-site)上的所有图片。因为我不熟悉Git的某些操作，我删除并重新上传了整个Repo，也就遇到了[Issue #169](https://github.com/appleboy/scp-action/issues/169)中说明的问题。

## 小抄

最后，是一份小抄。如果你需要在VPS上，添加一个子域名，依次：

```sh
vi /etc/httpd/sites-available/com.<example>.<sub_domain>
```

```
<VirtualHost *:80>
    ServerName <sub_domain>.<example>.com
    DocumentRoot /var/www/sub-domains/<sub_domain>/html
    DirectoryIndex index.php index.htm index.html

    CustomLog "/var/log/httpd/demo-access_log" combined
    ErrorLog  "/var/log/httpd/demo-error_log"

    <Directory /var/www/sub-domains/<sub_domain>/html>
        Options -ExecCGI -Indexes
        AllowOverride None

        Order deny,allow
        Deny from all
        Allow from all

        Satisfy all
    </Directory>
</VirtualHost>
```

```sh
mkdir -p /var/www/sub-domains/<sub_domain>/html
ln -s /etc/httpd/sites-available/com.<example>.<sub_domain> /etc/httpd/sites-enabled/
systemctl restart httpd
```

打开浏览器，登录VPS的控制台，添加子域名的解析。

```sh
certbot --apache
```

```sh
chown git:git -R /var/www/sub-domains/<sub_domain>/html
su git
```

```sh
cd ~
mkdir com.<example>.<sub_domain>.git
cd com.<example>.<sub_domain>.git
git config --global init.defaultBranch main
git init --bare
cd hooks
touch post-receive
chmod +x post-receive
vi post-receive
```

```
#!/bin/bash -l
GIT_REPO=/home/git/com.<example>.<sub_domain>.git
TMP_GIT_CLONE=/tmp/com.<example>.<sub_domain>
PUBLIC_WWW=/var/www/html/sub-domains/<sub_domain>/html
rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_WWW}/*
cp -rf ${TMP_GIT_CLONE}/BuildDir/* ${PUBLIC_WWW}
```

```sh
git add .
git commit -m "test add sub-domain"
git push
```

[^1]: [Apache web server multiple site setup](https://docs.rockylinux.org/guides/web/apache-sites-enabled/)  
[^2]: [What ServerAlias for my Subdomain?](https://www.linode.com/community/questions/10436/what-serveralias-for-my-subdomain)
[^3]: [How to install Apache on Rocky Linux 9](https://www.linuxteck.com/how-to-install-apache-on-rocky-linux/)  
[^4]: [firewalld for Beginners](https://docs.rockylinux.org/guides/security/firewalld-beginners/)  
[^5]: [mod_ssl on Rocky Linux in an Apache web](https://docs.rockylinux.org/guides/web/mod_SSL_apache/)
[^6]: [Generating SSL keys - Let's Encrypt](https://docs.rockylinux.org/guides/security/generating_ssl_keys_lets_encrypt/)