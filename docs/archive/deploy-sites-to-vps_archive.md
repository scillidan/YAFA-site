---
title: "部署网站到VPS（归档）"
date:
    created: 2023-05-01
    updated: 2023-05-01
keywords: >
    Deploy Site, VPS, Ubuntu, Nginx
locale: en
---

## Rocky Linux or Ubuntu

=== "Rocky Linux"

    ```bash
    hostnamectl
    dnf update -y
    ```

    If you need migrating To Rocky Linux:

    ```bash
    dnf update -y
    wget https://raw.githubusercontent.com/rocky-linux/rocky-tools/main/migrate2rocky/migrate2rocky.sh
    chmod u+x migrate2rocky.sh
    ./migrate2rocky.sh -V
    ./migrate2rocky.sh -r
    sudo reboot
    ```
		↪ [How to Migrate to Rocky Linux from CentOS Stream, CentOS, Alma Linux, RHEL, or Oracle Linux](https://docs.rockylinux.org/guides/migrate2rocky)

=== "Ubuntu"

    ```bash
    sudo apt-get update
    sudo apt-get upgrade -y
    ```

## Apache or Nginx

=== "Rocky Linux"

	  ```bash
	  dnf install httpd php -y
	  systemctl enable httpd
	  ```
		↪ [Apache web server multiple site setup](https://docs.rockylinux.org/guides/web/apache-sites-enabled)

=== "Ubuntu"

    ```bash
    sudo apt-get install nginx -y
    sudo service nginx restart
    systemctl daemon-reload
    ```
    ↪ [How to Install Nginx on CentOS 8](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-centos-8)

## Edit sites.conf

=== "Rocky Linux"

    ```bash
    mkdir -p /var/www/example.com
    vi /etc/httpd/conf.d/example.com.conf
    ```

    ``` title="example.com.conf"
    <VirtualHost *:80>
      ServerName example.com
      ServerAlias www.example.com
      # ServerName www.demo.example.com
      # ServerAlias demo.example.com
      DocumentRoot /var/www/example.com
      DirectoryIndex index.php index.htm index.html
      ErrorLog /var/log/httpd/example.com-error.log
      CustomLog /var/log/httpd/example.com-access.log combined
    </VirtualHost>
    ```

    ```bash
    sudo systemctl restart httpd
    sudo systemctl status httpd
    ```
		↪ [Apache web server multiple site setup](https://docs.rockylinux.org/guides/web/apache-sites-enabled)  
		↪ [Encrypt Apache Webserver with Let's Encrypt SSL Certificate on Rocky Linux 8 - How to do it ?](https://linuxapt.com/blog/706-encrypt-apache-webserver-with-lets-encrypt-ssl-certificate-on-rocky-linux-8)

=== "Ubuntu"

    ```bash
    sudo mkdir /etc/nginx/vhost
    sudo mkdir /var/www/example.com
    sudo vi /etc/nginx/vhost/example.com.conf
    ```

    ```bash title="example.com.conf"
    server {
        listen 80;
        server_name www.example.com example.com;
        root /var/www/example.com;
        index index.html index.htm index.php;

        error_page 403 /403.html;
        error_page 404 /404.html;
        error_page 500 501 502 503 504 /50x.html;

        location = 403.html {
            internal;
        }
        location = /404.html {
            internal;
        }
        location = /50x.html {
            internal;
        }
    }
    ```

    ```bash
    sudo vi /etc/nginx/nginx.conf
    ```

    ``` title="nginx.conf"
    http {
            …
            include /etc/nginx/vhost/*.conf;
    }
    ```

    ```bash
    sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_bak
    sudo vi /etc/nginx/sites-available/default
    ```

    Type `:.,$d` in command mode to delete all, then write:

    ``` title="default"
    server {
        listen 80 default_server;
        server_name _;
        return 404;
    }
    ```

    ```bash
    sudo systemctl restart httpd
    ```

## SSL 

=== "Rocky Linux"

    ```bash
    sudo dnf install epel-release -y
    sudo dnf install certbot python3-certbot-apache mod_ssl -y
    sudo systemctl restart httpd
    sudo systemctl status httpd
    sudo firewall-cmd --permanent --zone=public --add-service=http
    sudo firewall-cmd --permanent --zone=public --add-service=https
    sudo firewall-cmd --reload
    sudo firewall-cmd --list-all
    sudo certbot --apache example.com, www.example.com, demo.example.com
    crontab -e
    ```

    ```
    0 0 * * * /usr/bin/certbot renew → /dev/null 2>&1
    ```

    ```bash
    sudo certbot renew --dry-run
    sudo certbot --apache delete -d example1.com
    ```
		↪ [Secure Apache with Let’s Encrypt Certificate on Rocky Linux](https://www.tecmint.com/secure-apache-with-ssl-in-rocky-linux)

=== "Ubuntu"

    ```bash
    sudo apt-get install software-properties-common
    sudo add-apt-repository ppa:certbot/certbot
    sudo apt-get update
    sudo apt-get install python-certbot-nginx
    sudo certbot --authenticator webroot --installer nginx
    sudo certbot --nginx -d example.com
    sudo certbot renew --dry-run
    ```
		↪	[How To Secure Nginx with Let's Encrypt on CentOS 8](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-centos-8)

## git init (on PC)

```sh
git config --global user.name "yourname"
git config --global user.email youremail
git config --list
ssh-keygen -t rsa -C ".email"
ssh -T git@github.com
```

Copy content from `C:\Users\yourname\.ssh\id_rsa.pub`, then open https://github.com/settings/keys and paste in.  

Open https://github.com/new, then create a repository.

=== "General"

    ```sh
    mkdir yoursite
    cd yoursite
    git init
    git remote add origin git@github.com:xxx/xxx.git
    git remote add vps git@yourvpsip:/home/git/example.com.git
    ```

    Edit `.git/config`, add:

    ``` title="config"
    [remote "origin"]
        …
        url = git@yourvpsip:/home/git/example.com.git
        fetch = +refs/heads/*:refs/remotes/vps/*
    ```

    Edit `.gitignore`, add:

    ``` title=".gitignore"
    node_modules/
    ```

=== "Hexo"

    Edit `yoursite/_config.yml`:

    ```yaml title="_config.yml"
    deploy:
      - type: git
        repo: git@github.com:yourname/repo.git
        branch: main
      - type: git
        repo: git@yourvpsip:/home/git/example.com.git
        branch: main
    ```

## Edit post-receive

Add a user:

```bash
sudo adduser git
sudo passwd git
sudo chown git:git -R /var/www/example.com
su git
cd ~
mkdir .ssh
touch .ssh/authorized_keys
vim .ssh/authorized_keys, paste in
ssh git@yourvpsip
```

```bash
sudo yum install git -y
cd ~
mkdir example.com.git
cd example.com.git
git config --global init.defaultBranch main
git init --bare
cd hooks
touch post-receive
chmod +x post-receive
vi post-receive
```

``` title="post-receive"
#!/bin/bash -l
GIT_REPO=/home/git/example.com.git
TMP_GIT_CLONE=/tmp/example.com
PUBLIC_WWW=/var/www/example.com
rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_WWW}/*
cp -rf ${TMP_GIT_CLONE}/apath/* ${PUBLIC_WWW} // Fill in with path that like public, site, docs
# cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}
```

↪ [Deploy Hexo sites to VPS](https://scillidan.github.io/repo_page/Deploy%20Hexo%20sites%20to%20VPS.md.html)

## git push (on PC)

```sh
git add .
git commit -m "text"
git push -u origin master
```
