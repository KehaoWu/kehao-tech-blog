---
layout: post
title: "Ubuntu 16上安装yourls"
author: Kehao Wu
tags:
 - Ubuntu
 - YOURLS
---


## 安装mariadb、nginx、php、php-pfm

```
sudo apt-get -y install php7.0-cli php7.0-fpm php7.0-mysql mariadb-client-10.0 mariadb-server-10.0 nginx
```

## 设置mysql密码

```
sudo mysql_secure_installation
```

设置完密码后如果不能用root正确登录，可以切换到root用户后进入mysql执行以下命令

```
update mysql.user set plugin='' where User='root';
flush privileges;
```

## 克隆YOURLS

```
cd /var/www/html/
sudo git clone https://github.com/YOURLS/YOURLS
```

## 修改配置

```
cd /var/www/html/YOURLS
sudo cp user/config-sample.php user/config.php
```

将以下内容改为服务器对应配置

假设我们的数据库用户为`root`，密码为`hello123`，数据库名为`yourls`，网站域名为`qq.ai`，那么我们的配置应为：

```
define( 'YOURLS_DB_USER', 'root' );
define( 'YOURLS_DB_PASS', 'hello123' );
define( 'YOURLS_DB_NAME', 'yourls' );
define( 'YOURLS_DB_HOST', 'localhost' );
define( 'YOURLS_DB_PREFIX', 'yourls_' );
define( 'YOURLS_SITE', 'http://qq.ai' );
define( 'YOURLS_COOKIEKEY', 'ksissksisksk' );
```

#### 设置管理员用户

假设我们的管理员用户为 `admin_999`，密码为 `admin_12931`，那么对应的配置为：

```
$yourls_user_passwords = array(
        'admin_999' => 'admin_12931'
	);
```

## 配置nginx

```
## 这里不配置https
server {

  # HTTP over IPv4 & IPv6
  listen 80;
  listen [::]:80;

  # HTTPS over IPv4 & IPv6
  # MUST BE EDITED TO REFLECT YOUR CONFIGURATION
  #listen 443 ssl;
  #listen [::]:443 ssl;
  #ssl_certificate     example.com.crt;
  #ssl_certificate_key example.com.key;

  # Server names
  # MUST BE EDITED TO REFLECT YOUR CONFIGURATION
  server_name qq.ai;

  # Root directory
  # MUST BE EDITED TO REFLECT YOUR CONFIGURATION
  root /var/www/html/YOURLS;

  # Rewrites
  location / {
    try_files $uri /yourls-loader.php$is_args$args;
  }

  # PHP engine
  location ~ \.php$ {
    include fastcgi.conf;
    # OR
    # include fastcgi_params;
    
    fastcgi_index index.php;
    
    # MUST BE EDITED TO REFLECT YOUR CONFIGURATION
    fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
  }

}
```

## 如果遇到无法打开网站的错误，请尝试修改数据库schema

这个错误的对应nginx日志为：
```
2018/08/16 16:43:19 [error] 15312#15312: *5455985 FastCGI sent in stderr: "PHP message: PHP Fatal error:  Uncaught PDOException: SQLSTATE[42000]: Syntax error or access violation: 1071 Specified key was too long; max key length is 767 bytes in /var/www/html/YOURLS/includes/vendor/aura/sql/src/ExtendedPdo.php:748
```

修改 include/function-install.php

```
        $create_tables[YOURLS_DB_TABLE_URL] =
                'CREATE TABLE IF NOT EXISTS `'.YOURLS_DB_TABLE_URL.'` ('.
                '`keyword` varchar(200) BINARY NOT NULL,'.
                '`url` text BINARY NOT NULL,'.
                '`title` text CHARACTER SET utf8,'.
                '`timestamp` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,'.
                '`ip` VARCHAR(41) NOT NULL,'.
                '`clicks` INT(10) UNSIGNED NOT NULL,'.
                ' PRIMARY KEY  (`keyword`),'.
                ' KEY `timestamp` (`timestamp`),'.
                ' KEY `ip` (`ip`)'.
                ') ENGINE=MyISAM;';

        $create_tables[YOURLS_DB_TABLE_OPTIONS] =
                'CREATE TABLE IF NOT EXISTS `'.YOURLS_DB_TABLE_OPTIONS.'` ('.
                '`option_id` bigint(20) unsigned NOT NULL auto_increment,'.
                '`option_name` varchar(64) NOT NULL default "",'.
                '`option_value` longtext NOT NULL,'.
                'PRIMARY KEY  (`option_id`,`option_name`),'.
                'KEY `option_name` (`option_name`)'.
                ') AUTO_INCREMENT=1 ENGINE=MyISAM;';

        $create_tables[YOURLS_DB_TABLE_LOG] =
                'CREATE TABLE IF NOT EXISTS `'.YOURLS_DB_TABLE_LOG.'` ('.
                '`click_id` int(11) NOT NULL auto_increment,'.
                '`click_time` datetime NOT NULL,'.
                '`shorturl` varchar(200) BINARY NOT NULL,'.
                '`referrer` varchar(200) NOT NULL,'.
                '`user_agent` varchar(255) NOT NULL,'.
                '`ip_address` varchar(41) NOT NULL,'.
                '`country_code` char(2) NOT NULL,'.
                'PRIMARY KEY  (`click_id`),'.
                'KEY `shorturl` (`shorturl`)'.
                ') AUTO_INCREMENT=1 ENGINE=MyISAM;';
```

## 添加.htaccess文件

```
# BEGIN YOURLS
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^.*$ /yourls-loader.php [L]
</IfModule>
# END YOURLS
```

## 访问管理页面

http://qq.ai/admin/index.php