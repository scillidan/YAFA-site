---
title: "部署网站到VPS"
date:
    created: 2024-01-27
    updated: 2024-01-29
keywords: >
    Deploy Site, VPS, Rocky Linux, Apache , Ubuntu, Nginx
locale: en
---

如果你有大量的记录或者创作，无论是文字、图片还是音视频，为此搭建一个个人网站，你便可以在其中任意移动、偶尔清扫。

流行的方式是从「生成式网页」到「静态内容托管」。例如从[Hexo](https://hexo.io)、[Hugo](https://gohugo.io)等网页框架选择一或多个主题模板，通过[Github Pages](https://pages.github.com/)等服务，或者[Vercel](https://vercel.com)、[Netlify](https://app.netlify.com/)等平台来构建和部署站点。

Hexo中更多的是博客模板，我最早使用了几年的[Icarus](https://github.com/ppoffice/hexo-theme-icarus)，也较喜欢[Stellar](https://github.com/xaoxuu/hexo-theme-stellar)的风格。Hugo中有相对丰富的主题，例如图墙模板的[Gallery Deluxe](https://github.com/bep/gallerydeluxe)，食谱模板的[recipe-book](https://github.com/rametta/recipe-book)等等，也易于修改。也不一定是静态网页和个人站点，但这是一个很好的入门选择。

而「部署到VPS」属于古代技术，成为了可选项。在不考虑外部资源（CDN、Web字体等等）的情况下，这将一步一步地手工地搭起一个原生的环境，所以它理应有很好的性能表现。

部署的过程繁琐、易出错，主要涉及「多个二级域名」、「开启HTTPS」等操作。以及通用的单独的知识点，例如「更新软件包」「新建子用户」「Git的简单操作」等等。最终，以「本地构建，云端部署」或是「远程构建并部署」的方式，同时地去更新Github Pages和VPS。

## 选购VPS

!!! repotemplate "「旧笔记」"

    这里沿用了2021年的笔记，使用的是腾讯云VPS。随着平台后续的更新，具体的操作可能已经变化，但流程差不太多。同样地，使用其他云服务商也可以。

1. 注册腾讯云帐号 → 实名认证
2. 购买云服务器
3. 域名注册
4. 域名 → 操作 → 解析 → 记录管理 → 添加记录 ...
5. 网站备案 → 我的备案 → 网站信息 → 新增网站 ...
6. 留意核验短信

备案需要预览站点，可以放在最后。完成后会有年审（吧），会检查（主域名的）页面的底部是否有备案号链接以及网站名称。

## VPS上安装系统

我最早使用[Ubuntu](https://ubuntu.com/) + [Nginx](https://www.nginx.com/)的组合，听说了[CentOS](https://www.centos.org/) + [Apache](https://www.apache.org/)的性能会更好。[Rocky Linux](https://rockylinux.org)则是一个由社区支持的CentOS的代替品。

腾讯云 → 控制台 → 云服务器 → 实例 → 目标实例 → 操作系统 → 重装系统 → 重装配置:

```
镜像配置 公共镜像
目标镜像 Rocky Linux（我目前用的是9.3）
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

![](winsshterm.png)

更新所有软件包，确认时默认`Yes`：

```bash
dnf update -y
```

## Apache上配置（多）站点

安装Apache：

```bash
dnf install httpd php -y
```

在Rocky Linux的文档中，为了防止一个网站出问题时影响其他网站，采用了稍复杂的文件结构。

```bash
mkdir -p /etc/httpd/sites-available /etc/httpd/sites-enabled
```

使用[Vim](https://www.vim.org/)编辑器编辑Apache的配置文件：

```bash
vi /etc/httpd/conf/httpd.conf
```

可按`PageDown`定位到末尾。在新的一行，按`i`进入`Insert`模式。可复制下面这行，在WinSSHTerm中可以右键粘贴。官方文档这里写的是`Include`，但是会报错。我删除了报错信息中提到的文件，并且参考上下文，修改了这一处。

```
IncludeOptional /etc/httpd/sites-enabled
```

按`Esc`退回`Normal`模式。按`:`进入`Command`模式，输入`wq`并回车，即`write(save)`并`quit`。

这里，文档将网站配置文件新建在了`sites-available/`，再通过系统链接，可选择地复制到`sites-enabled/`。文件夹中，以`com.Example.www`, `com.Example.SubDomain`（子域名）的风格来命名。且主域名的文件放在`/var/www/html/`，各个子域名的放在各自的`/var/www/sub-domains/SubDomain/html/`。

新建并配置（主）域名。粘贴之前，根据你的实际域名，替换掉`Example.com`, `SubDomain`等示例名：

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

新建文件夹：

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

通过新建（或删除）系统链接，来控制Web服务开启（或关闭）站点。这里是开启：

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

![](rocky-linux-apache.png)

↪ [Apache web server multiple site setup](https://docs.rockylinux.org/guides/web/apache-sites-enabled/)  
↪ [What ServerAlias for my Subdomain?](https://www.linode.com/community/questions/10436/what-serveralias-for-my-subdomain)

## 防火墙设置

配置防火墙永久地打开公域的80和443端口，可通过HTTP和HTTPS协议被访问。

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

使用[certbot](https://certbot.eff.org/)来配置域名的SSL证书。安装软件包：

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
3. 是否愿意分享你的邮箱地址给它的一个合作伙伴、非盈利机构。也可以`Y(es)`

如果顺利，这里会列出Web服务中开启的域名：

```bash
1: Example.com
2: www.Example.com
3: SubDomain.Example.com
```

输入数字，以空格间隔即可，`1 2 3`。如果是以后新增一个域名，这里只需输入新域名的数字。

只是测试一下证书的续订功能是否正常：

```bash
certbot renew --dry-run
```

↪ [Generating SSL keys - Let's Encrypt](https://docs.rockylinux.org/guides/security/generating_ssl_keys_lets_encrypt/)

## 本地Build、上传云端

到此，如果你能够上传网页文件到对应的文件夹，就可以访问了。

这个步骤，网络上有许多不同的方法和工具，参考[One-Command Deployment](https://hexo.io/docs/one-command-deployment)、[Hosting and Deployment](https://gohugo.io/categories/hosting-and-deployment/)。

这里使用了Git以及脚本工具来完成。

### PC上安装Git

安装分布式版本控制系统[Git](https://git-scm.com/)。注册一个[GitHub](https://github.com/)账号。打开终端，配置PC上的Git用户信息：

```sh
git config --global user.name "YourName"
git config --global user.email YourEmail
git config --list
```

生成SSH密钥对，用来与远程Repo通信：

```sh
ssh-keygen -t rsa -C "YourEmail"
```

复制`C:\Users\YourName\.ssh\id_rsa.pub`里的内容, 浏览器中打开Github的[SSH and GPG keys](https://github.com/settings/keys)设置，「New SSH key」，粘贴进去。密钥的名字随意，当你看到名字，能够知道是哪台设备即可。

测试与GitHub的SSH连接：

```sh
ssh -T git@github.com
```

这里的`ssh.exe`位于Git安装目录的`\usr\bin\`内，不是其他的`ssh.exe`，可以通过`where ssh`查看。

### PC上添加远程Repo

新建一个[Repository](https://github.com/new)。Repo默认「公开」，对所有人可见，这样也能够使用[Github Pages](https://pages.github.com/)服务。名称随意。

在你使用网页生成器生成文件夹后，进入文件夹，新建一个本地Repo：

```sh
git init
```

加入远程Repo。这里的`origin`是后文的目标远程Repo的默认简称：

```sh
git remote add origin git@github.com:YourName/YourRepo.git
```

### 添加VPS上的远程Repo

参考[One-Command Deployment](https://hexo.io/docs/one-command-deployment#Git)，如果你使用[Hexo](https://hexo.io/zh-cn/index.html)和部署插件[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)，只需要编辑Hexo配置文件`_config.yml`，添加一个Repo:

```yaml
deploy:
  - type: git
    repo: git@github.com:YourName/Repo.git
    branch: main
  - type: git
    repo: SubUser@YourHost:/home/git/com.Example.git
    branch: main
```

这个工具会在运行`hexo deploy`后上传`public/`里的所有内容。

没有专用的部署工具时，可采用手动的方法。方法来自[Deploy Hexo sites to VPS](https://scillidan.github.io/repo_page/Deploy%20Hexo%20sites%20to%20VPS.md.html)。原文写于2015年，链接已经失效，我在[Internet Archive](https://archive.org/)上找到了存档并保存了副本。

以Hugo站点为例，在添加了`github.com`上的Repo后，额外添加VPS上的Repo，可命名为`vps`。这里，VPS上还没有`SubUser`这个用户，和`com.Example.git`文件夹，稍后会新建。

```sh
git remote add vps SubUser@YourHost:/home/SubUser/com.Example.git
```

编辑`.git/config`，增加两行。在`git push`时，就会同时`push`到Github和VPS：

```
[remote "origin"]
    ...
    url = SubUser@YourHost:/home/git/com.Example.git
    fetch = +refs/heads/*:refs/remotes/vps/*
```

手动设置时，注意是否需要编辑`.gitignore`，添加要忽略的文件和文件夹。

### VPS上新建子用户

回到VPS。增加一个SubUser（子用户），命名为`git`:

```bash
adduser git
```

设置子用户的登录密码：

```bash
passwd git
```

递归地给予`git`用户网页文件夹的权限，例如`/var/www/html`的权限：

```bash
chown git:git -R /var/www/html
```

切换到`git`用户：

```bash
su git
```

为子用户设置用以SSH访问的密钥：

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

### 新建Hook脚本

VPS上安装Git：

```bash
su root
yum install git -y
```

账户目录下新建一个仅用以协作的Repo：

```bash
su git
cd ~
mkdir com.Example.git
cd com.Example.git
git config --global init.defaultBranch main
git init --bare
```

新建Bash脚本：

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

### 上传到Repo（PC上）

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

## 上传云端，再Build

只需要在「部署到Github Pages」的基础上，加入[SCP for GitHub Actions](https://github.com/appleboy/scp-action)里的关键工作流。

以Hugo站点为例，根据[Host on GitHub Pages](https://gohugo.io/hosting-and-deployment/hosting-on-github/)，在`.github/workflows`下创建`deploy.yaml`。复制一份`deploy.yaml`，重命名为`deploy_cps.yml`，编辑文件，替换「Deployment job」段落为：

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
    target: /var/www/sub-domains/SubDomain/
```

去到你在Github上的Repo → Settings → Secrets and variables → Actions → New repository secret → 依次创建以上4个secret，填写Name和Secret：

```
VPS_HOST `YourHost`
VPS_PORT `22`
VPS_USERNAME `git`
VPS_PASSWORD `YourPassword`
```

对于其他网页框架，请查找并阅读文档中的Publish、Deploy说明，或者你使用的具体的[GitHub Actions](https://github.com/marketplace)。再举个例子，在Material for MkDocs的[Publishing your site](https://squidfunk.github.io/mkdocs-material/publishing-your-site/)部分提供了用于GitHub Actions的`deploy.yaml`内容，它提示了如果要手动部署，去看[MkDocs](https://www.mkdocs.org/)的[Deploying your docs](https://www.mkdocs.org/user-guide/deploying-your-docs)。对于部署到VPS，就属于Other Providers部分。最后，知道了需要修改`deploy.yaml`的：

```
- run: mkdocs gh-deploy --force
```

到：

```
- run: mkdocs build --clean
- run: mv site html
- name: Copy file to server
  uses: appleboy/scp-action@v0.1.7
  with:
    host: ${{ secrets.VPS_HOST }}
    username: ${{ secrets.VPS_USERNAME }}
    password: ${{ secrets.VPS_PASSWORD }}
    port: ${{ secrets.VPS_PORT }}
    source: "./html/*"
    target: /var/www/sub-domains/SubDomain/
```

上传你的更新：

```sh
git add .github\workflows\deploy.yml .github\workflows\deploy_scp.yml
git commit -m "test deploy_scp"
git push
```

到你在Github上的Repo的Actions栏，观察运行情况。如果顺利的话，将看到：

![](deploy_cps.png)