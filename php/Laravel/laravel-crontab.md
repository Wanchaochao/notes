### Laravel Cron 定时任务“跳坑”点

Laravel 中执行定时任务是通过 cron 来实现，官网文档中就是简单一句 + 一行Cron 代码

`* * * * * php /path-to-your-project/artisan schedule:run >> /dev/null 2>&1`

#### 坑1：环境变量

当Cron无法生效时，可能是Cron执行 **环境变量** 不正确引起的。

##### 执行命令

`env > /tmp/env.output`

打开/tmp/env.output文件，将PATH字段整行添加至corntab文件顶部，corntab文件在/var/spool/cron目录下

crontab 文件示例

```php
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/opt/mysql/bin:/opt/php7/bin:/opt/memcached/bin:/root/bin
 * * * * * php /path-to-your-project/artisan schedule:run >> /dev/null 2>&1
```

#### 坑2：Cron 执行用户导致 Laravel log 不可写

通过 **crontab -e** 命令创建的 Cron 是属于 **root** 用户，如果定时任务在实行时主动写入日志或者遇到异常未捕捉，会创建 root 权限的日志文件，最终会导致 php-fpm 的 www 账号无法写入。

因此需要在创建 cron 的时候**指定用户**

`crontab -u www -e`


#### 坑3：cron 内容最后一行未回车

解决上述两点问题后，如果仍然发现 cron 不执行，请确认

`* * * * * php /path-to-your-project/artisan schedule:run >> /dev/null 2>&1`