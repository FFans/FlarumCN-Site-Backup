## Flarum 安装指南
**关键词**：Flarum 安装, Flarum 安装教程, Flarum 安装指南, Flarum 中文教程, Flarum 中文指南 
**主要内容**：服务器环境要求，安装命令，运行目录，文件夹权限设置，创建数据库，URL 重写规则（Apache/Nginx/Caddy），Composer 源，自定义路径，注册邮件设置，扩展插件，面板安装等

>  💡 **全文编排解释: Yannis & Triangle - 2021/10/10**
>  
> **Flarum 1.0** 正式版已发布，经过 6 年的洗礼，测试版结束了其任务。
> 请不要在生产中使用 **Flarum Beta** ，除非你知道你正在做什么以及有能力修复漏洞。
> 如果您发现 Flarum 存在安全漏洞，请发送邮件至官方邮件 security@flarum.org 进行反应。


> ❌ 安装前或提问前请 **仔细阅读** 指南或在 [FAQ](https://discuss.flarum.org.cn/d/325) 和 [**求助**](https://discuss.flarum.org.cn/t/support) 分类下查找类似案例，并了解[《提问的智慧》精读注解版](https://ld246.com/article/1536377163156)。
> ⭕ 迫不得已，请在 [**求助**](https://discuss.flarum.org.cn/t/support) 分类中提问能更快得到帮助。

>如果您发现指南中存在语法错误或者漏洞，请及时指出，
感谢以下朋友们指出错误或提及建议，或许有其他伙伴未能在这里出现，请留言告诉我：
@"Dikkoo"#2402、@"undo"#2444 、@"自媒体酷"#3788、@没胡子的猫



> ### 环境配置

### Composer 安装与支持

重要提示：在安装之前，请务必检查服务器是否满足以下要求

### 1. Composer 安装与支持

Flarum 需要基于 Composer 容器，这在后续的升级和安装扩展等内容上至关重要，

请务必在安装之前对服务器或其提供商进行调查，是否支持 Composer 依赖项，

截至 2020 年 10 月 24 号，Composer 已从 v1 版本更新迭代至 v2 !

**🔦 相关链接：**[Composer](https://getcomposer.org/download/) | [Composer 中文文档 | Composer 中文网](https://docs.phpcomposer.com/)

#### 1.1 我如何判断服务器安装了 Composer

```
composer -v
```
查看 Composer 版本命令
如果以上命令不能运行，则需要你手动安装或者通过面板自动化进行安装。

####  1.2 我能否使用 zip/rar 的形式安装

>  🧁 注意：安装与管理 Flarum 的唯一官方支持方式是通过 Composer

不能。过去，社区框架往往通过用户自行下载并上传的形式来安装管理，这虽然看起来简单，却无法及时有效对代码更新迭代，甚至在多站点管理，自动部署以及测试等方面上造成不必要的麻烦和未知冲突。所以，使用 Composer 可以更好的处理所有这些问题。

####   1.3 我的共享主机不支持 SSH 怎么办

大多数共享主机都会提供 SSH 访问权限，如果服务商屏蔽了，请及时和他们联系并说明情况。如果不支持，请及时切换服务商！

------
### 服务器推荐要求配置
### 2. 服务器推荐要求配置
如果服务器不满足要求，Flarum 可能无法安装且正常运行。

- **SSH** 命令功能
- **Apache** 或 **Nginx** 功能
- **PHP 7.3** 版本以上
- 数据库 **MySQL** **5.6** 版本以上 或 **MariaDB** **10.0.5** 版本以上

#### 2.1 PHP 功能支持及函数解禁
##### 2.1.1 PHP 必要功能 fileinfo、exif
根据你的社区后续功能拓展，可能会需要用到除上方外其他的 PHP 功能扩展
curl, dom, gd, json, mbstring, openssl, pdo_mysql, tokenizer, zip

#####  2.1.2 PHP 必要函数 putenv、proc_open
运行命令时，请额外注意错误提示，是否有其他依赖函数未解禁，

> **经典函数未解禁案例：**
> ` Installing flarum/flarum (v1.0.0): Downloading (100%) The Process class relies on proc_open, which is not available on your PHP in stallation.`


----

> ### 安装


### 清空指定目录

提示：进行安装之前，请确保已阅读上方有关配置的内容。

### 3. 清空指定目录

清空指定目录来安装 Flarum 程序，其中包含目录下的隐藏项，

运行 `rm` 命令务必注意，反复确认其在指定目录下运行，否则特别危险和致命！！！

#### 3.1 删除命令部分详解

| 命令 | 功能作用                                      |
| :--: | :-------------------------------------------- |
|  -f  | 强制删除                                      |
|  -r  | 递归， 删除指定目录下内容并包含其子目录及文件 |
|  *   | 所有文件                                      |
|  /*  | 目录下的所有文件                              |
| /.*  | 目录下的所有隐藏文件                          |


### Flarum 安装命令
### 4. 使用 Composer 命令安装

```
composer create-project flarum/flarum .
```

### 调整 Webroot 及文件权限
### 5. 调整 Webroot 及文件权限

#### 5.1 调整 Webroot 运行目录

确认 Webroot 运行目录为 `/public` 文件夹

- Apache 配置中目录定向到 `/public` 

```
<Directory "/<Flarum 路径>/public">
    AllowOverride All
</Directory>
```

- Nginx 全站配置中 root 目录定向到 `/public`

```
......
root /www/wwwroot/xxx.com/public;
```
可以根据实际修改路径 /www/wwwroot/xxx.com

  
#### 5.2 文件权限

⛔ 请勿将任何文件权限设置为 **777** ，该权限允许任何人随意访问文件内容，严重威胁安全。

调整 `public`、`storage`、`vendor` 文件夹以及子目录下所有文件权限为 `755`，且所有者为 `www` 组别。默认下，文件均为 root 权限，当外网访问时，www-data 用户组别无权限查看，故需用 chown 更改文件夹权限组。

```
chown -R www-data:www-data /www/wwwroot/xxx.com
```
可以根据实际修改路径 /www/wwwroot/xxx.com
```
chmod -R 755 /www/wwwroot/xxx.com/public
```
可以根据实际修改路径 /www/wwwroot/xxx.com
755 权限对 /public、/storage、/vendor 文件夹及子目录下所有文件

### 数据库
### 6. 数据库
#### 6.1 登陆 MySQL

```
mysql -u root -p
```
若出现错误，请善于搜索和学习后再进行提问
#### 6.2 创建数据库

```
create database 数据库名 character set utf8mb4 collate utf8mb4_unicode_ci;
```
为什么使用 utf8mb4 而不是 utf8 ，因为，utf8是一个有限版本，只适用于部分字符，不适用于表情符号。后来便有了 utf8mb4，但 MySQL 必须保持向后兼容，所以这就是为什么添加了一个新的编码而不是修复旧的。所以，这里建议所有新的数据库都应该使用 utf8mb4 编码。

### 完成安装，请重启服务器
### 7. 完成安装后，请重启服务器


-------


> ### Apache/Nginx

### Apache

提示：如果完成后并没有生效，记得重启你的 Apache/Nginx ！

## Apache

请确认服务器启用 `mod_rewrite` 模块或 `.htaccess` ，否则将无法正常运行。

`/pubilc` 目录下存在 `.htaccess` 文件，此文件包含了需要用到的定向规则。

在配置中加入以下内容启用定向，将运行目录设置为 `/public` 。

```makefile
<Directory "/<Flarum 路径>/public">
    AllowOverride All
</Directory>
```


### Nginx
## Nginx



安装目录下存在 `.nginx.conf` 文件，只需将该文件包括在全站重定向配置中即可，

使用 `include` 包含该文件即可：

```makefile
include /www/wwwroot/xxx.com/.nginx.conf;
```
可以根据实际修改目录路径 /www/wwwroot/xxx.com

由于某些服务商不支持，可以将 `.nginx.conf` 文件内容复制并插入到全站配置中，

注意，这里不是全覆盖，而是在最后插入 `.nginx.conf` 文件中的定向内容：
### .nginx.conf
```
# Pass requests that don't refer directly to files in the filesystem to index.php
location / {
  try_files $uri $uri/ /index.php?$query_string;
}

# Uncomment the following lines if you are not using a `public` directory
# to prevent sensitive resources from being exposed.
# location ~* ^/(\.git|composer\.(json|lock)|auth\.json|config\.php|flarum|storage|vendor) {
#   deny all;
#   return 404;
# }

# The following directives are based on best practices from H5BP Nginx Server Configs
# https://github.com/h5bp/server-configs-nginx

# Expire rules for static content
location ~* \.(?:manifest|appcache|html?|xml|json)$ {
  add_header Cache-Control "max-age=0";
}

location ~* \.(?:rss|atom)$ {
  add_header Cache-Control "max-age=3600";
}

location ~* \.(?:jpg|jpeg|gif|png|ico|cur|gz|svg|mp4|ogg|ogv|webm|htc)$ {
  add_header Cache-Control "max-age=2592000";
  access_log off;
}

location ~* \.(?:css|js)$ {
  add_header Cache-Control "max-age=31536000";
  access_log off;
}

location ~* \.(?:ttf|ttc|otf|eot|woff|woff2)$ {
  add_header Cache-Control "max-age=2592000";
  access_log off;
}

# Gzip compression
gzip on;
gzip_comp_level 5;
gzip_min_length 256;
gzip_proxied any;
gzip_vary on;
gzip_types
    application/atom+xml
    application/javascript
    application/json
    application/ld+json
    application/manifest+json
    application/rss+xml
    application/vnd.geo+json
    application/vnd.ms-fontobject
    application/x-font-ttf
    application/x-web-app-manifest+json
    application/xhtml+xml
    application/xml
    font/opentype
    image/bmp
    image/svg+xml
    image/x-icon
    text/cache-manifest
    text/css
    text/plain
    text/vcard
    text/vnd.rim.location.xloc
    text/vtt
    text/x-component
    text/x-cross-domain-policy;
```



-------

> ### 其他

### 扩展插件
请在扩展分类中寻找：[中文社区](https://discuss.flarum.org.cn/t/extensions) | [英文社区](https://discuss.flarum.org/t/extensions)

或者在非官方的 [Extensions](https://extiverse.com/) 扩展数据库页面中搜寻。
#### 安装扩展

与 Flarum 一样，扩展是使用 SSH 通过 [Composer](https://getcomposer.org/) 安装的。要安装一个典型的扩展：

1. `cd` 到 `composer.json` 所在文件夹。
2. 运行 `composer require COMPOSER_包名` 具体安装命令一般可在扩展的文档中找到。
3. 卸载 `composer remove COMPOSER_包名`

#### 管理扩展

后台管理面板的「扩展」页面提供了一种在安装后便捷管理扩展程序的途径。您可以：

- 启用或禁用一个扩展
- 访问扩展设置（有些扩展会在主侧边栏添加标签页进行设置）。
- 回滚一个扩展的迁移，以删除它所做的任何数据库修改（这可以通过卸载按钮来完成）。这将删除与该扩展相关的所有数据，并且是不可逆的。只有当你要删除一个扩展程序，并且不打算再次安装它时，才应该这样做。当然这不是非要做的事情，选择权在您手中。




### 面板安装
- 宝塔 Linux 面板安装 Flarum：https://discuss.flarum.org.cn/d/2195  / 由 @itnt 提供
- Linux OneinStake 安装：https://ryanc.cc/archives/flarum-install-and-config / 由 [@Ryanc](https://bbs.halo.run) 提供


### Composer 源切换 (一般使用默认源)
默认源有时候会出现安装等待时间过长等问题，可切换至国内源，这里以阿里举例：

全局切换阿里源：
```
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
```

全局取消配置：
```
composer config -g --unset repos.packagist
```


### 自定义路径

默认情况下 Flarum 的目录结构，包含公开访问的文件 `public` 目录。 这是最好最安全的，确保所有敏感的源代码文件无法从网站根目录完全访问。


但, 如果你希望将 Flarum 安装在一个子目录中（例如 `你的网站.com/forum`），或者如果你的主机不让你安装在自己的网站根目录下 (受制于 `public_html` 或 `htdocs` 之类的东西），则可以进行设置没有 `public` 目录的 Flarum。

只需将 public 目录（包括`.htaccess`）中的所有文件移动到你要安装 Flarum 的目录中。 然后编辑 `.htaccess` 并取消注释第 9 ~ 14 行用来保护敏感资源的 Code 。 最后，编辑 index.php 和 Flarum 可执行文件，并更新以下行中的路径以反映你的新目录结构：
```
require 'vendor/autoload.php';
```
```
'base' => __自定义__,
'public' => __自定义__,
'storage' => __自定义__.'/storage',
```


### 注册邮件设置
每个社区都需要发送电子邮件给用户，用来激活账户、重置密码、接收通知以及与其他用户通讯交流。作为论坛管理员，您首先要做的几件事之一就是配置好论坛的邮件服务！配置错误的话，用户在注册时会收到报错。

#### 支持的邮件驱动

Flarum 默认提供以下所列驱动，若有需要，开发者可自行开发插件添加 [自定义邮件驱动](https://docs.flarum.org/zh/extend/mail)。

#### SMTP

这是最常用的邮件驱动，需要您配置主机地址、端口、加密方式、用户名和密码，以使用外部 SMTP 服务。请注意，加密方式必须为小写的 `ssl` 或 `tls`。

#### Mail

`mail` 会用到许多托管服务器上都有的 sendmail / postfix 邮件系统。您必须在服务器上正确安装并配置好 sendmail 才能正常工作。

#### Mailgun

通过您的 [Mailgun](https://www.mailgun.com/) 来发送邮件。您需要填写 secret key，以及您在 Mailgun 配置的域名、区域。

#### Log

日志模式下 **不会有任何邮件被发出**，该模式多在开发者调试时使用，在这个模式下，任何邮件内容都会被写入日志中（日志路径：`Flarum根目录/storage/logs`）。

#### 测试邮件

在您保存邮件配置后，您可以在后台管理面板的邮件配置页面点击「发送测试邮件」发送一封邮件以检测您的配置是否可用。如果提示错误，或者您没有收到测试邮件，那就意味着配置有误，请检查配置然后重试。如果没有任何报错，邮件也发送出去了，但是您在收件箱中找不到测试邮件，请去垃圾箱看看，它很可能在那边。
> 若你的邮件设置全部正确，但是无法发送邮件或出现无法注册错误，
> 请重新检查邮箱的用户名和密码是否正确，或切换邮箱服务商重试，
> 推荐163邮箱或其他域名邮箱
相关链接：[163 FAQ](http://help.163.com/09/1223/14/5R7P6CJ600753VB8.html)

