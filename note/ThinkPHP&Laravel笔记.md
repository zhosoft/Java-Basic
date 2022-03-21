

# ThinkPHP 部分

**Elasticsearch-PHP 中文文档** :https://learnku.com/docs/elasticsearch-php/6.0

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