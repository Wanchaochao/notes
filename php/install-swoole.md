# Mac 下 swoole 安装

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