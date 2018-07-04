



修改 .env 中 BROADCAST_DRIVER 配置项为 pusher：

```
	BROADCAST_DRIVER=pusher
```

尽管 Laravel 开箱支持 Pusher，但是我们还是需要安装对应的 PHP SDK：

```
	composer require pusher/pusher-php-server
```

设置 Pusher 凭证信息
访问 Pusher 官网，注册并登录到用户后台，创建一个新的 Channels App https://pusher.com


#### 前端资源初始化
使用 Laravel Mix 来编译前端 CSS 和 JavaScript
```
npm install
```
此外，Laravel 还提供了 JavaScript 库 Laravel Echo 来订阅和监听事件：

```
npm install --save laravel-echo pusher-js
```


安装完成，还要告知 Laravel Echo 使用 Pusher，Laravel 已经在 resources/assets/js/bootstrap.js 中为我们提供了该实现，只不过默认注释起来了，只需要取消这段注释即可：

```
import Echo from 'laravel-echo'

window.Pusher = require('pusher-js');

window.Echo = new Echo({
    broadcaster: 'pusher',
    key: process.env.MIX_PUSHER_APP_KEY,
    cluster: process.env.MIX_PUSHER_APP_CLUSTER,
    encrypted: true
});
```

#### 用户认证脚手架代码

设定只有登录用户才能进入聊天室进行聊天，为了简化流程，我们使用 Laravel 默认的用户认证功能：
```

php artisan make:auth
php artisan migrate

```


#### 业务代码实现

##### 消息模型
首先要为发送的消息创建一个模型类及其对应数据库迁移文件：
```
php artisan make:model Message -m
# 在新生成的 app/Messaage 模型类中新增下面这行代码以方便批量赋值：
/**
 * Fields that are mass assignable
 *
 * @var array
 */
protected $fillable = ['message'];

#然后在 databases/migrations 目录下编写刚生成的 messages 对应迁移文件的 up 方法：
Schema::create('messages', function (Blueprint $table) {
    $table->increments('id');
    $table->integer('user_id')->unsigned();
    $table->text('message');
    $table->timestamps();
});

#最后执行迁移命令生成数据表 messages：
php artisan migrate

```




### 注
最终实现了，但是效果不太好，而且还收费，不太推荐
原文地址 url http://laravelacademy.org/post/9408.html


























