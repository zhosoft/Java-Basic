>结合楠哥课程整理

# 安装 Redis

1、官网下载最新版 Redis 6.0.6

2、复制到 Linux

3、解压缩并安装

```shell
tar -xvf redis-6.0.6.tar.gz
cd redis-6.0.6
make install
```

报错：server.c:xxxx:xx: error: ‘xxxxxxxx’ has no member named ‘xxxxx’

原因：CentOS 7 默认安装 gcc 4.8.5，Redis 6 必须将 gcc 升级到 9.3

```
# 查看gcc版本是否在5.3以上，centos7.6默认安装4.8.5
gcc -v

# 升级gcc到5.3及以上,如下：
升级到gcc 9.3：
yum -y install centos-release-scl
yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils
scl enable devtoolset-9 bash
需要注意的是scl命令启用只是临时的，退出shell或重启就会恢复原系统gcc版本。
如果要长期使用gcc 9.3的话：

echo "source /opt/rh/devtoolset-9/enable" >>/etc/profile
这样退出shell重新打开就是新版的gcc了
以下其他版本同理，修改devtoolset版本号即可。
```

如果 yum 安装报错 yum 被 packagekit 占用

将 /etc/yum/pluginconf.d/refresh-packagekit.conf 改为如下

enabled=0

然后重启 Linux 即可

安装成功如下所示。

![image-20200508134657188](C:\Users\ningn\AppData\Roaming\Typora\typora-user-images\image-20200508134657188.png)

8、启动 Redis

```shell
cd src
./redis-server
```

![image-20200506145508970](C:\Users\ningn\AppData\Roaming\Typora\typora-user-images\image-20200506145508970.png)

9、配置为后台服务

修改 redis.conf 文件

```shell
cd ..
vim redis.conf
```

守护线程改为 yes 表示启动后台启动，保存退出

![image-20200506145912636](C:\Users\ningn\AppData\Roaming\Typora\typora-user-images\image-20200506145912636.png)

10、将 redis.conf 复制到 /etc/redis 路径下，并改名为 6379.conf

```shell
cd /etc
mkdir redis
cd redis
cp /usr/local/redis-6.0.6/redis.conf 6379.conf
```

11、将启动文件 usr/local/redis-6.0.6/utils/redis_init_script 拷贝到 /etc/rc.d/init.d/

```shell
cp /usr/local/redis-6.0.6/utils/redis_init_script /etc/rc.d/init.d/
```

修改文件名为 redisd

```shell
cd /etc/rc.d/init.d/
mv redis_init_script redisd
```

12、修改复制后的 redisd 文件，让它成为服务

```shell
cd /etc/rc.d/init.d/
vim redisd
```

- 修改 EXEC、CLIEXEC 的路径

```shell
#原内容
EXEC=/usr/local/bin/redis-server
CLIEXEC=/usr/local/bin/redis-cli

#修改后的内容
EXEC=/usr/local/redis-6.0.6/src/redis-server
CLIEXEC=/usr/local/redis-6.0.6/src/redis-cli
```

- 在 $EXEC $CONF 后面加上 &，表示后台启动

![image-20200506152536230](C:\Users\ningn\AppData\Roaming\Typora\typora-user-images\image-20200506152536230.png)

13、添加开机启动

```shell
chkconfig redisd on
```

14、启动 redis 服务

```shell
service redisd start
```

![image-20200506153910650](C:\Users\ningn\AppData\Roaming\Typora\typora-user-images\image-20200506153910650.png)

Ctrl + c 退出，Reids 也不会关闭了，执行命令查看

```shell
ps -ef | grep redis
```

![image-20200506154005213](C:\Users\ningn\AppData\Roaming\Typora\typora-user-images\image-20200506154005213.png)

可以看到 Redis 服务已经后台启动了

15、关闭 Redis 服务

```shell
service redisd stop
```

16、客户端访问

```shell
cd /usr/local/redis-6.0.6/src
redis-cli
```

![image-20200506154402798](C:\Users\ningn\AppData\Roaming\Typora\typora-user-images\image-20200506154402798.png)

17、允许外部访问

- 添加端口

1、查看防火墙状态

```shell
firewall-cmd --state
```

![image-20200429144736832](C:\Users\ningn\AppData\Roaming\Typora\typora-user-images\image-20200429144736832.png)

runing 表示开启，not runing 表示关闭，如果关闭，执行

```shell
systemctl start firewalld.service
```

开放 6379 端口

```shell
firewall-cmd --zone=public --add-port=6379/tcp --permanent
systemctl restart firewalld.service
firewall-cmd --reload
```

- 修改配置文件

```shell
vim /etc/redis/6379.conf
```

![image-20200506155651916](C:\Users\ningn\AppData\Roaming\Typora\typora-user-images\image-20200506155651916.png)

bind 改为 0.0.0.0 表示任何 IP 都可以连接。









# Redis 基本操作

Redis 默认有 16 个数据库，使用的是第 0 个，切换数据库

```
select 0
```

添加数据/修改数据

```
set key value
```

查询数据

```
get key
```

批量添加

