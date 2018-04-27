```php
echo date('w'); //返回当天的星期;数字0表示是星期天,数字123456表示星期一到六


# 匹配用户名(中文+字母+下划线)
return preg_match('/^[A-Za-z0-9_\x{4e00}-\x{9fa5}]+$/u',$string);
```

