```php
# 运行命令
php artisan make:middleware EnableCrossRequestMiddleware


# 中间件
<?php
namespace App\Http\Middleware;
use Closure;
class EnableCrossRequestMiddleware
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request $request
     * @param  \Closure $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        $response = $next($request);
        $origin = $request->server('HTTP_ORIGIN') ? $request->server               ('HTTP_ORIGIN') : '';
        $allow_origin = [
            'http://localhost:8000', // 允许跨域的列表了，可自行修改
        ];
        if (in_array($origin, $allow_origin)) {
            $response->header('Access-Control-Allow-Origin', $origin);
            $response->header('Access-Control-Allow-Headers', 'Origin,             Content-Type, Cookie, X-CSRF-TOKEN, Accept, Authorization,             X-XSRF-TOKEN');
            $response->header('Access-Control-Expose-Headers', 'Authorization,             authenticated');
            $response->header('Access-Control-Allow-Methods', 'GET, POST, PATCH,                PUT, OPTIONS');
            $response->header('Access-Control-Allow-Credentials', 'true');
        }
        return $response;
    }
}



# 在内核文件注册该中间件(在 App\Http\Kernel 类的 $middleware 属性添加，这里注册的中间件属于全局中间件)

protected $middleware = [
    // more
    \App\Http\Middleware\EnableCrossRequestMiddleware::class,
];

```