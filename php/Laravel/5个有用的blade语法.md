
##### 1. 检测用户是否认证你可以通过验证用户是否为空来检测其是否认证：

```php
@if(auth()->user())
    // 用户已认证
@endif
```

然而，Laravel 自带的 Blade 命令可以更简洁地实现相同的功能：
```php
@auth
    // 用户已认证
@endauth
```

##### 2. 检测用户是否为访客与认证相反，我们可以用 auth 辅助函数的 guest() 方法来检测用户是否为访客：
```php
@if(auth()->guest())
    // 用户未认证
@endif
```
不过 Laravel 也为此提供了 @guest 命令：
```php
@guest
    // 用户未认证
@endguest
```
我们也可以使用 else 语句来组合这两个命令：@guest
    // 用户未认证
@else
    // 用户已认证
@endguest
##### 3. 如果第一个视图存在则引入，否则引入第二个构建多主题站点可能会有一个文件如果存在就引入，否则就引入另一个的需要，你可以简单地使用条件判断来实现：
```php
@if(view()->exists('first-view-name'))
    @include('first-view-name')
@else
    @include('second-view-name')
@endif
```
不过还是有一个更简洁直观的命令来做这件事：
```php
@includeFirst(['first-view-name', 'second-view-name']);
```
##### 4. 根据条件引入视图当你只想在一定逻辑的基础上（如：一个已通过认证的用户）添加一些内容的时候，根据条件引入视图就非常有用了。你可以使用 @if 条件来这样写：
```php
@if($post->hasComments())
    @include('posts.comments')
@endif
```
我们可以只用一行命令 @includeWhen 来做到：
```php
@includeWhen($post->hasComments(), 'posts.comments');
```
##### 5. 引入一个存在的视图如果你有自定义主题系统或者你需要动态地创建 Blade 视图，那么检查文件是否存在就是必须要做的。可以在辅助函数 view() 上调用 exists 方法：
```php
@if(view()->exists('view-name'))
    @include('view-name')
@endif
```
也可以使用 Blade 命令  includeIf 来处理：
```php
@includeIf('view-name')
```