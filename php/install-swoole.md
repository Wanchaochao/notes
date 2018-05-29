### Mac 下 swoole 安装

```php

git clone git@github.com:swoole/swoole-src.git
cd swoole-src
git submodule update --init thirdparty/libco
phpize
./configure
make -j 
sudo make install

php --ini  // Loaded Configuration File:         /usr/local/etc/php/7.1/php.ini

cd /usr/local/etc/php/7.1/php.ini
vim php.ini
extension=swoole.so

sudo brew services restart nginx

php -m | grep swoole // swoole

// 安装完成

```

### linux 下 swoole 安装

####Swoole安装 安装完PHP后，即可安装swoole扩展。 

swoole扩展下载地址：https://github.com/swoole/swoole-src/releases 尽量选择stable版本，alpha版本最好仅用于实验新特性。 解压源码至任意目录，执行如下命令：

```php

cd swoole-src-swoole-1.7.6-stable/
phpize
./configure --enable-async-mysql
sudo make
sudo make install

```
(注：swoole的./configure有很多额外参数，可以通过./configure --help命令查看，这里仅开启其中async-mysql项，其他均选择默认项) 安装完成后，进入/etc/php目录下，打开php.ini文件，在其中加上如下一句：

extension=swoole.so
随后在终端中输入命令

php -m
查看扩展安装情况。如果在列出的扩展中看到了swoole，则说明安装成功。