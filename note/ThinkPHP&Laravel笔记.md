

# ThinkPHP 部分

**Elasticsearch-PHP 中文文档** :https://learnku.com/docs/elasticsearch-php/6.0

Restful:

http://www.ruanyifeng.com/blog/2014/05/restful_api.html
http://www.ruanyifeng.com/blog/2011/09/restful.html

镜像网址：

https://packagist.org/

## 事务

> 使用事务处理的话，需要数据库引擎支持事务处理。比如 `MySQL` 的 `MyISAM` 不支持事务处理，需要使用 `InnoDB` 引擎。

```php
Db::transaction(function () {
    Db::table('think_user')->find(1);
    Db::table('think_user')->delete(1);
});
```

也可以手动控制事务，例如：

```php
// 启动事务
Db::startTrans();
try {
    Db::table('think_user')->find(1);
    Db::table('think_user')->delete(1);
    // 提交事务
    Db::commit();
} catch (\Exception $e) {
    // 回滚事务
    Db::rollback();
}
```

> 添加数据后如果需要返回新增数据的自增主键，可以使用`insertGetId`方法新增数据并返回主键值：

```php
$userId = Db::name('user')->insertGetId($data);//返回添加数据的自增主键
```



```php
// 软删除数据 使用delete_time字段标记删除
Db::name('user')
	->where('id', 1)
	->useSoftDelete('delete_time',time())
    ->delete();
```

```php
Db::table('user')->fetchSql(true)->find(1);
```

> 不要在控制器中使用包括`die`、`exit`在内的中断代码。如果你需要调试并中止执行，可以使用系统提供的`halt`助手函数。

使用枚举类（类似与java枚举类）

composer require phpenum/phpenum

https://blog.csdn.net/u012981882/article/details/106844872/



## 验证器

> unique:table,field,except,pk

验证当前请求的字段值是否为唯一的

```php
// 表示验证name字段的值是否在user表（不包含前缀）中唯一
'name'   => 'unique:user',
// 验证其他字段
'name'   => 'unique:user,account',
// 排除某个主键值
'name'   => 'unique:user,account,10',
// 指定某个主键值排除
'name'   => 'unique:user,account,10,user_id',

```

如果需要对复杂的条件验证唯一，可以使用下面的方式：

```php
// 多个字段验证唯一验证条件
'name'   => 'unique:user,status^account',
// 复杂验证条件
'name'   => 'unique:user,status=1&account='.$data['account'],
```

登录场景

```php
$data = $request->param();
$result = Validate::rule([
	"username" => 'unique:admin,username^password',
])->check([
    'username' => $data['username'],
    'password' => MD5Util::passwordEncrypt($data['password'])
]);
```

## 请求信息

```
由于无法获取映射的controller、action名称，换个思路，采取$request->baseUrl()+$request->method()进行权限验证
```

```php
//获取的是当前操作多应用模式当前应用名称
$app = app('http')->getName();
//获取的是当前操作控制器方法的实际名称
$controller = Request::controller();
//获取的是当前操作方法的实际名称
$action = Request::action();
//获取的是当前操作动词的实际名称
$method = Request::method();
$name = $app . DIRECTORY_SEPARATOR . $controller . DIRECTORY_SEPARATOR . $action;
```

# Restful api

> - GET /zoos：列出所有动物园
> - POST /zoos：新建一个动物园
> - GET /zoos/ID：获取某个指定动物园的信息
> - PUT /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息）
> - PATCH /zoos/ID：更新某个指定动物园的信息（提供该动物园的部分信息）
> - DELETE /zoos/ID：删除某个动物园
> - GET /zoos/ID/animals：列出某个指定动物园的所有动物
> - DELETE /zoos/ID/animals/ID：删除某个指定动物园的指定动物