```
mset k1 v1 k2 v2...
```

批量查询

```
mget k1 k2 
```

删除数据

```
del key
```

查询所有的 key

```
keys *
```

清除当前数据库

```
flushdb
```

清除所有数据库

```
flushall
```

查看 key 是否存在

```
exists key
```

设置有效期

```
expire key 10
```

查看有效期

```
ttl key
```



# Redis 数据类型

> String

追加字符串

```
append key value
```

查看字符串长度

```
strlen key
```

自增

```
incr key
```

递减

```
decr key
```

指定递增长度

```
incrby k v
```

指定递减长度

```
decrby k v
```

字符串截取

```
getrange k start end
```

修改局部字段

```
setrange k start v
```



> List

从左侧添加

```
lpush k v...
```

从右侧添加

```
rpush k v...
```

取值

```
lrange k start end
```

删除，左侧移除

```
lpop k
```

右侧移除

```
rpop k
```

通过下标获取值

```
lindex k index
```

删除集合中指定的值，count 是删除的个数

```
lrem k count v
```

通过下标修改集合中的值

```
lset k index v
```

获取长度

```
llen k
```

截取list

```
ltrim k start end
```

查看集合是否存在

```
exists k
```



> Set

添加数据

```
sadd k v
```

查询数据

```
smembers k
```

判断集合中是否存在某个值

```
sismember k v
```

获取集合长度

```
scard k
```

删除元素

```
srem k v1 v2...
```

随机取值

```
srandmember k
```



> Hash

存值

```
hset hash k1 v1 k2 v2
```

取值

```
hget hash k1
```

存多个值

```
hmset hash k1 a k2 b k3 c
```

取多个值

```
hmget hash k1 k2 k3
```

取所有值

```
hgetall hash
```

删除数据

```
hdel hash k1 k2
```

获取长度

```
hlen k
```

判断集合中是否存在某个值

```
hexists hahs k
```

获取集合中所有 key

```
hkeys hash
```

获取集合中所有 value

```
hvals hash
```



> Zset

添加数据

```
zadd set index v
```

查询数据

```
zrange set 0 -1
```

升序查询

```
zrangebyscore score -inf +inf withscores
```

降序查询

```
zrevrange score 0 -1 withscores
```

删除数据

```
zrem score jack
```

 

# Spring Boot 整合 Redis

Spring Data Redis

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>

<!-- Swagger -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

```yaml
spring:
  redis:
    database: 0
    host: 192.168.248.138
    port: 6379
```

```java
package com.southwind.entity;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;
import java.util.Date;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Student implements Serializable {
    private Integer id;
    private String name;
    private Double score;
    private Date birthday;
}
```

```java
package com.southwind.controller;

import com.southwind.entity.Student;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.web.bind.annotation.*;

@RestController
public class StudentController {

    @Autowired
    private RedisTemplate redisTemplate;

    @PostMapping("/set")
    public void set(@RequestBody Student student){
        this.redisTemplate.opsForValue().set("stu", student);
    }

    @GetMapping("/get/{key}")
    public Student get(@PathVariable("key") String key){
        return (Student) this.redisTemplate.opsForValue().get(key);
    }

    @PutMapping("/put")
    public void update(@RequestBody Student student){
        this.redisTemplate.opsForValue().set("stu", student);
    }

    @DeleteMapping("/delete/{key}")
    public Boolean delete(@PathVariable("key") String key){
        this.redisTemplate.delete(key);
        return this.redisTemplate.hasKey(key);
    }
}
```

```java
package com.southwind.configuration;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
public class SwaggerConfiguration {
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.southwind"))
                .build().apiInfo(new ApiInfoBuilder()
                        .title("Redis测试")
                        .description("测试")
                        .version("V1.0")
                        .build());
    }
}
```

字符串

```java
@PostMapping("/string")
public String string(){
    String str = "Hello World";
    this.redisTemplate.opsForValue().set("str", str);
    return (String) this.redisTemplate.opsForValue().get("str");
}
```

List

```java
@PostMapping("/list")
public void list(){
    ListOperations<String,String> list = redisTemplate.opsForList();
    list.leftPush("list", "Hello");
    list.leftPush("list", "World");
    list.leftPush("list","Java");
    list.rightPush("list", "1");
    list.rightPush("list", "2");
    list.rightPush("list", "3");
}
```

Set

```java
@PostMapping("/setadd")
public void setadd(){
    SetOperations<String,String> set = this.redisTemplate.opsForSet();
    set.add("set", "Hello");
    set.add("set", "World");
    set.add("set", "Java");
}
```

Zset

```java
@PostMapping("/zset")
public void zset(){
    ZSetOperations<String,String> set = this.redisTemplate.opsForZSet();
    set.add("zset", "Hello",1);
    set.add("zset", "World",2);
    set.add("zset", "Java",3);
}
```

Hash

```java
@PostMapping("/hash")
public void hash(){
    HashOperations<String,String,String> hash = this.redisTemplate.opsForHash();
    hash.put("hash", "id", "1");
    hash.put("hash", "name", "tom");
    hash.put("hash", "age","22" );
}
```

