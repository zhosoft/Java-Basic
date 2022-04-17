#  基本配置

（1）、创建maven工程，在pom.xml文件中添加如下依赖：

```xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.7</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.26</version>
</dependency>
```

使用lombok插件，添加如下依赖：

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.20</version>
</dependency>
```

（2）、写mybatis-config.xml配置文件

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!-- 打印sql日志 -->
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
    <!-- 数据源 -->
    <environments default="dev">
        <environment id="dev">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatisplus"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>

    <!-- 注册UserMapper.xml -->
    <mappers>
        <mapper resource="com/zhosoft/mapper/xml/userMapper.xml" />
        <mapper resource="com/zhosoft/mapper/xml/companyMapper.xml" />
    </mappers>
 </configuration>
```

（3）、写MyBatisUtil工具类：

```java
/**
 * 工具类
 */
public class MyBatisUtil {

    private static ThreadLocal<SqlSession> threadLocal = new ThreadLocal<SqlSession>();
    private static SqlSessionFactory sqlSessionFactory;

    /**
     * 加载resources/mybatis-config.xml配置文件
     */
    static {
        try {
//            Reader resource = Resources.getResourceAsReader("mybatis-config.xml");
            InputStream resource = MyBatisUtil.class.getClassLoader().getResourceAsStream("mybatis-config.xml");
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(resource);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 禁止外部通过new创建
     */
    private MyBatisUtil() {
    }

    /**
     * 获取SqlSession对象
     * @return sqlSession
     */
    public static SqlSession getSqlSession() {
        //从线程中获取SqlSession对象
        SqlSession sqlSession = threadLocal.get();
        if (sqlSession == null) {
            sqlSession = sqlSessionFactory.openSession(true);
            threadLocal.set(sqlSession);
        }
        //返回sqlSession对象
        return sqlSession;
    }

    /**
     * 关闭SqlSession对象
     */
    public static void closeSqlSession() {
        SqlSession sqlSession = threadLocal.get();
        if (sqlSession != null) {
            //关闭SqlSession对象
            sqlSession.close();
            //分开当前线程与SqlSession对象的关系（让GC尽早回收）
            threadLocal.remove();
        }
    }
}
```

# 底层实现

MyBatis最核心的功能就是动态代理；

MyBatis的使用：

- 自定义接口；
- 创建接口对应的SQL;
- 加载MyBatis，获取动态代理对象，进行操作；





动态代理

jenkins



https://gitee.com/roncoocom/roncoo-education?_from=gitee_search#%E9%A2%86%E8%AF%BE%E6%95%99%E8%82%B2%E7%B3%BB%E7%BB%9Froncoo-education%E7%A0%81%E4%BA%91%E5%9C%B0%E5%9D%80--github%E5%9C%B0%E5%9D%80--codechina